{#each allMessages as message}
{#if message.sender != email}
<div style="background-color: lightgrey; 
width:300px; padding: 5px 5px 5px 5px; 
margin-left:20px; margin-top:10px; border-radius:10px">
{message.message}
<p style="font-size:8px;">{message.sender}</p>
<p style="font-size:8px;">{message.date}</p>
</div>
{:else}
<div style="background-color: lightblue; 
width:300px; padding: 5px 5px 5px 5px; 
margin-left:350px; margin-top:10px; border-radius:10px">
{message.message}
<p style="font-size:8px;">{message.sender}</p>
<p style="font-size:8px;">{message.date}</p>
</div>
{/if}
{/each}

<div style = "position:fixed; bottom:72px; width:97%">

<input class="input input-block" placeholder="Type here..." 
style = "bottom:0px; margin: 10px 10px 10px 10px;|" 
bind:value={bindTextbox}/>

<button class="btn btn-primary btn-sm"
style = "position:absolute; bottom:14px; right:-5px;"
on:click={sendMessage}>
SEND</button>

</div>

<BottomBar />

<script>

//TODO: PREVENT PAGE FROM UPDATING IF OTHERS
//POST TO THE TABLE NOT RELATED TO THIS CHAT ID

//COMPLETE THE CHAT LIST PAGE

//EMAN: FIX THE DATE THING

  import { onDestroy, onMount } from "svelte";
  import {createClient} from "@supabase/supabase-js";
  import { goto } from "$app/navigation";
  import BottomBar from "$lib/BottomBar.svelte";

  const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")

  let allMessages = [];
  let chatID;
  let bindTextbox;
  let email;
  let fname;
  let username;
  let userID;
  let listenforMessages;


  onMount(()=>
  {
    getChats();
    autoLogin();

    //subscribe to the table, listen for updates
    //then run the getchats function again :)

    listenforMessages = supabase
    .channel("messageHistory")
    .on(
        'postgres_changes',
        {
            event: "*",
            schema: "public",
            table: "messageHistory"
        },
        (payload) => {getChats();}
    )
    .subscribe();

  });

  onDestroy(()=>
  {
    listenforMessages.unsubscribe();
  });

  async function getChats()
  {
    allMessages = [];
    //we get the chat id from the url parameter
    let currentURL = window.location.href; //returns the url
    //create a url object
    let url = new URL(currentURL);
    
    //get the url params from the object
    let params = new URLSearchParams(url.search);

    //get the value of the chatid parameter
    chatID = params.get("chatid")

    let getMessages = await supabase
    .from("messageHistory")
    .select()
    .match({chat_id: chatID})

    for(let i = 0; i < getMessages.data.length; i++)
    {

    allMessages =
        [ ...allMessages,
                {
                    "date": getMessages.data[i].created_at,
                    "sender" : getMessages.data[i].sender,
                    "message": getMessages.data[i].message
                }
            ]
    }
    
  }

  async function sendMessage()
  {
    let sendMessage = await supabase
    .from("messageHistory")
    .insert({chat_id: chatID, sender: email, message: bindTextbox})

    bindTextbox = "";
  }

  async function autoLogin()
    {
        if(localStorage.getItem("token")
        && localStorage.getItem("email"))
        {
            let checkDetails = await supabase
            .from("users")
            .select()
            .match({email:localStorage.getItem("email"), 
            sessionToken: localStorage.getItem("token")})


            if(checkDetails.data.length > 0)
            {
               
                let getFirstName = await supabase
                .from("users")
                .select()
                .match({email: localStorage.getItem("email")})

                email = getFirstName.data[0].email;
                fname = getFirstName.data[0].FirstName;
                username = getFirstName.data[0].username;
                userID = getFirstName.data[0].id;

            } else
            {
                goto("/")
            }
        } else
        {
            
            goto("/")
        }
    }
</script>
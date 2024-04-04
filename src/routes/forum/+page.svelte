<!--FIX DATE AND ENABLE LIKE AND COMMENT COUNTER-->
<!--ALLOW LIKING AND COMMENTING AND VIEW COMMENTS-->
<!--MODERATOR CAPABILITY-->



<div style="position:relative; width:400px; max-width:90%; left:50%; 
transform:translateX(-50%);">

<input class="input input-solid max-w-full" placeholder="Heading" 
style="position:relative;  
    margin-top:20px;" bind:value={headingString}/>

    <textarea class="textarea textarea-solid max-w-full" placeholder="Say something..." 
    rows="3" id="message" style="position:relative;  
    margin-top:20px;" bind:value={contentString}></textarea>

    <button class="btn btn-primary" on:click={submitPost}>Post</button>

</div>
<br>

{#if showPosts}
{#each posts as obj}
<div style="border: color: black; border-width:5px; width: 400px; max-width:90%;
position:relative; left:50%; transform:translateX(-50%); margin-top:10px; 
border-radius:15px; padding: 10px 10px 10px 10px">

<p style="font-size:24px;"><b>{obj.header}</b></p>
<br>
<p style="font-size:16px;">{obj.content}</p>
<br>
<p style="font-size:16px;">{obj.date}</p>
<br>
<p style="font-size:16px;">Post by {obj.username}</p>

</div>
{/each}
{/if}
<br><br><br><br><br>


{#if ispostSuccessful == true}
<div class="alert alert-success" style="position:absolute; 
width:300px; top:10px; right:10px;">
	<svg width="48" height="48" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
		<path fill-rule="evenodd" clip-rule="evenodd" d="M24 4C12.96 4 4 12.96 4 24C4 35.04 12.96 44 24 44C35.04 44 44 35.04 44 24C44 12.96 35.04 4 24 4ZM18.58 32.58L11.4 25.4C10.62 24.62 10.62 23.36 11.4 22.58C12.18 21.8 13.44 21.8 14.22 22.58L20 28.34L33.76 14.58C34.54 13.8 35.8 13.8 36.58 14.58C37.36 15.36 37.36 16.62 36.58 17.4L21.4 32.58C20.64 33.36 19.36 33.36 18.58 32.58Z" fill="#00BA34" />
	</svg>
	<div class="flex flex-col">
		<span>Success</span>
		<span class="text-content2">Your post was submitted successfully!</span>
	</div>
</div>
{/if}

<BottomBar />
<script>
    import BottomBar from "$lib/BottomBar.svelte"
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import {createClient} from "@supabase/supabase-js";
  import { get } from "svelte/store";

    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")
  
    let username;
    let fname;
    let headingString;
    let contentString;
    let ispostSuccessful = false;
    let posts = [];
    let showPosts = false;

    onMount(()=>
    {
       autoLogin();

    }
    );

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

                fname = getFirstName.data[0].FirstName;
                username = getFirstName.data[0].username;
              
                fetchPosts();

            } else
            {
                goto("/")
            }
        } else
        {
            
            goto("/")
        }

        
    }

async function submitPost()
{
    showPosts = false;
    //show alert and cancel if heading or content length == 0;
    let insertPost = await supabase
    .from("forum")
    .insert({user: username, heading: headingString, 
        content: contentString})

        ispostSuccessful = true;
        setTimeout(() => {
           ispostSuccessful = false; 
        }, 2000);

        headingString = "";
        contentString = "";
        posts = [];
        fetchPosts();
}

async function fetchPosts()
{
    let getPosts = await supabase
    .from("forum")
    .select()
    .order('id', {ascending: false})

   

    for(let i = 0; i < getPosts.data.length; i++)
    {
        posts.push(
        {
            "header": getPosts.data[i].heading,
            "content": getPosts.data[i].content,
            "username": getPosts.data[i].user,
            "likes": getPosts.data[i].likeCounter,
            "comments": getPosts.data[i].commentsCounter,
            "date": getPosts.data[i].created_at
        });

    }

    showPosts = true; 

}
    </script>
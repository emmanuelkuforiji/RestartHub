<div class="background">
{#if showModal}
<div style="position:fixed; width:100%; height:100%; background-color:rgba(29, 51, 64, 0.9);
 z-index:1">
    <div style="position:fixed; top:100px; left:50%; transform:translateX(-50%); 
    width:400px; max-width:96%; height:400px; border: 5px solid black; background-color:white;">
    <img src = "close.svg"
                style="cursor:pointer; display:inline-block; width:32px; 
                height:32px; float:right;" on:click={function(){showModal = false; posts = []; showPosts = false; fetchPosts();}}>
    <div style="position:relative; width:100%; 
    height:300px; overflow-y:scroll; word-wrap: break-word; ">

        {#if showComments}
        {#each commentsArray as obj}

        {#if username == obj.username}
        <span style="z-index:2; cursor:pointer; position:relative; float:right; right:35px; font-size:18px;"
        on:click={function(){deleteComment(obj.uniqueID, obj.postID)}}>
            <img src = "delete.svg" style="width:32px; height:32px;"></span>
        {/if}

        <div style="position: relative; width:320px; left:50%; transform:translateX(-50%); 
                    border-color:#cccaca; border-width:1px; margin-bottom:8px; border-radius:8px;
                    padding:5px 40px 5px 5px; text-align:justify; z-index:1">
        {obj.content}
        <br>
        <p style="font-size:12px" ><b>by {obj.username}</b></p>
        <p style="font-size:12px" ><b>on {obj.date}</b></p>
        </div>

        {/each}
        {/if}

    </div>
            <span style="position:absolute; width:300px; left:50%; 
            transform:translateX(-50%); bottom:5px;">
                <input bind:value={commentContent} class="input" placeholder="Comment..." style="width: 200px;"/>
                <button class="btn btn-primary" 
                on:click={function(){postComment(currentlySelectedPost)}}>Submit</button>
            </span>
    </div>
</div>
{/if}

<div id="post-container">
    <input class="input input-solid max-w-full post-input" placeholder="Heading" bind:value={headingString}>

    <textarea class="textarea textarea-solid max-w-full post-textarea" placeholder="Say something..." rows="3" id="message" bind:value={contentString}></textarea>

    <button class="btn btn-success post-btn" on:click={submitPost}>Post</button>
</div>
<br>

{#if showPosts}
{#each posts as obj}
    <div class="post">
        <p class="post-header"><b>{obj.header}</b>
            {#if username == obj.username}
        <span style="cursor:pointer; position:relative; float:right; font-size:18px;"
        on:click={function(){deletePost(obj.uniqueID)}}>
            <img src = "delete.svg" style="width:32px; height:32px;"></span>
        {/if}
    </p>
        <br>
        <p class="post-content">{obj.content}</p>
        <br>
        <p class="post-date">{obj.date}</p>
        <p class="post-username">Post by {obj.username}</p>
        <p style="display: inline-block; color:black;">
            <img src = "heart.svg" on:click={function(){likePost(obj.uniqueID, obj.likes)}}
            style="cursor:pointer; display:inline-block; width:32px; 
            height:32px;">
            {obj.likes} </p>
        <p style="display: inline-block; color:black;">
            <img src="comments.svg"  style="cursor:pointer; display:inline-block; width:32px; height:32px;"
            on:click={function(){curentpostComments = obj.comments; currentlySelectedPost = obj.uniqueID; showModal = true; fetchComments(obj.uniqueID)}}>
            {obj.comments}</p>
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
</div>

<BottomBar />

<style>
    .background {
        background: linear-gradient(to bottom, #bdc3c7 0%, #2c3e50 100%);
        height: 100%;
    }

    #post-container {
        position: relative;
        width: 400px;
        max-width: 90%;
        left: 50%;
        transform: translateX(-50%);
    }
    
    .post-input, .post-textarea {
        position: relative;
        margin-top: 20px;
    }
    
    .post {
        border: 5px solid black;
        width: 400px;
        max-width: 90%;
        position: relative;
        left: 50%;
        transform: translateX(-50%);
        margin-top: 10px;
        border-radius: 15px;
        padding: 10px;
    }
    
    .post-header {
        font-size: 24px;
        color: black;
        font-weight: bold;
    }
    
    .post-content {
        font-size: 16px;
        color: black;
    }
    
    .post-date, .post-username {
        font-size: 12px;
        font-weight: bold;
        color: black;
    }

</style>
    

<script>
    import BottomBar from "$lib/BottomBar.svelte"
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import {createClient} from "@supabase/supabase-js";
  import { get } from "svelte/store";

    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")
  
    let username;
    let fname;
    let userID;
    let headingString;
    let contentString;
    let ispostSuccessful = false;
    let posts = [];
    let showPosts = false;
    let showComments = false;
    let currentlySelectedPost;
    let commentContent = "";
    let curentpostComments = "";
    let showModal = false;

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
                userID = getFirstName.data[0].id;
              
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
        }, 2500);

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
            "uniqueID": getPosts.data[i].id,
            "header": getPosts.data[i].heading,
            "content": getPosts.data[i].content,
            "username": getPosts.data[i].user,
            "likes": getPosts.data[i].likeCounter,
            "comments": getPosts.data[i].commentsCounter,
            "date": dateTimeLocaliser(getPosts.data[i].created_at)
        });

    }

    showPosts = true; 

}

async function deletePost(uniqueID)
{
    
    let deletePost = await supabase
    .from("forum")
    .delete()
    .match({id: uniqueID})

    posts = [];
    showPosts = false;
    fetchPosts();
    
}

async function likePost(uniqueID, currentLikes)
{
    let getLikes = await supabase
    .from("likes")
    .select()
    .match({postID: uniqueID, userLiked: userID})

    if(getLikes.data.length > 0)
    {
        currentLikes = parseInt(currentLikes)
        currentLikes--;

        let likePost = await supabase
        .from("forum")
        .update({likeCounter: currentLikes})
        .match({id: uniqueID})

        let updateLikes = await supabase
        .from("likes")
        .delete()
        .match({postID: uniqueID, userLiked: userID})

        posts = [];
        showPosts = false;
        fetchPosts();

    } else
    {
        currentLikes = parseInt(currentLikes)
        currentLikes++;

        let likePost = await supabase
        .from("forum")
        .update({likeCounter: currentLikes})
        .match({id: uniqueID})

        let updateLikes = await supabase
        .from("likes")
        .insert({postID: uniqueID, userLiked: userID})

        posts = [];
        showPosts = false;
        fetchPosts();
    }
    
}

async function postComment(UID)
{
    posts = [];
    
    let getPostsCounter = await supabase
    .from("forum")
    .select()
    .match({id: UID})

    let postCounter = getPostsCounter.data[0].commentsCounter

    postCounter++;

    let updateCounter = await supabase
    .from("forum")
    .update({commentsCounter: postCounter})
    .match({id: UID})


    let insertComment = await supabase
    .from("comments")
    .insert({postID: UID, userID: userID, content: commentContent})
    commentContent = "";
    

    commentsArray = [];
    showComments = false;
    fetchComments(currentlySelectedPost);

    
    showPosts = false;
    fetchPosts();

}

let commentsArray = [];

async function fetchComments(UID)
{
    showComments = false;
    commentsArray = [];
    let getComments = await supabase
    .from("comments")
    .select()
    .match({postID: UID})
    .order('created_at', {ascending: false})
   

    for(let i = 0; i < getComments.data.length; i++)
    {
        let getUsername = await supabase
        .from("users")
        .select()
        .match({id: getComments.data[i].userID})
        
        commentsArray.push(
        {
            "postID": getComments.data[i].postID,
            "uniqueID": getComments.data[i].id,
            "content": getComments.data[i].content,
            "username": getUsername.data[0].username,
            "date": dateTimeLocaliser(getComments.data[i].created_at)
        });

    }


    showComments = true;
}


async function deleteComment(uniqueID, postID)
{
    posts = [];
    let getPostsCounter = await supabase
    .from("forum")
    .select()
    .match({id: postID})

    let postCounter = getPostsCounter.data[0].commentsCounter

    postCounter--;

    let updatePostsCounter = await supabase
    .from("forum")
    .update({commentsCounter: postCounter})
    .match({id:postID})

    let deletePost = await supabase
    .from("comments")
    .delete()
    .match({id: uniqueID})

    commentsArray = [];
    showComments = false;
    fetchComments(currentlySelectedPost);

   
    showPosts = false;
    fetchPosts();
    
}


function dateTimeLocaliser(date)
{
    const timestamp = new Date(date);

    const options = {
    hour12: false,
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
    day: '2-digit',
    month: '2-digit',
    year: '2-digit'
    };

    const localTimeString = timestamp.toLocaleString(undefined, options);

    return localTimeString; // Output in HH:MM:SS DD-MM-YY format in the local timezone
}

    </script>
<div class="background">
<br>
<p class="uName">Hey, {fname}!</p>
<br>

<div class="tile2" on:click={goToAboutUs}>
    <p class="tile2Header">About Us
        <img 
        src="about.svg"
        class="tile2Image">
    </p>
    
    <p class="tile2subHeading">Find out more about us.</p>
   
</div>

<div class="tile3" on:click={goToJobListings}>
    <p class="tile3Header">Job Listings
        <img 
        src="job-search.svg"
        class="tile3Image">
    </p>
    
    <p class="tile3subHeading">Find jobs that suit your skills.</p>
   
</div>

<div class="tile4" on:click={goToMentorship}>
    <p class="tile4Header">Mentorship
        <img 
        src="mentorship.svg"
        class="tile4Image">
    </p>
    
    <p class="tile4subHeading">Get involved in the mentoring scheme.</p>
   
</div>
 
<div class="tile5" on:click={goToHealthInfo}>
    <p class="tile5Header" >General Health Information
        <img 
        src="health.svg"
        class="tile5Image">
    </p>
    
    <p class="tile5subHeading">General tips and information about maintaining good health.</p>
   
</div>

<div class="tile6" on:click={goToEducation}>
    <p class="tile6Header">Educational and Training Information
        <img 
        src="education.svg"
        class="tile6Image">
    </p>
    
    <p class="tile6subHeading">Find out about the types of education available to you.</p>
   
</div>
</div>

<BottomBar />

<style>

.background {
    background: linear-gradient(to bottom, #bdc3c7 0%, #2c3e50 100%);
    height: 950px;
}

.uName
{
    position:relative;
    left:50%;
    transform: translateX(-50%);
    width: 90%;
    font-size: 28px;
    font-weight: bold;
    color: white;
}

.tile2
{
    position:relative;
    left:50%;
    transform: translateX(-50%);
    width: 98%;
    height: 150px;
    background: linear-gradient(to bottom right, #dec0f1 0%, #b79bec 100%);
    border-radius: 20px;
    margin-top: 4px;
    cursor: pointer;
}

.tile2Header
{
    font-size: 24px;
    padding-left: 30px;
    padding-top: 18px;
    color:white;
}

.tile2subHeading
{
    font-size: 14px;
    padding-left: 30px;
    margin-top: 10px;
    color:white;
}

.tile2Image
{
    display:inline-block; 
    width: 84px; 
    height:84px;
    float:right;
    margin-right:20px;
    ;
}


.tile3
{
    position:relative;
    left:50%;
    transform: translateX(-50%);
    width: 98%;
    height: 150px;
    background: linear-gradient(to bottom right, #e2cfea 0%, #a496ce 100%);
    border-radius: 20px;
    margin-top: 4px;
    cursor: pointer;
}

.tile3Header
{
    font-size: 24px;
    padding-left: 30px;
    padding-top: 18px;
    color:white;
}

.tile3subHeading
{
    font-size: 14px;
    padding-left: 30px;
    margin-top: 10px;
    color:white;
}

.tile3Image
{
    display:inline-block; 
    width: 84px; 
    height:84px;
    float:right;
    margin-right:20px;
    ;
}

.tile4
{
    position:relative;
    left:50%;
    transform: translateX(-50%);
    width: 98%;
    height: 150px;
    background: linear-gradient(to bottom right, #b8e0f4 0%, #86c4e7 100%);
    border-radius: 20px;
    margin-top: 4px;
    cursor: pointer;
}

.tile4Header
{
    font-size: 24px;
    padding-left: 30px;
    padding-top: 18px;
    color:white;
}

.tile4subHeading
{
    font-size: 14px;
    padding-left: 30px;
    margin-top: 10px;
    color:white;
}

.tile4Image
{
    display:inline-block; 
    width: 84px; 
    height:84px;
    float:right;
    margin-right:20px;
    ;
}

.tile5
{
    position:relative;
    left:50%;
    transform: translateX(-50%);
    width: 98%;
    height: 150px;
    background: linear-gradient(to bottom right, #ffd2a5 0%, #f76b6b 100%);
    border-radius: 20px;
    margin-top: 4px;
    cursor: pointer;
}

.tile5Header
{
    font-size: 24px;
    padding-left: 30px;
    padding-top: 18px;
    color:white;
}

.tile5subHeading
{
    font-size: 14px;
    padding-left: 30px;
    margin-top: 10px;
    color:white;
}

.tile5Image
{
    display:inline-block; 
    width: 84px; 
    height:84px;
    float:right;
    margin-right:20px;
    ;
}

.tile6
{
    position:relative;
    left:50%;
    transform: translateX(-50%);
    width: 98%;
    height: 150px;
    background: rgb(2,0,36);
    background: linear-gradient(90deg, rgb(218, 126, 76) 0%, rgb(240, 209, 163) 0%, rgb(217, 133, 5) 92%);
    border-radius: 20px;
    margin-top: 4px;
    margin-bottom: 90px;
    cursor: pointer;
}

.tile6Header
{
    font-size: 24px;
    padding-left: 30px;
    padding-top: 18px;
    color:white;
}

.tile6subHeading
{
    font-size: 14px;
    padding-left: 30px;
    margin-top: 10px;
    color:white;
}

.tile6Image
{
    display:inline-block; 
    width: 84px; 
    height:84px;
    float:right;
    margin-right:20px;
    ;
}
</style>

<script>

function goToAboutUs() {
    goto('/about_us'); 
}

function goToJobListings() {
    goto('/jobs'); 
}

function goToMentorship() {
    goto('/mentorship'); 
}

function goToHealthInfo() {
    goto('/general_health_information'); 
}

function goToEducation() {
    goto('/educational_and_training_information'); 
}


    import BottomBar from "$lib/BottomBar.svelte"
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import {createClient} from "@supabase/supabase-js";

    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")
    
    let fname = "";

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

            console.log(checkDetails.data)

            if(checkDetails.data.length > 0)
            {
               
                let getFirstName = await supabase
                .from("users")
                .select()
                .match({email: localStorage.getItem("email")})

                fname = getFirstName.data[0].FirstName;

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
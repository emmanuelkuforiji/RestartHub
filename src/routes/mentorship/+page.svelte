<div class="background">
<div class="mentorship-intro">
    <h2>Mentorship Program</h2>
    <br>
    <p>At RestartHub, we believe in the power of shared experience and guidance. Our mentorship program connects ex-offenders with mentors who have navigated similar paths back into society. Whether you're looking to share your expertise or seeking wisdom on your reintegration journey, our platform facilitates these transformative relationships.</p>
    <p>To become a mentor, select skills that you excel in and are passionate about passing on. To find a mentor, choose areas where you seek growth. Our platform will guide you through the process, making meaningful connections that foster personal and professional development.</p>
  </div>  
<br>
<button class="btn btn-solid-primary" on:click={() => {        
    findMentor = false; 
    becomeMentorBool = true;
    listSkills();}}>Become a Mentor</button>
<br>
<button class="btn btn-solid-secondary" on:click={() => {        
    findMentor = true; 
    becomeMentorBool = false;
    listSkills();}}>Search for a Mentor</button>

{#if showAlert}
<div class="alert alert-success">
	<svg width="188" height="188" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
		<path fill-rule="evenodd" clip-rule="evenodd" d="M24 4C12.96 4 4 12.96 4 24C4 35.04 12.96 44 24 44C35.04 44 44 35.04 44 24C44 12.96 35.04 4 24 4ZM18.58 32.58L11.4 25.4C10.62 24.62 10.62 23.36 11.4 22.58C12.18 21.8 13.44 21.8 14.22 22.58L20 28.34L33.76 14.58C34.54 13.8 35.8 13.8 36.58 14.58C37.36 15.36 37.36 16.62 36.58 17.4L21.4 32.58C20.64 33.36 19.36 33.36 18.58 32.58Z" fill="#00BA34" />
	</svg>
	<div class="flex flex-col">
		<span>Saved</span>
		<span class="text-content2">Your skills have been saved</span>
	</div>
</div>
{/if}

{#if becomeMentorBool}
<div class="mentorship-container">
    <p class="bmHeading">Become a mentor</p>
    <p class="text1">Pick up to five skills that you excel in and would like to help a mentee with.</p>
    <div class="controls">
        <select style="width:200px; display:inline-block;" bind:value={currentlyselectedskill}>
            {#each allskills as s}
                <option value="{s.skilltype}">{s.skilltype}</option>
            {/each}
        </select>
        <button class="btn btn-primary" on:click={addSkill}>Add</button>
        <button class="btn {editMode ? 'btn-warning' : 'btn-secondary'}" on:click={toggleEditMode}>
            {#if editMode}Done{:else}Edit{/if}
        </button>
    </div>
    <div class="skill-set">
        {#each currentSkills as a}
    <button 
        class="skill-button {selectedSkillsForRemoval.has(a.currentskill) ? 'selected' : ''}"
        on:click={() => toggleSkillSelection(a.currentskill)}>
        {a.currentskill}
    </button>
{/each}
    </div>
    <div class="action-buttons">
        {#if editMode}
            <button class="btn btn-danger" on:click={removeSelectedSkills}>Remove Selected</button>
        {/if}
        <button class="btn btn-primary" on:click={saveMentor}>Save</button>
    </div>
</div>
{/if}

{#if findMentor}
<div class="mentorship-container">
    <p class="bmHeading">Find a mentor</p>
    <p class="text1">Pick up to five skills that you would like a mentor to help you improve.</p>
    <div class="controls">
        <select style="width:200px; display:inline-block;" bind:value={currentlyselectedskill}>
            {#each allskills as s}
                <option value="{s.skilltype}">{s.skilltype}</option>
            {/each}
        </select>
        <button class="btn btn-primary" on:click={addSkill}>Add</button>
        <button class="btn {editMode ? 'btn-warning' : 'btn-secondary'}" on:click={toggleEditMode}>
            {#if editMode}Done{:else}Edit{/if}
        </button>
    </div>
    <div class="skill-set">
        {#each currentSkills as a}
            <button 
                class="skill-button {editMode && selectedSkillsForRemoval.has(a.currentskill) ? 'selected' : ''}"
                on:click={editMode ? () => toggleSkillSelection(a.currentskill) : null}>
                {a.currentskill}
            </button>
        {/each}
    </div>
    <div class="action-buttons">
        {#if editMode}
            <button class="btn btn-danger" on:click={removeSelectedSkills}>Remove Selected</button>
        {/if}
        <button class="btn btn-primary" on:click={findMentorFunc}>Search</button>
    </div>
</div>
    {#if searchPerformed}
        {#if eligibleMentors.length > 0}
            {#each eligibleMentors as i}
                <p class="text5">{i.first} may be a suitable mentor for you. Contact them via:</p>
                <button class="btn" style="margin-left:10px"
                on:click={function(){createChat(i.email)}}>Live Chat</button>
            {/each}
        {:else}
            <p class="text5">Sorry, there are no mentors that can help you with these skills at the moment. Please try again later.</p>
        {/if}
    {/if}

{/if}

<div class="space">
    <br>
</div>
</div>
<style>
    .background {
        background: linear-gradient(to bottom, #bdc3c7 0%, #2c3e50 100%);
        height: 1500px;
    }

    .mentorship-intro h2{
        font-size: 28px;
        font-weight: bold;
        text-align: center;
        color: white;
    }

    .mentorship-intro p{
        font-size: 16px;
        width: 95%;
        margin-left: 8px;
        color: white;
    }

    .mentorship-container {
    position: relative; 
    width:1400px; 
    max-width:98%; 
    background-color:  #1F2833;
    margin: 0 auto;
    margin-top: 2px;
    padding: 10px;
    box-sizing: border-box;
    }

    .bmHeading {
        font-weight: bold;
        font-size: 24px;
        color: white;
    }

    .text1 {
        color: white;
    }

    .text4 {
        color: blue;
        font-style: underline;
        margin-left: 20px;
        max-width: 98%;
    }

    .text5 {
        margin-left: 10px;
        width: 98%;
        color: white;
    }

    .controls, .skill-set, .action-buttons {
        margin: 0 auto;
    }

    .skill-button {
        padding: 6px 12px;
        margin-right: 10px;
        margin-bottom: 10px;
        margin-top: 10px;
        border: 1px solid transparent;
        border-radius: 20px;
        background-color: #11ca33;
        color: white;
        font-size: 14px;
        cursor: pointer;
        transition: background-color 0.3s, border 0.3s;
    }

    .skill-button.editable:hover {
        background-color: #34495E;
    }

    .skill-button.selected {
        background-color: #ECC94B; 
        border-color: #ECC94B;
    }

    .action-buttons {
        display: flex;
        justify-content: space-between;
        margin-top: 20px;
    }

    .space {
        margin-bottom: 80px;
    }


</style>

<BottomBar />

<script>

    import BottomBar from "$lib/BottomBar.svelte"
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import {createClient} from "@supabase/supabase-js";

    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")
  
    let fname;
    let username;
    let userID;
    let email;

    let becomeMentorBool = false;
    let findMentor = false;
    let currentlyselectedskill = "";
    let searchPerformed = false;
    let editMode = false;
    let showAlert = false;
    let selectedSkillsForRemoval = new Set();


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

    let allskills = [];
    async function listSkills()
    {
        allskills = [];
        currentSkills = [];

        let getskills = await supabase
        .from("skills")
        .select()

        for(let i = 0; i < getskills.data.length; i++)
        {
            allskills =
            [ ...allskills,
                {
                'skilltype': getskills.data[i].skill
                }
            ]
        }

    }

    let currentSkills = [];
    function addSkill()
    {
        if(currentSkills.length == 5)
        {
            return;
        }

        if(currentSkills.some(item => item.currentskill === currentlyselectedskill))
        {

        } else
        {
            currentSkills = 
        [
            ...currentSkills,
            {
                "currentskill" : currentlyselectedskill
            }];
     
        }

    }

    async function saveMentor()
    {
        if(currentSkills.length > 0)
        {
            let makeMentor = await supabase
            .from("users")
            .update({isMentor: true})
            .match({username: username})
        }

        let listofskills = "";
        for(let i = 0;  i < currentSkills.length; i++)
        {
           listofskills += currentSkills[i].currentskill + ", ";
        }

        becomeMentorBool = false;
        timeSuccess();

        let addSkills = await supabase
            .from("users")
            .update({skills: listofskills})
            .match({username: username})
            location.reload();
    
    }

    function timeSuccess() {
        showAlert = true;
        setTimeout(() => {
        showAlert = false; // Hide the alert
        window.location.reload(); // Reload the page
    }, 50000); 
    }

    let eligibleMentors = [];

    async function findMentorFunc()
    {
        searchPerformed = true;
        let findSkills = await supabase
            .from("users")
            .select()
            .match({isMentor: true})

            let arrayofskills = [];
            let userID = [];
            eligibleMentors = [];

            let selectskills = [];
            for(let i = 0;  i < currentSkills.length; i++)
            {
            selectskills.push(currentSkills[i].currentskill)
            }

            let collectUsernames = [];

        for(let i = 0; i < findSkills.data.length; i++)
        {
            
            arrayofskills = [];

            arrayofskills = findSkills.data[i].skills.split(",")
            for(let x = 0; x < arrayofskills.length; x++)
            {
                arrayofskills[x] = arrayofskills[x].trim();
            }

            for(let a = 0; a < arrayofskills.length; a++)
            {

                if(username == findSkills.data[i].username)
                {
                    continue;
                }

                if(collectUsernames.includes(findSkills.data[i].username))
            {
                continue;
            }
            collectUsernames.push(findSkills.data[i].username)

            let collectFnames = [];

            for(let a = 0; a < arrayofskills.length; a++)
            {
                if(collectFnames.includes(findSkills.data[i].FirstName))
            {
                continue;
            }
            collectFnames.push(findSkills.data[i].FirstName)

                if(selectskills.includes(arrayofskills[a]))
                {
                    eligibleMentors = [
                        ...eligibleMentors,
                        {
                            "name" : findSkills.data[i].username,
                            "email" : findSkills.data[i].email,
                            "first" : findSkills.data[i].FirstName
                        }
                    ];
                } else
                {
                   console.log("false");
                }
            }
        }

    }  
        
    }

    function toggleEditMode() {
        editMode = !editMode;
        if (!editMode) {
            selectedSkillsForRemoval.clear();
        }
    }

    function toggleSkillSelection(skill) {
    if (selectedSkillsForRemoval.has(skill)) {
        selectedSkillsForRemoval.delete(skill);
    } else {
        selectedSkillsForRemoval.add(skill);
    }
    }

    function removeSelectedSkills() {
        currentSkills = currentSkills.filter(skill => !selectedSkillsForRemoval.has(skill.currentskill));
        selectedSkillsForRemoval.clear(); 
        editMode = false; 
    }

    async function createChat(otherUserEmail)
    {
        const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
        let result = '';
        const charactersLength = characters.length;

        for (let i = 0; i < 30; i++) {
        result += characters.charAt(Math.floor(Math.random() * charactersLength));
        }

        let findExistingConvo = await supabase
        .from("conversations")
        .select()
        .match({user1: email, user2: otherUserEmail})

        if(findExistingConvo.data.length > 0)
        {
            goto("/chatlist");
            return;
        }

        let {insertConvo, error} = await supabase
        .from("conversations")
        .insert({user1: email, user2: otherUserEmail, chat_id: result})

        if(error != null)
        {
            alert("No internet connection detected!");
        } else
        {
            goto("/messenger?chatid=" + result);
        }
    }

</script>
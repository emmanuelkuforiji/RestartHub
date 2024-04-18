<div class="navigation">
    {#each Object.keys(sections) as section}
        <button 
            class:selected={selectedSection === section} 
            on:click={() => selectedSection = section}>
            {sections[section].title}
        </button>
    {/each}
</div>

<div class="content">
    {#if selectedSection}
        <h2>{sections[selectedSection].title}</h2>
        {#each sections[selectedSection].content as item}
            <div class="resource">
                <h3>{item.name}</h3>
                {#if item.url}<a href={item.url} target="_blank">{item.url}</a>{/if}
                <p>{item.details}</p>
            </div>
        {/each}
    {/if}
</div>

<style>
  .page-container {
      max-width: 800px;
      margin: 40px auto;
      padding: 20px;
      padding-bottom: 80px;
      background-color: #ffffff;
      border-radius: 10px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
      font-family: 'Arial', sans-serif;
      position: relative; 
  }

  h1 {
      color: #333;
      font-size: 2rem;
      text-align: center;
      margin-bottom: 1rem;
  }

  .navigation {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      margin-bottom: 20px;
      background: #f2f2f2; 
      padding: 10px;
      border-radius: 10px;
  }

  .navigation button {
      padding: 8px 12px;
      margin: 2px;
      background-color: #e7e7e7; 
      color: #333; 
      border: 2px solid #ddd; 
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s, border-color 0.3s, color 0.3s;
      flex-grow: 1;
      min-width: 120px;
  }

  .navigation button:hover, .navigation button.selected {
      background-color: #ddd; 
      color: #0056b3; 
      border-color: #ccc; 
  }
  .content{
    margin-bottom: 90px;
    margin-left: 3px;
    width: 98%;
  }
  .content h2 {
      color: #0056b3;
      margin: 20px 5px;
      font-size: 1.5rem;
  }

  .resource {
      background: #f9f9f9;
      border-left: 5px solid #0056b3; /* Accent color to tie it with the headings */
      padding: 15px;
      margin-bottom: 15px;
      border-radius: 5px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2); /* Soft shadow for depth */
  }

  .resource h3 {
      margin: 0;
      color: #333;
      font-size: 1.2rem;
  }

  .resource a {
      display: inline-block;
      margin-top: 5px;
      color: #0077cc;
      text-decoration: none;
  }

  .resource a:hover {
      text-decoration: underline;
  }

  .resource p {
      margin: 10px 0 0;
      color: #666;
  }
  
   
    @media (max-width: 768px) {
      .navigation {
          flex-direction: column;
      }
  
      .navigation button {
          width: 100%;
          margin-bottom: 5px;
      }
    }
  </style>
  
  

<BottomBar />

<script>

    // The initial section to be displayed.
    let selectedSection = 'nationalResources';

    // Object with all the sections and their content.
    const sections = {
        nationalResources: {
            title: "National Resources",
            content: [
                {
                    name: "Unlock",
                    url: "https://www.unlock.org.uk",
                    details: "Unlock is a charity that provides support for people with convictions. They offer a wealth of information on training and employment opportunities, including advice on dealing with the stigma of a criminal record."
                },
                {
                    name: "The Prisoners' Education Trust (PET)",
                    url: "https://www.prisonerseducation.org.uk",
                    details: "PET supports prisoners by funding educational courses ranging from basic literacy skills to Open University degrees. Their services continue to support ex-offenders after release."
                },
                {
                    name: "Nacro",
                    url: "https://www.nacro.org.uk",
                    details: "Nacro offers a variety of services including education, housing, and substance misuse services to help ex-offenders reintegrate back into the community. They provide direct education and training programs aimed at improving skills and employability."
                }
            ]
        },
        localInitiatives: {
            title: "Local Initiatives and Programs",
            content: [
                {
                    name: "Local Colleges and Adult Education Centers",
                    details: "Many local colleges offer courses specifically designed for individuals looking to enter new fields or enhance their skills. They often have programs or support systems in place for ex-offenders."
                },
                {
                    name: "Community Rehabilitation Companies (CRCs)",
                    details: "CRCs work with low to medium-risk offenders, providing them with support and programs that include education and skills training aimed at reducing reoffending."
                }
            ]
        },
        governmentSupport: {
            title: "Government Support",
            content: [
                {
                    name: "Education and Skills Funding Agency (ESFA)",
                    details: "Supports lifelong learning and skills training, and they have specific provisions for vulnerable groups, including ex-offenders."
                }
            ]
        },
        onlineLearning: {
            title: "Online Learning Platforms",
            content: [
                {
                    name: "FutureLearn",
                    url: "https://www.futurelearn.com",
                    details: "Offers a range of free and paid courses in partnership with educational institutions and organizations around the world."
                },
                {
                    name: "OpenLearn",
                    url: "https://www.open.edu/openlearn/",
                    details: "Free learning from The Open University."
                }
            ]
        },
        employmentServices: {
            title: "Employment Services",
            content: [
                {
                    name: "Jobcentre Plus",
                    details: "Through the 'Rapid Response Service,' Jobcentre Plus offers specialized support for ex-offenders, including help with CVs, job searches, and interviews."
                }
            ]
        },
        volunteeringOpportunities: {
            title: "Volunteering Opportunities",
            content: [
                {
                    name: "Volunteering Matters",
                    url: "https://volunteeringmatters.org.uk",
                    details: "Volunteering can be a valuable way for ex-offenders to gain experience, build a network, and strengthen their CV. Organizations like Volunteering Matters can help connect ex-offenders with appropriate volunteering opportunities."
                }
            ]
        },
        additionalSupport: {
            title: "Additional Support",
            content: [
                {
                    name: "The Shaw Trust",
                    url: "https://www.shawtrust.org.uk",
                    details: "A charity helping to transform the lives of people with disabilities and disadvantages through employment and skill building."
                }
            ]
        }
    };

    // Function to change the selected section
    function changeSection(section) {
        selectedSection = section;
    }

    import BottomBar from "$lib/BottomBar.svelte"
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import {createClient} from "@supabase/supabase-js";

    const supabase = createClient("https://tkkcqbnwdharnagmhten.supabase.co", "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InRra2NxYm53ZGhhcm5hZ21odGVuIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTEzOTk2MDMsImV4cCI6MjAyNjk3NTYwM30.VyF3VVorlbPus_GBz8YXK4c-xQ3jInSyxiNSTndDTpg")
  
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
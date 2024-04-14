<button on:click={function(){fetchJobs(apiKey1)}}>Click</button>

<script>
let apiKey1 = "0a9c346a-50a1-45e2-ae92-9e6cebeef36c";

// Function to fetch jobs for a programmer in London from the Reed API
async function fetchJobs(apiKey) {
    const baseUrl = 'https://www.reed.co.uk/api/1.0/search';
    const keyword = 'programmer';
    const location = 'london';

    const queryParams = new URLSearchParams({
        keywords: keyword,
        locationName: location
    });

    const url = `${baseUrl}?${queryParams}`;

    try {
        const response = await fetch(url, {
            headers: {
                Authorization: `Basic ${btoa(apiKey + ':')}` // Encoding apiKey as Basic Auth header
            }
        });
        if (!response.ok) {
            throw new Error(`Failed to fetch jobs. Status: ${response.status}`);
        }
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error fetching jobs:', error);
        return null;
    }
}

async function go()
{
    // Example usage
fetchJobs(apiKey)
    .then(data => {
        if (data && data.results && data.results.length > 0) {
            console.log('Jobs found:');
            data.results.forEach(job => {
                console.log(`Title: ${job.jobTitle}, Company: ${job.employerName}, Location: ${job.locationName}`);
            });
        } else {
            console.log('No jobs found.');
        }
    })
    .catch(error => {
        console.error('Error:', error);
    });
}

</script>
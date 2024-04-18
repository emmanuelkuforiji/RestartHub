<script>
async function dosomething()
{
    const REED_JOBS_API_KEY = "0a9c346a-50a1-45e2-ae92-9e6cebeef36c";
const REED_JOBS_API_URL = "https://www.reed.co.uk/api/1.0/search?";

const JOB_LOCATION = "London";
const JOB_KEYWORDS = "developer";

fetch(`${REED_JOBS_API_URL}keywords=${JOB_KEYWORDS}&locationName=${JOB_LOCATION}&resultsToTake=10&apiKey=${REED_JOBS_API_KEY}`)
 .then(response => response.json())
 .then(data => {
  console.log(data.results);
 })
 .catch(error => {
  console.error(error);
 });
}

</script>

<button on:click={dosomething}>go </button>
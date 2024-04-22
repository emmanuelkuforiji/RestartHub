<script>
async function dosomething()
{
    const apiKey = "0a9c346a-50a1-45e2-ae92-9e6cebeef36c";

    const versionNumber = '1.0';
const keywords = 'accountant';
const locationName = 'london';

const url = `https://www.reed.co.uk/api/${versionNumber}/search?keywords=${keywords}&locationName=${locationName}`;

fetch(url, {
  method: 'GET',
  headers: {
    'Authorization': `Basic ${btoa(apiKey + ':')}` // Basic Authentication
  }
})
.then(response => {
  if (!response.ok) {
    throw new Error('Network response was not ok');
  }
  return response.json();
})
.then(data => {
  // Handle the response data here
  console.log(data);
})
.catch(error => {
  // Handle errors here
  console.error('There was a problem with the fetch operation:', error);
});
}

</script>

<button on:click={dosomething}>go </button>
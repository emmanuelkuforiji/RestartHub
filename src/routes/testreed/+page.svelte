<script>
    async function dosomething() {
        const apiKey = "0a9c346a-50a1-45e2-ae92-9e6cebeef36c"; 
        const versionNumber = '1.0';
        const keywords = 'accountant';
        const locationName = 'london';
        const employerId = '123';
        const distanceFromLocation = '15';
    
        const url = `https://www.reed.co.uk/api/${versionNumber}/search?keywords=${keywords}&locationName=${locationName}&employerId=${employerId}&distanceFromLocation=${distanceFromLocation}`;
    
        fetch(url, {
          method: 'GET',
          headers: {
            'Authorization': `Basic ${btoa(apiKey + ':')}`
          }
        })
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.json();
        })
        .then(data => {
          console.log(data);
        })
        .catch(error => {
          console.error('There was a problem with the fetch operation:', error);
        });
    }
    </script>
    

<button on:click={dosomething}>go </button>
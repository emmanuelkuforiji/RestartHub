<button on:click={callReed}>Click</button>

<script>
let APIKey = "0a9c346a-50a1-45e2-ae92-9e6cebeef36c";

async function callReed() 
{

  try {

    const body = new URLSearchParams();


    body.append('keywords', "programmer");
    body.append('locationName', "London");

    const response = await fetch("https://www.reed.co.uk/api/1.0/search?", {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Authorization': `API-KEY ${APIKey}`
      },
      body: body
    });

    if (!response.ok) {
      throw new Error(`Failed to initialize transaction: ${response.status}`)
    }

    let responseData = await response.json();

    console.log('Transaction initialized successfully ' + responseData);
    

  } catch (error) {
    console.error('Error initializing transaction:', error.message);
    throw error;
  }
};
</script>
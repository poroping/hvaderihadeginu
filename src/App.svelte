<script>
	let currentDate = new Date();
	      let month = '' + (currentDate.getMonth() + 1),
         day = '' + currentDate.getDate(),
         year = currentDate.getFullYear();

    if (month.length < 2) 
        month = '0' + month;
    if (day.length < 2) 
        day = '0' + day;

    let dateString = [year, month, day].join('-');
	
	  let promise = getFood();
  
	  async function getFood() {
			let url = `https://matarspor.is/api/daymenu/88/${dateString}/${dateString}`
		  const res = await fetch(url);
		  const lunch = await res.json();
  
		  if (res.ok) {
			  if (lunch.length != 0) {
				  return lunch;
			  } else {
				  throw new Error("No food today (${dateString}) :(")
			  }
		  } else {
			  throw new Error("Upstream error!");
		  }
	  }
  
	  function handleClick() {
		  promise = getFood();
	  }
  </script>
  
  <button on:click={handleClick}>
	  Hvað er í hádeginu?
  </button>
  
  {#await promise}
	  <p>...searching</p>
  {:then lunch}
{#each lunch as l}
<p><b>{l["meal"]["mealCategory"]["nameIs"]}</b></p>
	  <p>{l["meal"]["mealName"]}</p>
	{/each}
  {:catch error}
	  <p style="color: red">{error.message}</p>
  {/await}
  
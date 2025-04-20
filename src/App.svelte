<script>
  // @ts-nocheck
  
    let countries = [];
    let loading = true;
    let search = '';
  
    fetch('https://restcountries.com/v3.1/all')
      .then(res => res.json())
      .then(data => {
        countries = data.sort((a, b) => b.population - a.population);
        loading = false;
      })
      .catch(error => {
        console.error('Error fetching world data:', error);
        loading = false;
      });
  
    // Reactive filtered list
    $: filteredCountries = countries.filter(
      country => country.name.common.toLowerCase().includes(search.toLowerCase())
    );
  
    // World totals
    $: totalPopulation = countries.reduce((sum, c) => sum + c.population, 0);
    $: totalArea = countries.reduce((sum, c) => sum + (c.area || 0), 0);
  
    // Special country stats
    function getMostPopulatedCountry() {
      return countries.reduce((max, c) => !max || c.population > max.population ? c : max, null);
    }
  
    function getLeastPopulatedCountry() {
      return countries.reduce((min, c) => !min || c.population < min.population ? c : min, null);
    }
  
    function getLargestCountry() {
      return countries.reduce((max, c) => !max || (c.area || 0) > (max.area || 0) ? c : max, null);
    }
  
    function getSmallestCountry() {
      return countries.reduce((min, c) => !min || ((c.area || Infinity) < (min.area || Infinity)) ? c : min, null);
    }
  
  </script>
  
  {#if loading}
    <p>Loading world data...</p>
  {:else}
    <h1>🌍 World Population by Country</h1>
    <p><strong>Total World Population:</strong> {totalPopulation.toLocaleString()}</p>
    <p><strong>Total Land Area:</strong> {totalArea.toLocaleString()} km²</p>
  
    <input
      type="text"
      bind:value={search}
      placeholder="Search for a country..."
      style="margin: 16px 0; padding: 8px; width: 100%; max-width: 400px;"
    />
  
    {#if search}
      <button 
        on:click={() => search = ''} 
        style="margin-bottom: 20px; padding: 8px 16px; cursor: pointer; border: none; background-color: #007BFF; color: white; border-radius: 4px;"
      >
        ⬅ Back to full list
      </button>
    {/if}
  
    {#if filteredCountries.length === 0}
      <p>No countries match your search.</p>
    {:else}
      <ul>
        {#each filteredCountries as country}
          <li style="display: flex; align-items: center; margin-bottom: 12px;">
            <img src={country.flags.svg} alt="Flag of {country.name.common}" style="width: 30px; margin-right: 10px;" />
            <span>
              {country.name.common} — Population: {country.population.toLocaleString()} | Area: {country.area?.toLocaleString() || "N/A"} km²
            </span>
          </li>
        {/each}
      </ul>
    {/if}
  
    <hr style="margin: 20px 0;" />
  
    <h2>🏆 Highlights</h2>
    {#if countries.length > 0}
      <p><strong>Most Populated:</strong> {getMostPopulatedCountry().name.common} ({getMostPopulatedCountry().population.toLocaleString()} people)</p>
      <p><strong>Least Populated:</strong> {getLeastPopulatedCountry().name.common} ({getLeastPopulatedCountry().population.toLocaleString()} people)</p>
      <p><strong>Largest by Area:</strong> {getLargestCountry().name.common} ({getLargestCountry().area.toLocaleString()} km²)</p>
      <p><strong>Smallest by Area:</strong> {getSmallestCountry().name.common} ({getSmallestCountry().area.toLocaleString()} km²)</p>
    {/if}
  {/if}
  
  <footer style="
    background: #f5f5f5;
    text-align: center;
    padding: 12px 0;
    font-size: 14px;
    color: #555;
    border-top: 1px solid #ddd;
  ">

    <a
      
              href="https://uspekhi.web.app" rel="noopener"  target="_blank"  >
             
               Copyright © {new Date().getFullYear()} USPEKHI
            </a>

  
  </footer>
  
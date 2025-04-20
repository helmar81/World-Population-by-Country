<script>
    // @ts-nocheck
    
      let visitedCountries = [
        { 
          name: "Japan", 
          flag: "https://flagcdn.com/w320/jp.png", 
          date: "April 2023", 
          notes: "Cherry blossom season was incredible!" 
        },
        { 
          name: "Norway", 
          flag: "https://flagcdn.com/w320/no.png", 
          date: "August 2022", 
          notes: "Loved the fjords and midnight sun." 
        },
        { 
          name: "Brazil", 
          flag: "https://flagcdn.com/w320/br.png", 
          date: "December 2019", 
          notes: "Carnival vibes even in the off-season!" 
        },
        // Add all your 72 here!
      ];
    </script>
    
    <h1>🌍 My Visited Countries ({visitedCountries.length})</h1>
    
    <ul style="list-style: none; padding: 0;">
      {#each visitedCountries as country}
        <li style="display: flex; align-items: center; margin-bottom: 16px;">
          <img src={country.flag} alt="Flag of {country.name}" style="width: 40px; margin-right: 12px; border-radius: 4px;" />
          <div>
            <strong>{country.name}</strong> <br />
            <small>📅 {country.date}</small> <br />
            <em>📝 {country.notes}</em>
          </div>
        </li>
      {/each}
    </ul>
    
    <footer style="
      background: #f5f5f5;
      text-align: center;
      padding: 12px 0;
      font-size: 14px;
      color: #555;
      border-top: 1px solid #ddd;
    ">
      Proudly visited {visitedCountries.length} countries and counting!
    </footer>
    
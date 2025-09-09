# WeatherApp
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Weather Application</title>
    <!-- Tailwind CSS CDN for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts CDN -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap"
      rel="stylesheet"
    />
    <style>
      body {
        background-color: #f9f7fe;
        font-family: "Roboto", sans-serif;
      }
    </style>
    <!-- This script tag loads the axios library -->
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
  </head>
  <body class="p-4">
    <div class="bg-white max-w-2xl mx-auto p-6 md:p-10 rounded-xl shadow-lg mt-6 md:mt-12">
      <header class="border-b border-gray-100 pb-6 mb-6">
        <form id="search-form" class="flex flex-col md:flex-row space-y-4 md:space-y-0 md:space-x-4">
          <input
            type="search"
            placeholder="Enter a city..."
            required
            class="flex-grow bg-[#f9f7fe] text-gray-400 text-lg py-4 px-6 rounded-md focus:outline-none"
            id="search-input"
          />
          <input
            type="submit"
            value="Search"
            class="bg-[#885df1] text-white text-lg py-4 px-6 rounded-md cursor-pointer hover:opacity-90 transition-opacity"
          />
        </form>
      </header>
      <main class="py-6">
        <div class="flex justify-between items-center mb-6">
          <div class="flex-1">
            <h1 class="text-4xl md:text-5xl font-extrabold text-[#272142] mb-1" id="current-city">
              Paris
            </h1>
            <p class="text-gray-400 text-base leading-6">
              <span id="current-date"></span>, <span id="condition-description">moderate rain</span>
              <br />
              Humidity: <strong class="text-[#f65282]" id="humidity">87%</strong>, Wind: <strong class="text-[#f65282]" id="wind">7.2km/h</strong>
            </p>
          </div>
          <div class="flex-shrink-0 text-center flex items-center">
            <span id="weather-icon" class="text-4xl mr-2">☀️</span>
            <span id="current-temperature-value" class="text-7xl font-bold text-[#272142]">24</span>
            <span class="relative -top-6 text-2xl text-[#272142] font-semibold">°C</span>
          </div>
        </div>
      </main>
      <footer class="border-t border-gray-100 pt-4 text-center text-sm text-gray-400">
        <p>
          This project was coded by
          <a href="https://github.com/Prudie90" target="_blank" class="text-[#885df1] hover:underline">PrudieM</a> and is
          <a href="https://github.com/Prudie90/WeatherApp/edit/main/README.md" target="_blank" class="text-[#885df1] hover:underline">on GitHub</a> and
          <a href="https://app.netlify.com/projects/idyllic-begonia-f54022/overview" target="_blank" class="text-[#885df1] hover:underline">hosted on Netlify</a>
        </p>
      </footer>
    </div>
    <script>
      (function () {
        // Function to update the weather UI with data from the API response
        function refreshWeather(response) {
          const temperatureElement = document.querySelector("#current-temperature-value");
          const cityElement = document.querySelector("#current-city");
          const descriptionElement = document.querySelector("#condition-description");
          const humidityElement = document.querySelector("#humidity");
          const windElement = document.querySelector("#wind");
          const iconElement = document.querySelector("#weather-icon");
          
          const temperature = response.data.temperature.current;
          const description = response.data.condition.description;
          const humidity = response.data.temperature.humidity;
          const wind = response.data.wind.speed;
          const icon = response.data.condition.icon_url;

          cityElement.innerHTML = response.data.city;
          temperatureElement.innerHTML = Math.round(temperature);
          descriptionElement.innerHTML = description;
          humidityElement.innerHTML = `${humidity}%`;
          windElement.innerHTML = `${wind}km/h`;
          iconElement.innerHTML = `<img src="${icon}" class="w-16 h-16 inline-block" alt="${description}" />`;
        }

        // Function to fetch weather data for a given city
        function searchCity(city) {
          const apiKey = "b2a5adcct04b33178913oc335f405433";
          const apiUrl = `https://api.shecodes.io/weather/v1/current?query=${city}&key=${apiKey}&units=metric`;
          axios.get(apiUrl).then(refreshWeather);
        }

        // Handler for the form submission
        function handleSearchSubmit(event) {
          event.preventDefault();
          const searchInput = document.querySelector("#search-input");
          searchCity(searchInput.value);
        }

        // Get the search form element and add a submit event listener
        const searchForm = document.querySelector("#search-form");
        searchForm.addEventListener("submit", handleSearchSubmit);

        // Set the current date and time
        const dateElement = document.querySelector("#current-date");
        const now = new Date();
        const formattedDate = `${now.getDate().toString().padStart(2, '0')}/${(now.getMonth() + 1).toString().padStart(2, '0')}/${now.getFullYear()}, ${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
        dateElement.innerHTML = formattedDate;

        // Load the weather for the initial city on page load
        searchCity("Paris");
      })();
    </script>
  </body>
</html>

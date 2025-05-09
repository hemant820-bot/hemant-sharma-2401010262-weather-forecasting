# hemant-sharma-2401010262-weather-forecasting

# team members
 hemant sharma 2401010262
 himanshu kumar 2401010264
 vishal takkar 2401010200
 krish jangra  2401010227

 # features
 accurate predict weather
 measure air quality
 weather condition show
show aqi level

# Installation
Follow these steps to get the Plant Type Detector up and running on your local machine:

Clone the repository:

git clone https://github.com/hemant820-bot/weather_forecasting_Webapp.git

cd weather predicton
Install the required dependencies:

pip install -r requirements.txt
Note: Ensure you have a requirements.txt file listing all necessary packages, including streamlit, json, opencv-python, and inference-sdk, liveserver, javascript.
Add and configure your Roboflow API key:

Obtain an API key from your Roboflow account.
In streamlit_app.py, the API key is expected as a Streamlit input. You can modify the api_key = st.sidebar.text_input("Roboflow API Key", ...) line if you prefer to use environment variables or Streamlit secrets.
# How to Run
Start the Streamlit application:

streamlit run streamlit_app.py
Open in your browser:

Streamlit will provide a local URL (e.g., http://localhost:8501) that you can open in your web browser.
# here is our code
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>My Weather App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-image: url('https://images.unsplash.com/photo-1504384308090-c894fdcc538d');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      color: white;
      text-align: center;
      margin: 0;
      padding: 0;
    }

    .overlay {
      background-color: rgba(0, 0, 0, 0.5);
      min-height: 100vh;
      padding: 50px 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }

    .container {
      background-color: rgba(255, 255, 255, 0.1);
      padding: 30px;
      border-radius: 15px;
      backdrop-filter: blur(8px);
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
      max-width: 400px;
      width: 100%;
    }

    input, button {
      padding: 10px;
      margin: 10px 5px;
      border: none;
      border-radius: 5px;
      font-size: 16px;
    }

    input {
      width: 70%;
    }

    button {
      background-color: #ffffff;
      color: #2193b0;
      cursor: pointer;
    }

    .result {
      margin-top: 20px;
      font-size: 18px;
    }

    .result img {
      width: 64px;
      height: 64px;
      vertical-align: middle;
    }

    h1 {
      margin-bottom: 20px;
    }
  </style>
</head>
<body>

  <div class="overlay">
    <div class="container">
      <h1>Weather App</h1>
      <input type="text" id="locationInput" placeholder="Enter location" />
      <button onclick="getWeather()">Get Weather</button>
      <div class="result" id="result"></div>
    </div>
  </div>

  <script>
    function getAQILevel(pm2_5) {
      if (pm2_5 <= 12) return "Good";
      if (pm2_5 <= 35.4) return "Moderate";
      if (pm2_5 <= 55.4) return "Unhealthy for Sensitive Groups";
      if (pm2_5 <= 150.4) return "Unhealthy";
      if (pm2_5 <= 250.4) return "Very Unhealthy";
      return "Hazardous";
    }

    async function getWeather() {
      const location = document.getElementById('locationInput').value.trim();
      if (!location) {
        alert("Please enter a location.");
        return;
      }

      const apiKey = '31185e123eb14f1da6b64227253004';
      const url = `https://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${encodeURIComponent(location)}&aqi=yes`;

      document.getElementById('result').innerHTML = '<p>Loading...</p>';

      try {
        const response = await fetch(url);
        if (!response.ok) throw new Error("Failed to fetch data.");
        const data = await response.json();

        const tempC = data.current.temp_c;
        const localTime = data.location.localtime;
        const condition = data.current.condition.text;
        const icon = data.current.condition.icon;

        const pm2_5 = data.current.air_quality.pm2_5.toFixed(1);
        const aqiLevel = getAQILevel(pm2_5);

        document.getElementById('result').innerHTML = `
          <p><strong>Location:</strong> ${data.location.name}, ${data.location.country}</p>
          <p><strong>Local Time:</strong> ${localTime}</p>
          <p><strong>Temperature:</strong> ${tempC}°C</p>
          <p><strong>Condition:</strong> ${condition} <img src="https:${icon}" alt="${condition}" /></p>
          <p><strong>Air Quality (PM2.5):</strong> ${pm2_5} µg/m³</p>
          <p><strong>AQI Level:</strong> ${aqiLevel}</p>
        `;
      } catch (error) {
        document.getElementById('result').innerHTML = '<p>Error fetching weather data.</p>';
        console.error(error);
      }
    }
  </script>

</body>
</html>




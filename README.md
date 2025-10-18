<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FORCASTT - Official India Meteorological Data</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 1000px;
            width: 100%;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.3);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            color: white;
            padding: 25px;
            text-align: center;
            position: relative;
        }

        .header h1 {
            font-size: 2.2rem;
            margin-bottom: 10px;
        }

        .data-source-selector {
            margin-top: 15px;
            display: flex;
            justify-content: center;
            gap: 15px;
        }

        .source-btn {
            padding: 8px 16px;
            background: rgba(255, 255, 255, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.3);
            border-radius: 20px;
            color: white;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .source-btn.active {
            background: rgba(255, 255, 255, 0.3);
            border-color: white;
        }

        .search-container {
            padding: 20px;
            display: flex;
            gap: 10px;
            background: #f8f9fa;
            border-bottom: 1px solid #e9ecef;
        }

        .city-select {
            flex: 1;
            padding: 15px 20px;
            border: 2px solid #e9ecef;
            border-radius: 50px;
            font-size: 1rem;
            outline: none;
            background: white;
        }

        .search-btn {
            padding: 15px 25px;
            background: #1e3c72;
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-size: 1rem;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .search-btn:hover {
            background: #2a5298;
        }

        .location-btn {
            padding: 15px;
            background: #28a745;
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-size: 1rem;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .location-btn:hover {
            background: #218838;
        }

        .current-weather {
            padding: 30px;
            text-align: center;
            background: white;
            position: relative;
        }

        .location {
            font-size: 1.8rem;
            color: #1e3c72;
            margin-bottom: 5px;
        }

        .temperature {
            font-size: 4rem;
            font-weight: bold;
            color: #1e3c72;
            margin: 15px 0;
        }

        .weather-icon {
            font-size: 4rem;
            margin: 15px 0;
            color: #ff9f43;
        }

        .details {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            padding: 25px;
            background: #f8f9fa;
        }

        .detail-card {
            background: white;
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
            border-left: 4px solid #1e3c72;
            transition: transform 0.3s ease;
        }

        .detail-card:hover {
            transform: translateY(-5px);
        }

        .forecast {
            padding: 25px;
            background: white;
        }

        .forecast-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 12px;
        }

        .forecast-card {
            background: white;
            padding: 15px;
            border-radius: 12px;
            text-align: center;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
            transition: transform 0.3s ease;
            border: 1px solid #e9ecef;
        }

        .forecast-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .forecast-day {
            font-weight: bold;
            color: #1e3c72;
            margin-bottom: 8px;
            font-size: 0.9rem;
        }

        .forecast-icon {
            font-size: 1.8rem;
            color: #ff9f43;
            margin: 8px 0;
        }

        .forecast-temp {
            font-size: 1.1rem;
            color: #1e3c72;
            font-weight: bold;
        }

        .footer {
            text-align: center;
            padding: 25px;
            background: #1e3c72;
            color: white;
            font-size: 1.1rem;
            font-weight: 500;
        }

        .made-by {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            animation: fadeIn 2s ease-in-out;
        }

        .heart {
            color: #ff6b6b;
            animation: heartbeat 1.5s ease-in-out infinite both;
            display: inline-block;
        }

        @keyframes heartbeat {
            from {
                transform: scale(1);
                transform-origin: center center;
                animation-timing-function: ease-out;
            }
            10% {
                transform: scale(0.91);
                animation-timing-function: ease-in;
            }
            17% {
                transform: scale(0.98);
                animation-timing-function: ease-out;
            }
            33% {
                transform: scale(0.87);
                animation-timing-function: ease-in;
            }
            45% {
                transform: scale(1);
                animation-timing-function: ease-out;
            }
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .loading {
            text-align: center;
            padding: 20px;
            color: #1e3c72;
        }

        .error {
            text-align: center;
            padding: 20px;
            color: #e74c3c;
            background: #ffeaea;
            margin: 10px;
            border-radius: 10px;
        }

        .data-source-info {
            text-align: center;
            padding: 10px;
            background: #e3f2fd;
            color: #1e3c72;
            font-size: 0.9rem;
        }

        .hidden {
            display: none;
        }

        .temp-min-max {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 10px;
        }

        .temp-min, .temp-max {
            font-size: 1rem;
            color: #666;
        }

        .temp-min i {
            color: #3498db;
        }

        .temp-max i {
            color: #e74c3c;
        }

        /* Responsive design */
        @media (max-width: 768px) {
            .header h1 {
                font-size: 1.8rem;
            }
            
            .temperature {
                font-size: 3rem;
            }
            
            .search-container {
                flex-direction: column;
            }
            
            .forecast-container {
                grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            }
            
            .data-source-selector {
                flex-direction: column;
                align-items: center;
            }
            
            .source-btn {
                width: 200px;
            }
            
            .footer {
                padding: 20px;
                font-size: 1rem;
            }
        }

        @media (max-width: 480px) {
            .header h1 {
                font-size: 1.5rem;
            }
            
            .temperature {
                font-size: 2.5rem;
            }
            
            .forecast-container {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .details {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1><i class="fas fa-cloud-sun"></i> FORCASTT</h1>
            <p>India Meteorological Department - Real-time Weather Data</p>
            <div class="data-source-selector">
                <button class="source-btn active" data-source="openweather">OpenWeather API</button>
                <button class="source-btn" data-source="imd">IMD Simulation</button>
            </div>
        </div>

        <div class="search-container">
            <select class="city-select" id="city-select">
                <option value="New Delhi">New Delhi</option>
                <option value="Mumbai">Mumbai</option>
                <option value="Chennai">Chennai</option>
                <option value="Kolkata">Kolkata</option>
                <option value="Bengaluru">Bengaluru</option>
                <option value="Hyderabad">Hyderabad</option>
                <option value="Ahmedabad">Ahmedabad</option>
                <option value="Pune">Pune</option>
                <option value="Jaipur">Jaipur</option>
                <option value="Lucknow">Lucknow</option>
                <option value="Srinagar">Srinagar</option>
                <option value="Shimla">Shimla</option>
                <option value="Dehradun">Dehradun</option>
                <option value="Patna">Patna</option>
                <option value="Ranchi">Ranchi</option>
                <option value="Bhubaneswar">Bhubaneswar</option>
                <option value="Guwahati">Guwahati</option>
                <option value="Thiruvananthapuram">Thiruvananthapuram</option>
                <option value="Kochi">Kochi</option>
                <option value="Panaji">Panaji</option>
                <option value="Chandigarh">Chandigarh</option>
                <option value="Bhopal">Bhopal</option>
                <option value="Raipur">Raipur</option>
                <option value="Gangtok">Gangtok</option>
                <option value="Agartala">Agartala</option>
                <option value="Shillong">Shillong</option>
                <option value="Aizawl">Aizawl</option>
                <option value="Imphal">Imphal</option>
                <option value="Kohima">Kohima</option>
                <option value="Itanagar">Itanagar</option>
                <option value="Port Blair">Port Blair</option>
                <option value="Jammu">Jammu</option>
                <option value="Leh">Leh</option>
                <option value="Coimbatore">Coimbatore</option>
                <option value="Visakhapatnam">Visakhapatnam</option>
                <option value="Surat">Surat</option>
                <option value="Kanpur">Kanpur</option>
                <option value="Nagpur">Nagpur</option>
                <option value="Indore">Indore</option>
                <option value="Vadodara">Vadodara</option>
                <option value="Gurugram">Gurugram (Haryana)</option>
                <option value="Faridabad">Faridabad (Haryana)</option>
                <option value="Hisar">Hisar (Haryana)</option>
            </select>
            <button class="location-btn" id="location-btn" title="Use Current Location">
                <i class="fas fa-location-arrow"></i>
            </button>
            <button class="search-btn" id="search-btn">
                <i class="fas fa-search"></i> Get Weather
            </button>
        </div>

        <div class="data-source-info" id="data-source-info">
            Currently using: OpenWeather API (Live Data)
        </div>

        <div class="error hidden" id="error-message"></div>

        <div class="loading hidden" id="loading">
            <i class="fas fa-spinner fa-spin"></i> Fetching weather data...
        </div>

        <div class="current-weather">
            <h2 class="location" id="location">New Delhi, India</h2>
            <p class="date" id="date">Monday, October 17, 2023</p>
            <div class="weather-icon" id="weather-icon">
                <i class="fas fa-sun"></i>
            </div>
            <div class="temperature" id="temperature">32°C</div>
            <div class="description" id="description">Clear Sky</div>
            <div class="temp-min-max">
                <div class="temp-min"><i class="fas fa-temperature-low"></i> <span id="temp-min">28°C</span></div>
                <div class="temp-max"><i class="fas fa-temperature-high"></i> <span id="temp-max">35°C</span></div>
            </div>
        </div>

        <div class="details">
            <div class="detail-card">
                <i class="fas fa-wind"></i>
                <h3>Wind Speed</h3>
                <p id="wind-speed">12 km/h</p>
            </div>
            <div class="detail-card">
                <i class="fas fa-tint"></i>
                <h3>Humidity</h3>
                <p id="humidity">45%</p>
            </div>
            <div class="detail-card">
                <i class="fas fa-compress-arrows-alt"></i>
                <h3>Pressure</h3>
                <p id="pressure">1012 hPa</p>
            </div>
            <div class="detail-card">
                <i class="fas fa-eye"></i>
                <h3>Visibility</h3>
                <p id="visibility">8 km</p>
            </div>
        </div>

        <div class="forecast">
            <h2>7-Day Forecast</h2>
            <div class="forecast-container" id="forecast-container">
                <!-- Forecast will be populated by JavaScript -->
            </div>
        </div>

        <!-- New Footer Section -->
        <div class="footer">
            <div class="made-by">
                Made with <span class="heart">💖</span> By Armeen
            </div>
        </div>
    </div>

    <script>
        // API Configuration
        const API_KEYS = {
            openweather: 'b86e6e087363a62208828e231d3edb7e'
        };

        // Data sources configuration
        const DATA_SOURCES = {
            imd: {
                name: "IMD Simulation",
                description: "Simulated IMD Data",
                fetchData: fetchIMDData
            },
            openweather: {
                name: "OpenWeather API",
                description: "Live Weather Data",
                fetchData: fetchOpenWeatherData
            }
        };

        let currentSource = 'openweather';

        // Update date
        function updateDate() {
            const now = new Date();
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('date').textContent = now.toLocaleDateString('en-US', options);
        }

        // IMD Data Scraping Simulation
        async function fetchIMDData(city) {
            // Simulate web scraping delay
            await new Promise(resolve => setTimeout(resolve, 1500));
            
            // Simulated scraped data based on city (expanded with more cities including Haryana)
            const cityData = {
                "New Delhi": { temp: 32, humidity: 45, wind: 12, pressure: 1012, visibility: 8, condition: "Clear Sky", icon: "fa-sun", temp_min: 28, temp_max: 35 },
                "Mumbai": { temp: 30, humidity: 78, wind: 15, pressure: 1010, visibility: 6, condition: "Partly Cloudy", icon: "fa-cloud-sun", temp_min: 27, temp_max: 32 },
                "Chennai": { temp: 34, humidity: 70, wind: 18, pressure: 1008, visibility: 10, condition: "Hot and Humid", icon: "fa-sun", temp_min: 30, temp_max: 37 },
                "Kolkata": { temp: 31, humidity: 82, wind: 10, pressure: 1011, visibility: 4, condition: "Haze", icon: "fa-smog", temp_min: 28, temp_max: 33 },
                "Bengaluru": { temp: 26, humidity: 65, wind: 14, pressure: 1013, visibility: 12, condition: "Pleasant", icon: "fa-cloud-sun", temp_min: 23, temp_max: 29 },
                "Hyderabad": { temp: 29, humidity: 60, wind: 13, pressure: 1010, visibility: 9, condition: "Mostly Sunny", icon: "fa-sun", temp_min: 25, temp_max: 32 },
                "Ahmedabad": { temp: 33, humidity: 50, wind: 16, pressure: 1009, visibility: 7, condition: "Sunny", icon: "fa-sun", temp_min: 29, temp_max: 36 },
                "Pune": { temp: 28, humidity: 68, wind: 11, pressure: 1011, visibility: 11, condition: "Clear", icon: "fa-sun", temp_min: 24, temp_max: 31 },
                "Jaipur": { temp: 31, humidity: 48, wind: 14, pressure: 1010, visibility: 8, condition: "Sunny", icon: "fa-sun", temp_min: 27, temp_max: 34 },
                "Lucknow": { temp: 30, humidity: 65, wind: 12, pressure: 1012, visibility: 6, condition: "Partly Cloudy", icon: "fa-cloud-sun", temp_min: 26, temp_max: 33 },
                "Srinagar": { temp: 18, humidity: 55, wind: 8, pressure: 1020, visibility: 10, condition: "Cool and Clear", icon: "fa-sun", temp_min: 14, temp_max: 22 },
                "Shimla": { temp: 15, humidity: 50, wind: 10, pressure: 1018, visibility: 12, condition: "Sunny", icon: "fa-sun", temp_min: 11, temp_max: 19 },
                "Dehradun": { temp: 25, humidity: 60, wind: 9, pressure: 1015, visibility: 9, condition: "Partly Cloudy", icon: "fa-cloud-sun", temp_min: 21, temp_max: 28 },
                "Patna": { temp: 30, humidity: 75, wind: 11, pressure: 1011, visibility: 5, condition: "Humid", icon: "fa-cloud", temp_min: 26, temp_max: 33 },
                "Ranchi": { temp: 28, humidity: 70, wind: 12, pressure: 1012, visibility: 8, condition: "Cloudy", icon: "fa-cloud", temp_min: 24, temp_max: 31 },
                "Bhubaneswar": { temp: 32, humidity: 80, wind: 15, pressure: 1009, visibility: 7, condition: "Hot and Humid", icon: "fa-sun", temp_min: 28, temp_max: 35 },
                "Guwahati": { temp: 29, humidity: 85, wind: 10, pressure: 1010, visibility: 6, condition: "Rainy", icon: "fa-cloud-rain", temp_min: 25, temp_max: 31 },
                "Thiruvananthapuram": { temp: 31, humidity: 75, wind: 14, pressure: 1008, visibility: 10, condition: "Tropical", icon: "fa-sun", temp_min: 27, temp_max: 33 },
                "Kochi": { temp: 30, humidity: 78, wind: 16, pressure: 1007, visibility: 9, condition: "Coastal Humid", icon: "fa-cloud-sun", temp_min: 26, temp_max: 32 },
                "Panaji": { temp: 29, humidity: 80, wind: 13, pressure: 1009, visibility: 8, condition: "Partly Cloudy", icon: "fa-cloud-sun", temp_min: 25, temp_max: 31 },
                "Chandigarh": { temp: 28, humidity: 50, wind: 11, pressure: 1014, visibility: 10, condition: "Clear", icon: "fa-sun", temp_min: 24, temp_max: 31 },
                "Bhopal": { temp: 29, humidity: 55, wind: 12, pressure: 1011, visibility: 9, condition: "Sunny", icon: "fa-sun", temp_min: 25, temp_max: 32 },
                "Raipur": { temp: 30, humidity: 65, wind: 10, pressure: 1010, visibility: 7, condition: "Warm", icon: "fa-sun", temp_min: 26, temp_max: 33 },
                "Gangtok": { temp: 16, humidity: 70, wind: 8, pressure: 1018, visibility: 8, condition: "Misty", icon: "fa-smog", temp_min: 12, temp_max: 20 },
                "Agartala": { temp: 30, humidity: 82, wind: 9, pressure: 1010, visibility: 5, condition: "Humid", icon: "fa-cloud", temp_min: 26, temp_max: 32 },
                "Shillong": { temp: 20, humidity: 75, wind: 10, pressure: 1015, visibility: 7, condition: "Cool Rain", icon: "fa-cloud-rain", temp_min: 16, temp_max: 23 },
                "Aizawl": { temp: 22, humidity: 80, wind: 7, pressure: 1014, visibility: 6, condition: "Cloudy", icon: "fa-cloud", temp_min: 18, temp_max: 25 },
                "Imphal": { temp: 25, humidity: 78, wind: 8, pressure: 1013, visibility: 8, condition: "Partly Cloudy", icon: "fa-cloud-sun", temp_min: 21, temp_max: 28 },
                "Kohima": { temp: 19, humidity: 72, wind: 9, pressure: 1016, visibility: 9, condition: "Mild", icon: "fa-sun", temp_min: 15, temp_max: 22 },
                "Itanagar": { temp: 26, humidity: 85, wind: 11, pressure: 1012, visibility: 6, condition: "Rainy", icon: "fa-cloud-rain", temp_min: 22, temp_max: 29 },
                "Port Blair": { temp: 31, humidity: 80, wind: 15, pressure: 1008, visibility: 10, condition: "Tropical", icon: "fa-sun", temp_min: 27, temp_max: 33 },
                "Jammu": { temp: 27, humidity: 50, wind: 10, pressure: 1015, visibility: 10, condition: "Clear", icon: "fa-sun", temp_min: 23, temp_max: 30 },
                "Leh": { temp: 5, humidity: 30, wind: 12, pressure: 1025, visibility: 15, condition: "Cold and Dry", icon: "fa-sun", temp_min: 0, temp_max: 10 },
                "Coimbatore": { temp: 28, humidity: 70, wind: 13, pressure: 1010, visibility: 9, condition: "Pleasant", icon: "fa-cloud-sun", temp_min: 24, temp_max: 31 },
                "Visakhapatnam": { temp: 31, humidity: 75, wind: 16, pressure: 1009, visibility: 8, condition: "Coastal", icon: "fa-sun", temp_min: 27, temp_max: 34 },
                "Surat": { temp: 32, humidity: 60, wind: 14, pressure: 1010, visibility: 7, condition: "Sunny", icon: "fa-sun", temp_min: 28, temp_max: 35 },
                "Kanpur": { temp: 30, humidity: 55, wind: 11, pressure: 1012, visibility: 6, condition: "Clear", icon: "fa-sun", temp_min: 26, temp_max: 33 },
                "Nagpur": { temp: 29, humidity: 62, wind: 12, pressure: 1011, visibility: 8, condition: "Warm", icon: "fa-sun", temp_min: 25, temp_max: 32 },
                "Indore": { temp: 28, humidity: 50, wind: 13, pressure: 1011, visibility: 9, condition: "Sunny", icon: "fa-sun", temp_min: 24, temp_max: 31 },
                "Vadodara": { temp: 31, humidity: 58, wind: 15, pressure: 1010, visibility: 8, condition: "Clear", icon: "fa-sun", temp_min: 27, temp_max: 34 },
                "Gurugram": { temp: 31, humidity: 48, wind: 13, pressure: 1011, visibility: 7, condition: "Sunny", icon: "fa-sun", temp_min: 27, temp_max: 34 },
                "Faridabad": { temp: 31, humidity: 50, wind: 12, pressure: 1011, visibility: 7, condition: "Clear Sky", icon: "fa-sun", temp_min: 27, temp_max: 34 },
                "Hisar": { temp: 32, humidity: 45, wind: 14, pressure: 1010, visibility: 8, condition: "Hot and Dry", icon: "fa-sun", temp_min: 28, temp_max: 36 }
            };

            const data = cityData[city];
            if (!data) throw new Error(`IMD data not available for ${city}`);

            return {
                location: `${city}, India`,
                temperature: `${data.temp}°C`,
                description: data.condition,
                icon: data.icon,
                windSpeed: `${data.wind} km/h`,
                humidity: `${data.humidity}%`,
                pressure: `${data.pressure} hPa`,
                visibility: `${data.visibility} km`,
                tempMin: `${data.temp_min}°C`,
                tempMax: `${data.temp_max}°C`,
                forecast: generateForecast(data.temp, data.condition)
            };
        }

        // OpenWeather API Integration
        async function fetchOpenWeatherData(city) {
            try {
                // Fetch current weather
                const currentResponse = await fetch(
                    `https://api.openweathermap.org/data/2.5/weather?q=${city},IN&appid=${API_KEYS.openweather}&units=metric`
                );
                
                if (!currentResponse.ok) {
                    throw new Error(`Weather data not found for ${city}`);
                }
                
                const currentData = await currentResponse.json();
                
                // Fetch forecast data
                const forecastResponse = await fetch(
                    `https://api.openweathermap.org/data/2.5/forecast?q=${city},IN&appid=${API_KEYS.openweather}&units=metric`
                );
                
                if (!forecastResponse.ok) {
                    throw new Error(`Forecast data not found for ${city}`);
                }
                
                const forecastData = await forecastResponse.json();
                
                return {
                    location: `${currentData.name}, India`,
                    temperature: `${Math.round(currentData.main.temp)}°C`,
                    description: currentData.weather[0].description,
                    icon: getWeatherIcon(currentData.weather[0].main),
                    windSpeed: `${(currentData.wind.speed * 3.6).toFixed(1)} km/h`,
                    humidity: `${currentData.main.humidity}%`,
                    pressure: `${currentData.main.pressure} hPa`,
                    visibility: `${(currentData.visibility / 1000).toFixed(1)} km`,
                    tempMin: `${Math.round(currentData.main.temp_min)}°C`,
                    tempMax: `${Math.round(currentData.main.temp_max)}°C`,
                    forecast: generateForecastFromAPI(forecastData.list)
                };
            } catch (error) {
                throw new Error(`OpenWeather API error: ${error.message}`);
            }
        }

        // Helper functions
        function getWeatherIcon(condition) {
            const icons = {
                'Clear': 'fa-sun',
                'Clouds': 'fa-cloud',
                'Rain': 'fa-cloud-rain',
                'Drizzle': 'fa-cloud-drizzle',
                'Thunderstorm': 'fa-bolt',
                'Snow': 'fa-snowflake',
                'Mist': 'fa-smog',
                'Smoke': 'fa-smog',
                'Haze': 'fa-smog',
                'Dust': 'fa-smog',
                'Fog': 'fa-smog',
                'Sand': 'fa-smog',
                'Ash': 'fa-smog',
                'Squall': 'fa-wind',
                'Tornado': 'fa-tornado',
                'Sunny': 'fa-sun',
                'Partly cloudy': 'fa-cloud-sun',
                'Cloudy': 'fa-cloud',
                'Overcast': 'fa-cloud'
            };
            return icons[condition] || 'fa-cloud';
        }

        function generateForecast(baseTemp, condition) {
            const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
            return days.map((day, i) => {
                const tempVariation = (Math.random() - 0.5) * 6;
                return {
                    day: day,
                    icon: getWeatherIcon(condition),
                    temp: `${Math.round(baseTemp + tempVariation)}°C`
                };
            });
        }

        function generateForecastFromAPI(forecastList) {
            // Group forecast by day and get one forecast per day
            const dailyForecasts = {};
            
            forecastList.forEach(item => {
                const date = new Date(item.dt * 1000);
                const day = date.toLocaleDateString('en-US', { weekday: 'short' });
                
                if (!dailyForecasts[day] || date.getHours() === 12) {
                    dailyForecasts[day] = {
                        day: day,
                        icon: getWeatherIcon(item.weather[0].main),
                        temp: `${Math.round(item.main.temp)}°C`
                    };
                }
            });
            
            // Return only 7 days
            return Object.values(dailyForecasts).slice(0, 7);
        }

        // Update weather display
        async function updateWeather(city) {
            const loadingElement = document.getElementById('loading');
            const errorElement = document.getElementById('error-message');
            
            loadingElement.classList.remove('hidden');
            errorElement.classList.add('hidden');

            try {
                const data = await DATA_SOURCES[currentSource].fetchData(city);
                
                document.getElementById('location').textContent = data.location;
                document.getElementById('temperature').textContent = data.temperature;
                document.getElementById('description').textContent = data.description;
                document.getElementById('weather-icon').innerHTML = `<i class="fas ${data.icon}"></i>`;
                document.getElementById('wind-speed').textContent = data.windSpeed;
                document.getElementById('humidity').textContent = data.humidity;
                document.getElementById('pressure').textContent = data.pressure;
                document.getElementById('visibility').textContent = data.visibility;
                document.getElementById('temp-min').textContent = data.tempMin;
                document.getElementById('temp-max').textContent = data.tempMax;

                // Update forecast
                const forecastContainer = document.getElementById('forecast-container');
                forecastContainer.innerHTML = '';
                
                data.forecast.forEach(day => {
                    const forecastCard = document.createElement('div');
                    forecastCard.className = 'forecast-card';
                    forecastCard.innerHTML = `
                        <div class="forecast-day">${day.day}</div>
                        <div class="forecast-icon"><i class="fas ${day.icon}"></i></div>
                        <div class="forecast-temp">${day.temp}</div>
                    `;
                    forecastContainer.appendChild(forecastCard);
                });

            } catch (error) {
                errorElement.textContent = error.message;
                errorElement.classList.remove('hidden');
            } finally {
                loadingElement.classList.add('hidden');
            }
        }

        // Get location using geolocation API
        function getCurrentLocation() {
            if (!navigator.geolocation) {
                alert('Geolocation is not supported by your browser');
                return;
            }

            const loadingElement = document.getElementById('loading');
            loadingElement.classList.remove('hidden');

            navigator.geolocation.getCurrentPosition(
                async (position) => {
                    try {
                        const { latitude, longitude } = position.coords;
                        
                        // Use OpenWeather's reverse geocoding to get city name
                        const response = await fetch(
                            `https://api.openweathermap.org/geo/1.0/reverse?lat=${latitude}&lon=${longitude}&limit=1&appid=${API_KEYS.openweather}`
                        );
                        
                        if (!response.ok) {
                            throw new Error('Could not determine your location');
                        }
                        
                        const locationData = await response.json();
                        
                        if (locationData.length === 0) {
                            throw new Error('Could not determine your location');
                        }
                        
                        const city = locationData[0].name;
                        
                        // Update the dropdown and fetch weather
                        document.getElementById('city-select').value = city;
                        updateWeather(city);
                        
                    } catch (error) {
                        document.getElementById('error-message').textContent = error.message;
                        document.getElementById('error-message').classList.remove('hidden');
                    } finally {
                        loadingElement.classList.add('hidden');
                    }
                },
                (error) => {
                    loadingElement.classList.add('hidden');
                    document.getElementById('error-message').textContent = 'Unable to retrieve your location';
                    document.getElementById('error-message').classList.remove('hidden');
                }
            );
        }

        // Event listeners
        document.getElementById('search-btn').addEventListener('click', () => {
            const city = document.getElementById('city-select').value;
            updateWeather(city);
        });

        document.getElementById('location-btn').addEventListener('click', getCurrentLocation);

        // Data source selection
        document.querySelectorAll('.source-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const source = e.target.dataset.source;
                
                // Update active button
                document.querySelectorAll('.source-btn').forEach(b => b.classList.remove('active'));
                e.target.classList.add('active');
                
                // Update data source
                currentSource = source;
                document.getElementById('data-source-info').textContent = 
                    `Currently using: ${DATA_SOURCES[source].name} (${DATA_SOURCES[source].description})`;
                
                // Refresh data with new source
                const city = document.getElementById('city-select').value;
                updateWeather(city);
            });
        });

        // Initialize
        updateDate();
        updateWeather('New Delhi');
    </script>
</body>
</html>

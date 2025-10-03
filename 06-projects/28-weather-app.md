# 28 - Weather App Project

## Project Overview

Build a weather application that displays current weather conditions and forecasts using a weather API. Features include:
- Current weather display
- 5-day forecast
- Location search
- Temperature unit toggle (Celsius/Fahrenheit)
- Weather icons and descriptions
- Responsive design

## HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <link rel="stylesheet" href="styles.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
</head>
<body>
    <div class="container">
        <header class="app-header">
            <h1><i class="fas fa-cloud-sun"></i> Weather App</h1>
            <div class="unit-toggle">
                <button id="celsiusBtn" class="unit-btn active">°C</button>
                <button id="fahrenheitBtn" class="unit-btn">°F</button>
            </div>
        </header>

        <div class="search-section">
            <div class="search-container">
                <input type="text" id="cityInput" placeholder="Enter city name...">
                <button id="searchBtn"><i class="fas fa-search"></i></button>
            </div>
            <div class="location-buttons">
                <button id="currentLocationBtn"><i class="fas fa-map-marker-alt"></i> Current Location</button>
            </div>
        </div>

        <div class="loading" id="loading">
            <i class="fas fa-spinner fa-spin"></i>
            <p>Loading weather data...</p>
        </div>

        <div class="error" id="error" style="display: none;">
            <i class="fas fa-exclamation-triangle"></i>
            <p id="errorMessage">Error loading weather data</p>
        </div>

        <div class="weather-content" id="weatherContent" style="display: none;">
            <!-- Current Weather -->
            <div class="current-weather">
                <div class="location-info">
                    <h2 id="currentCity">Loading...</h2>
                    <p id="currentDate">Loading...</p>
                </div>
                <div class="weather-main">
                    <div class="temperature">
                        <span id="currentTemp">--</span>
                        <span class="unit">°C</span>
                    </div>
                    <div class="weather-icon">
                        <i id="currentIcon" class="fas fa-sun"></i>
                    </div>
                </div>
                <div class="weather-description">
                    <p id="currentDescription">Loading...</p>
                </div>
                <div class="weather-details">
                    <div class="detail-item">
                        <i class="fas fa-eye"></i>
                        <span>Visibility</span>
                        <span id="visibility">-- km</span>
                    </div>
                    <div class="detail-item">
                        <i class="fas fa-tint"></i>
                        <span>Humidity</span>
                        <span id="humidity">--%</span>
                    </div>
                    <div class="detail-item">
                        <i class="fas fa-wind"></i>
                        <span>Wind Speed</span>
                        <span id="windSpeed">-- km/h</span>
                    </div>
                    <div class="detail-item">
                        <i class="fas fa-thermometer-half"></i>
                        <span>Feels Like</span>
                        <span id="feelsLike">--°C</span>
                    </div>
                </div>
            </div>

            <!-- 5-Day Forecast -->
            <div class="forecast-section">
                <h3>5-Day Forecast</h3>
                <div class="forecast-container" id="forecastContainer">
                    <!-- Forecast items will be inserted here -->
                </div>
            </div>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
```

## CSS Styles

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #74b9ff 0%, #0984e3 100%);
    min-height: 100vh;
    color: #333;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

.app-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 30px;
    color: white;
}

.app-header h1 {
    font-size: 2.5rem;
    font-weight: 300;
}

.app-header h1 i {
    margin-right: 10px;
    color: #fdcb6e;
}

.unit-toggle {
    display: flex;
    background: rgba(255, 255, 255, 0.2);
    border-radius: 25px;
    padding: 5px;
}

.unit-btn {
    background: none;
    border: none;
    color: white;
    padding: 10px 20px;
    border-radius: 20px;
    cursor: pointer;
    transition: all 0.3s;
    font-weight: 500;
}

.unit-btn.active {
    background: white;
    color: #0984e3;
}

.search-section {
    margin-bottom: 30px;
}

.search-container {
    display: flex;
    max-width: 500px;
    margin: 0 auto 20px;
    background: white;
    border-radius: 25px;
    overflow: hidden;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

#cityInput {
    flex: 1;
    padding: 15px 20px;
    border: none;
    outline: none;
    font-size: 16px;
    background: transparent;
}

#searchBtn {
    background: #0984e3;
    color: white;
    border: none;
    padding: 15px 20px;
    cursor: pointer;
    transition: background-color 0.3s;
}

#searchBtn:hover {
    background: #0770c2;
}

.location-buttons {
    text-align: center;
}

#currentLocationBtn {
    background: rgba(255, 255, 255, 0.2);
    color: white;
    border: none;
    padding: 12px 24px;
    border-radius: 25px;
    cursor: pointer;
    font-size: 14px;
    transition: all 0.3s;
}

#currentLocationBtn:hover {
    background: rgba(255, 255, 255, 0.3);
}

.loading, .error {
    text-align: center;
    padding: 40px;
    color: white;
}

.loading i, .error i {
    font-size: 2rem;
    margin-bottom: 15px;
    display: block;
}

.error {
    background: rgba(231, 76, 60, 0.1);
    border: 1px solid rgba(231, 76, 60, 0.3);
    border-radius: 10px;
    color: #e74c3c;
}

.weather-content {
    background: white;
    border-radius: 20px;
    padding: 30px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

.current-weather {
    text-align: center;
    margin-bottom: 40px;
}

.location-info h2 {
    font-size: 2rem;
    margin-bottom: 5px;
    color: #2d3436;
}

.location-info p {
    color: #636e72;
    font-size: 1.1rem;
}

.weather-main {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 30px 0;
}

.temperature {
    font-size: 4rem;
    font-weight: 300;
    color: #2d3436;
    margin-right: 20px;
}

.weather-icon {
    font-size: 4rem;
    color: #fdcb6e;
}

.weather-description {
    margin-bottom: 30px;
}

.weather-description p {
    font-size: 1.2rem;
    color: #636e72;
    text-transform: capitalize;
}

.weather-details {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
    margin-top: 30px;
}

.detail-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    background: #f8f9fa;
    border-radius: 10px;
    text-align: center;
}

.detail-item i {
    font-size: 1.5rem;
    color: #0984e3;
    margin-bottom: 10px;
}

.detail-item span:first-of-type {
    font-size: 0.9rem;
    color: #636e72;
    margin-bottom: 5px;
}

.detail-item span:last-of-type {
    font-size: 1.2rem;
    font-weight: 600;
    color: #2d3436;
}

.forecast-section {
    margin-top: 40px;
}

.forecast-section h3 {
    font-size: 1.5rem;
    margin-bottom: 20px;
    color: #2d3436;
    text-align: center;
}

.forecast-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 15px;
}

.forecast-item {
    background: #f8f9fa;
    border-radius: 10px;
    padding: 20px;
    text-align: center;
    transition: transform 0.3s;
}

.forecast-item:hover {
    transform: translateY(-5px);
}

.forecast-date {
    font-size: 0.9rem;
    color: #636e72;
    margin-bottom: 10px;
}

.forecast-icon {
    font-size: 2rem;
    color: #fdcb6e;
    margin: 10px 0;
}

.forecast-temp {
    font-size: 1.2rem;
    font-weight: 600;
    color: #2d3436;
    margin-bottom: 5px;
}

.forecast-desc {
    font-size: 0.8rem;
    color: #636e72;
    text-transform: capitalize;
}

@media (max-width: 768px) {
    .container {
        padding: 10px;
    }
    
    .app-header {
        flex-direction: column;
        gap: 20px;
        text-align: center;
    }
    
    .app-header h1 {
        font-size: 2rem;
    }
    
    .weather-main {
        flex-direction: column;
        gap: 20px;
    }
    
    .temperature {
        font-size: 3rem;
        margin-right: 0;
    }
    
    .weather-icon {
        font-size: 3rem;
    }
    
    .weather-details {
        grid-template-columns: repeat(2, 1fr);
    }
    
    .forecast-container {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 480px) {
    .weather-details {
        grid-template-columns: 1fr;
    }
    
    .forecast-container {
        grid-template-columns: 1fr;
    }
}
```

## JavaScript Implementation

```javascript
class WeatherApp {
    constructor() {
        this.apiKey = 'YOUR_API_KEY'; // Replace with your OpenWeatherMap API key
        this.baseURL = 'https://api.openweathermap.org/data/2.5';
        this.currentUnit = 'metric';
        this.currentCity = '';
        
        this.init();
    }

    init() {
        this.bindEvents();
        this.loadLastLocation();
    }

    bindEvents() {
        // Search functionality
        document.getElementById('searchBtn').addEventListener('click', () => this.searchWeather());
        document.getElementById('cityInput').addEventListener('keypress', (e) => {
            if (e.key === 'Enter') this.searchWeather();
        });

        // Current location
        document.getElementById('currentLocationBtn').addEventListener('click', () => this.getCurrentLocation());

        // Unit toggle
        document.getElementById('celsiusBtn').addEventListener('click', () => this.setUnit('metric'));
        document.getElementById('fahrenheitBtn').addEventListener('click', () => this.setUnit('imperial'));
    }

    async searchWeather() {
        const cityInput = document.getElementById('cityInput');
        const city = cityInput.value.trim();
        
        if (city === '') {
            this.showError('Please enter a city name');
            return;
        }

        this.showLoading();
        try {
            const weatherData = await this.fetchWeatherData(city);
            this.displayWeather(weatherData);
            this.currentCity = city;
            this.saveLastLocation(city);
        } catch (error) {
            this.showError('City not found. Please try again.');
        }
    }

    async getCurrentLocation() {
        if (!navigator.geolocation) {
            this.showError('Geolocation is not supported by this browser');
            return;
        }

        this.showLoading();
        
        navigator.geolocation.getCurrentPosition(
            async (position) => {
                try {
                    const { latitude, longitude } = position.coords;
                    const weatherData = await this.fetchWeatherByCoords(latitude, longitude);
                    this.displayWeather(weatherData);
                } catch (error) {
                    this.showError('Unable to get weather for your location');
                }
            },
            (error) => {
                this.showError('Unable to get your location');
            }
        );
    }

    async fetchWeatherData(city) {
        const url = `${this.baseURL}/weather?q=${city}&appid=${this.apiKey}&units=${this.currentUnit}`;
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error('City not found');
        }
        
        return await response.json();
    }

    async fetchWeatherByCoords(lat, lon) {
        const url = `${this.baseURL}/weather?lat=${lat}&lon=${lon}&appid=${this.apiKey}&units=${this.currentUnit}`;
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error('Weather data not available');
        }
        
        return await response.json();
    }

    async fetchForecast(city) {
        const url = `${this.baseURL}/forecast?q=${city}&appid=${this.apiKey}&units=${this.currentUnit}`;
        const response = await fetch(url);
        
        if (!response.ok) {
            throw new Error('Forecast not available');
        }
        
        return await response.json();
    }

    displayWeather(data) {
        this.hideLoading();
        this.hideError();
        
        // Update current weather
        this.updateCurrentWeather(data);
        
        // Fetch and display forecast
        this.displayForecast(data.name);
        
        // Show weather content
        document.getElementById('weatherContent').style.display = 'block';
    }

    updateCurrentWeather(data) {
        const city = data.name;
        const country = data.sys.country;
        const temp = Math.round(data.main.temp);
        const description = data.weather[0].description;
        const icon = data.weather[0].icon;
        const humidity = data.main.humidity;
        const windSpeed = Math.round(data.wind.speed * 3.6); // Convert m/s to km/h
        const visibility = Math.round(data.visibility / 1000); // Convert m to km
        const feelsLike = Math.round(data.main.feels_like);

        // Update DOM elements
        document.getElementById('currentCity').textContent = `${city}, ${country}`;
        document.getElementById('currentDate').textContent = this.formatDate(new Date());
        document.getElementById('currentTemp').textContent = temp;
        document.getElementById('currentDescription').textContent = description;
        document.getElementById('currentIcon').className = this.getWeatherIcon(icon);
        document.getElementById('humidity').textContent = `${humidity}%`;
        document.getElementById('windSpeed').textContent = `${windSpeed} km/h`;
        document.getElementById('visibility').textContent = `${visibility} km`;
        document.getElementById('feelsLike').textContent = `${feelsLike}°${this.currentUnit === 'metric' ? 'C' : 'F'}`;
    }

    async displayForecast(city) {
        try {
            const forecastData = await this.fetchForecast(city);
            const forecastContainer = document.getElementById('forecastContainer');
            
            // Get next 5 days (skip today)
            const dailyForecasts = this.getDailyForecasts(forecastData.list);
            
            forecastContainer.innerHTML = dailyForecasts.map(day => this.createForecastHTML(day)).join('');
        } catch (error) {
            console.error('Error fetching forecast:', error);
        }
    }

    getDailyForecasts(forecastList) {
        const dailyData = {};
        
        forecastList.forEach(item => {
            const date = new Date(item.dt * 1000);
            const dayKey = date.toDateString();
            
            if (!dailyData[dayKey]) {
                dailyData[dayKey] = {
                    date: date,
                    temps: [],
                    descriptions: [],
                    icons: []
                };
            }
            
            dailyData[dayKey].temps.push(item.main.temp);
            dailyData[dayKey].descriptions.push(item.weather[0].description);
            dailyData[dayKey].icons.push(item.weather[0].icon);
        });
        
        // Get next 5 days
        const today = new Date();
        const next5Days = [];
        
        for (let i = 1; i <= 5; i++) {
            const futureDate = new Date(today);
            futureDate.setDate(today.getDate() + i);
            const dayKey = futureDate.toDateString();
            
            if (dailyData[dayKey]) {
                const dayData = dailyData[dayKey];
                next5Days.push({
                    date: dayData.date,
                    temp: Math.round(dayData.temps.reduce((a, b) => a + b) / dayData.temps.length),
                    description: dayData.descriptions[0],
                    icon: dayData.icons[0]
                });
            }
        }
        
        return next5Days;
    }

    createForecastHTML(day) {
        return `
            <div class="forecast-item">
                <div class="forecast-date">${this.formatDate(day.date, true)}</div>
                <div class="forecast-icon">
                    <i class="${this.getWeatherIcon(day.icon)}"></i>
                </div>
                <div class="forecast-temp">${day.temp}°${this.currentUnit === 'metric' ? 'C' : 'F'}</div>
                <div class="forecast-desc">${day.description}</div>
            </div>
        `;
    }

    getWeatherIcon(iconCode) {
        const iconMap = {
            '01d': 'fas fa-sun',
            '01n': 'fas fa-moon',
            '02d': 'fas fa-cloud-sun',
            '02n': 'fas fa-cloud-moon',
            '03d': 'fas fa-cloud',
            '03n': 'fas fa-cloud',
            '04d': 'fas fa-cloud',
            '04n': 'fas fa-cloud',
            '09d': 'fas fa-cloud-rain',
            '09n': 'fas fa-cloud-rain',
            '10d': 'fas fa-cloud-sun-rain',
            '10n': 'fas fa-cloud-moon-rain',
            '11d': 'fas fa-bolt',
            '11n': 'fas fa-bolt',
            '13d': 'fas fa-snowflake',
            '13n': 'fas fa-snowflake',
            '50d': 'fas fa-smog',
            '50n': 'fas fa-smog'
        };
        
        return iconMap[iconCode] || 'fas fa-sun';
    }

    setUnit(unit) {
        this.currentUnit = unit;
        
        // Update button states
        document.getElementById('celsiusBtn').classList.toggle('active', unit === 'metric');
        document.getElementById('fahrenheitBtn').classList.toggle('active', unit === 'imperial');
        
        // Refresh weather if city is loaded
        if (this.currentCity) {
            this.searchWeather();
        }
    }

    formatDate(date, short = false) {
        const options = short 
            ? { weekday: 'short', month: 'short', day: 'numeric' }
            : { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
        
        return date.toLocaleDateString('en-US', options);
    }

    showLoading() {
        document.getElementById('loading').style.display = 'block';
        document.getElementById('weatherContent').style.display = 'none';
        document.getElementById('error').style.display = 'none';
    }

    hideLoading() {
        document.getElementById('loading').style.display = 'none';
    }

    showError(message) {
        document.getElementById('errorMessage').textContent = message;
        document.getElementById('error').style.display = 'block';
        document.getElementById('loading').style.display = 'none';
        document.getElementById('weatherContent').style.display = 'none';
    }

    hideError() {
        document.getElementById('error').style.display = 'none';
    }

    saveLastLocation(city) {
        localStorage.setItem('lastLocation', city);
    }

    loadLastLocation() {
        const lastLocation = localStorage.getItem('lastLocation');
        if (lastLocation) {
            document.getElementById('cityInput').value = lastLocation;
        }
    }
}

// Initialize the app when DOM is loaded
document.addEventListener('DOMContentLoaded', () => {
    new WeatherApp();
});
```

## API Setup

### Getting an API Key

1. **Sign up at OpenWeatherMap**: Go to [openweathermap.org](https://openweathermap.org)
2. **Create an account** and verify your email
3. **Get your API key** from the dashboard
4. **Replace 'YOUR_API_KEY'** in the JavaScript code with your actual API key

### API Endpoints Used

- **Current Weather**: `https://api.openweathermap.org/data/2.5/weather`
- **5-Day Forecast**: `https://api.openweathermap.org/data/2.5/forecast`

## Features Implemented

### Core Features
- ✅ Current weather display
- ✅ 5-day forecast
- ✅ City search functionality
- ✅ Current location detection
- ✅ Temperature unit toggle (Celsius/Fahrenheit)
- ✅ Weather icons and descriptions
- ✅ Responsive design

### Advanced Features
- ✅ Geolocation support
- ✅ Local storage for last searched city
- ✅ Error handling and loading states
- ✅ Weather details (humidity, wind, visibility, feels like)
- ✅ Responsive grid layout
- ✅ Smooth animations and transitions

## How to Use

1. **Search by City**: Enter a city name and click search or press Enter
2. **Current Location**: Click "Current Location" to get weather for your location
3. **Temperature Units**: Toggle between Celsius and Fahrenheit
4. **View Forecast**: Scroll down to see the 5-day forecast
5. **Weather Details**: View additional information like humidity, wind speed, etc.

## Key Learning Points

### API Integration
- Making HTTP requests with fetch API
- Handling API responses and errors
- Working with external data sources
- API key management and security

### Geolocation
- Using the Geolocation API
- Handling location permissions
- Converting coordinates to weather data

### Data Processing
- Transforming API data for display
- Grouping and filtering forecast data
- Calculating averages and statistics

### User Experience
- Loading states and error handling
- Responsive design principles
- Interactive elements and feedback
- Local storage for persistence

## Extensions and Improvements

### Possible Enhancements
1. **Weather Maps**: Add interactive weather maps
2. **Weather Alerts**: Display severe weather warnings
3. **Historical Data**: Show weather trends over time
4. **Multiple Locations**: Save and switch between favorite cities
5. **Weather Widgets**: Create embeddable weather widgets
6. **Offline Support**: Cache weather data for offline viewing
7. **Weather Notifications**: Browser notifications for weather updates

### Code Improvements
1. **Error Handling**: More comprehensive error handling
2. **Performance**: Implement caching and optimization
3. **Accessibility**: Add ARIA labels and keyboard navigation
4. **Testing**: Add unit tests for core functionality
5. **TypeScript**: Add type safety with TypeScript

## Common Issues and Solutions

### Issue 1: API Key Not Working
**Solution**: Ensure your API key is correct and activated

### Issue 2: CORS Errors
**Solution**: Use HTTPS and ensure your domain is registered with the API provider

### Issue 3: Geolocation Not Working
**Solution**: Check browser permissions and use HTTPS

### Issue 4: Weather Icons Not Displaying
**Solution**: Ensure Font Awesome is loaded and icon classes are correct

## Next Steps

1. **Get your API key** from OpenWeatherMap
2. **Test the application** with different cities
3. **Customize the design** to match your preferences
4. **Add additional features** from the enhancement list
5. **Deploy the application** to a web server
6. **Add error handling** for network issues

---

**Congratulations! You've built a complete Weather application! 🌤️**

This project demonstrates API integration, geolocation, data processing, and responsive design. Use this as a foundation to build more complex weather applications!

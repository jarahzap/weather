# Weather MicroService
# Please send get requests with city and date parameters to https://weathermicro-52a3a2f0a6a1.herokuapp.com
  Note: Date parameter is within 5 days of current date
# Use /weather as the endpoint
# Responses will be sent as a json with the following data:   	
  Date:	Date requested
  Description:	Descitption of forecast eg. "broken clouds"
  Max Temp: Maximum Temperature for that day in degrees farenheit
  Min Temp:	Minimum Temperature for that day in degrees farenheit
  Temp:	Average Temperature for that day in degrees farenheit
  Wind:	
    deg:	Wind degree
    gust:	Wind Gust
    speed:	Wind Speed
# Example call using React
import React, { useState } from 'react';

const WeatherComponent = () => {
  const [city, setCity] = useState('');
  const [date, setDate] = useState('');
  const [weatherData, setWeatherData] = useState(null);

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    if (name === 'city') {
      setCity(value);
    } else if (name === 'date') {
      setDate(value);
    }
  };

  const fetchWeatherData = async () => {
    try {
      const response = await fetch(
        `https://your-app-name.herokuapp.com/weather?city=${encodeURIComponent(
          city
        )}&date=${encodeURIComponent(date)}`
      );

      if (!response.ok) {
        throw new Error('Weather data not found for the specified city and date.');
      }

      const data = await response.json();
      setWeatherData(data);
    } catch (error) {
      console.error('Error fetching weather data:', error.message);
    }
  };

  return (
    <div>
      <input
        type="text"
        name="city"
        placeholder="Enter City"
        value={city}
        onChange={handleInputChange}
      />
      <input
        type="date"
        name="date"
        value={date}
        onChange={handleInputChange}
      />
      <button onClick={fetchWeatherData}>Get Forecast</button>

      {weatherData && (
        <div>
          <h3>Weather Information for {city} on {date}</h3>
          <p>Temperature: {weatherData.Temp} °C</p>
          <p>Max Temperature: {weatherData['Max Temp']} °C</p>
          <p>Min Temperature: {weatherData['Min Temp']} °C</p>
          <p>Description: {weatherData.Description}</p>
          <p>Wind: {JSON.stringify(weatherData.Wind)}</p>
        </div>
      )}
    </div>
  );
};

export default WeatherComponent;
![UML](https://github.com/jarahzap/weather/assets/102558003/fbdd5973-52aa-442c-b86b-a9e4d917faeb)


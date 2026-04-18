<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather Outfit Suggester</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>What Should I Wear? 👕</h1>
        <div class="search">
            <input type="text" id="cityInput" placeholder="Enter City Name...">
            <button id="searchBtn">Check</button>
        </div>
        <div class="result" id="result">
            <h2 id="cityName">---</h2>
            <p id="temp">--°C</p>
            <p id="description">Weather condition</p>
            <div class="suggestion-box">
                <strong>Suggestion:</strong>
                <p id="outfitSuggestion">Enter a city to get suggestions!</p>
            </div>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>
body { font-family: 'Arial', sans-serif; background: #f0f2f5; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; }
.container { background: white; padding: 2rem; border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); text-align: center; width: 350px; }
input { padding: 10px; border: 1px solid #ddd; border-radius: 5px; width: 70%; }
button { padding: 10px; background: #007bff; color: white; border: none; border-radius: 5px; cursor: pointer; }
.suggestion-box { margin-top: 20px; padding: 15px; background: #e7f3ff; border-radius: 10px; color: #0056b3; }






const apiKey = "44fe55f2cc0c68d8882b77b7a97a1b29"; // Yahan apni API key daalein
const searchBtn = document.getElementById("searchBtn");

searchBtn.addEventListener("click", () => {
    const city = document.getElementById("cityInput").value;
    if (city) {
        getWeather(city);
    }
});

async function getWeather(city) {
    const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`;
    try {
        const response = await fetch(url);
        const data = await response.json();
        
        if (data.cod === 200) {
            updateUI(data);
        } else {
            alert("City not found!");
        }
    } catch (error) {
        console.error("Error fetching data", error);
    }
}

function updateUI(data) {
    const temp = Math.round(data.main.temp);
    document.getElementById("cityName").innerText = data.name;
    document.getElementById("temp").innerText = `${temp}°C`;
    document.getElementById("description").innerText = data.weather[0].description;

    let suggestion = "";
    if (temp < 15) {
        suggestion = "Thand hai! Jacket aur hoodie pehen lo. 🧥";
    } else if (temp >= 15 && temp < 25) {
        suggestion = "Mausam sahi hai. Full sleeves t-shirt ya light shirt thik rahegi. 👕";
    } else {
        suggestion = "Garmi hai! Cotton t-shirt aur sunglasses cool lagenge. ☀️";
    }
    
    document.getElementById("outfitSuggestion").innerText = suggestion;
}





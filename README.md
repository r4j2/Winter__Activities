# Winter__Activities
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Winter Activities</title>
    <link rel="stylesheet" href="styles.css">
    <script src="script.js" defer></script>
    <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&libraries=places" defer></script>
</head>
<body>
    <div class="container">
        <h1>Winter Activities</h1>
        <p>Select a winter activity below to find the nearest place offering it!</p>
        <div class="activities">
            <button onclick="findActivity('ski_resort')">Skiing</button>
            <button onclick="findActivity('ice_skating_rink')">Ice Skating</button>
            <button onclick="findActivity('snowboarding_area')">Snowboarding</button>
        </div>
    </div>
</body>
</html>

/* styles.css */
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #f0f8ff;
    color: #333;
}
.container {
    margin: 50px;
}
.activities button {
    padding: 10px 20px;
    margin: 10px;
    border: none;
    background-color: #007BFF;
    color: white;
    cursor: pointer;
    border-radius: 5px;
}
.activities button:hover {
    background-color: #0056b3;
}

// script.js
function findActivity(activityType) {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
            const lat = position.coords.latitude;
            const lng = position.coords.longitude;
            const map = new google.maps.Map(document.createElement('div'));
            const service = new google.maps.places.PlacesService(map);

            service.nearbySearch({
                location: { lat, lng },
                radius: 5000,
                keyword: activityType
            }, (results, status) => {
                if (status === google.maps.places.PlacesServiceStatus.OK && results.length > 0) {
                    window.location.href = `https://www.google.com/maps/search/?api=1&query=${results[0].geometry.location.lat()},${results[0].geometry.location.lng()}`;
                } else {
                    alert('No results found nearby.');
                }
            });
        }, error => {
            alert('Error retrieving location. Please ensure location services are enabled.');
        });
    } else {
        alert("Geolocation is not supported by your browser.");
    }
}

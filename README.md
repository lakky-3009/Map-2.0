<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Map 2.0</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <style>
        /* Fullscreen map styling */
        #map {
            height: 100vh;
            width: 100%;
        }
        /* Search bar styling */
        .search-container {
            position: absolute;
            top: 10px;
            left: 10px;
            z-index: 1000;
            background: white;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
        }
        .search-container input {
            width: 200px;
            padding: 5px;
        }
        .search-container button {
            padding: 5px 10px;
            margin-left: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <!-- Search Bar -->
    <div class="search-container">
        <input id="location-search" type="text" placeholder="Search location" />
        <button onclick="searchLocation()">Search</button>
    </div>

    <!-- Map Container -->
    <div id="map"></div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        
        const map = L.map('map').setView([51.505, -0.09], 13);

        
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            maxZoom: 19,
            attribution: '© OpenStreetMap contributors'
        }).addTo(map);

        
        let marker = L.marker([51.505, -0.09]).addTo(map).bindPopup("Default Marker").openPopup();

        
        async function searchLocation() {
            const searchQuery = document.getElementById('location-search').value;
            if (!searchQuery) {
                alert("Please enter a location!");
                return;
            }

            const url = `https://nominatim.openstreetmap.org/search?format=json&q=${searchQuery}`;
            const response = await fetch(url);
            const data = await response.json();

            if (data && data.length > 0) {
                const lat = data[0].lat;
                const lon = data[0].lon;

              
                map.setView([lat, lon], 13);
                marker.setLatLng([lat, lon]).bindPopup(`You searched: ${searchQuery}`).openPopup();
            } else {
                alert("Location not found!");
            }
        }
        map.on('click', function(e) {
            const { lat, lng } = e.latlng;
            L.marker([lat, lng]).addTo(map).bindPopup(`Lat: ${lat}, Lng: ${lng}`).openPopup();
        });
    </script>
</body>
</html>

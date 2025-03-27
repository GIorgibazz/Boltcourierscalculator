<!DOCTYPE html>
<html lang="ka">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>კურიer's ანაზღაურების კალკულატორი</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #0c2c1c, #2A9C64);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            background: white;
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.3);
            display: flex;
            justify-content: center;
            align-items: flex-start;
            width: 80%;
            gap: 30px;
        }

        .form-container, .map-container {
            width: 48%;
            padding: 20px;
            background: #fff;
            border-radius: 10px;
            box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.1);
        }

        input {
            padding: 12px;
            margin: 10px 0;
            width: 90%;
            border: 2px solid #2A9C64;
            border-radius: 8px;
            font-size: 16px;
        }

        button {
            padding: 12px 25px;
            cursor: pointer;
            background: #2A9C64;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 18px;
            transition: 0.3s;
        }

        button:hover {
            background: #74efaa;
            color: black;
        }

        #result {
            margin-top: 20px;
            font-size: 22px;
            font-weight: bold;
            color: #2A9C64;
        }

        img {
            display: block;
            margin: 0 auto 20px auto;
            width: 200px;
        }

        /* Map container */
        .map-container {
            height: 400px;
        }

    </style>
    <!-- OpenStreetMap JavaScript Library -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
</head>
<body>

    <div class="container">
        <!-- Form container -->
        <div class="form-container">
            <img src="https://i.ibb.co/mrvCMqFP/Bolt-Food-logo-horisontal-CMYK.png" alt="კომპანიის ლოგო">
            <h2>კურიერის ანაზღაურების კალკულატორი</h2>
            <label for="km">განვლილი კმ:</label>
            <input type="number" id="km" placeholder="შეიყვანე კმ">
            <br>
            <label for="rate">კმ-ის ფასი (₾):</label>
            <input type="text" id="rate" value="0.30" readonly>
            <br>
            <label for="multiplier">Zone Multiplier:</label>
            <input type="number" id="multiplier" step="0.01" placeholder="Multiplier">
            <br>
            <button onclick="calculatePayment()">გამოთვლა</button>
            <div id="result"></div>
        </div>

        <!-- Map container -->
        <div class="map-container" id="map"></div>
    </div>

    <script>
        // Initialize OpenStreetMap
        var map = L.map('map').setView([41.78559, 44.76392], 15); // Set the coordinates for the map view

        // Add OpenStreetMap tile layer
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);

        // Create draggable markers
        var startMarker = L.marker([41.78559, 44.76392], {draggable: true}).addTo(map)
            .bindPopup("<b>Start</b><br> ეს არის თქვენი საწყისი მდებარეობა.")
            .openPopup();

        var endMarker = L.marker([41.794, 44.775], {draggable: true}).addTo(map)
            .bindPopup("<b>End</b><br> ეს არის თქვენი დანიშნულების ადგილი.");

        // Function to calculate the distance and update the km input field
        function updateDistance() {
            var distance = map.distance(startMarker.getLatLng(), endMarker.getLatLng());
            document.getElementById('km').value = (distance / 1000).toFixed(2); // in kilometers
        }

        // Update distance when markers are moved
        startMarker.on('moveend', updateDistance);
        endMarker.on('moveend', updateDistance);

        // Initial distance update
        updateDistance();

        // Calculate Payment
        function calculatePayment() {
            let km = parseFloat(document.getElementById('km').value);
            let rate = 0.30;  // 固定价格 0.30₾
            let multiplier = document.getElementById('multiplier').value;

            if (km && multiplier) {
                let baseEarning = km * rate * multiplier;

                document.getElementById('result').innerText = `სულ ანაზღაურება: ₾${baseEarning.toFixed(2)}`;
            } else {
                document.getElementById('result').innerText = "გთხოვთ შეავსოთ ყველა ველი!";
            }
        }
    </script>

</body>
</html>

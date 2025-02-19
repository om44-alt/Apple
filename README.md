<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Health Tracker - Login</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #6e7c7e, #a9c1c4);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            color: #fff;
        }

        .container {
            background: rgba(255, 255, 255, 0.8);
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.2);
            max-width: 500px;
            width: 100%;
            animation: fadeIn 1s ease-in-out;
        }

        h1 {
            color: #4CAF50;
            font-size: 2.5rem;
            margin-bottom: 15px;
        }

        h2 {
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: #333;
        }

        p {
            font-size: 1.1rem;
            color: #555;
        }

        input, button {
            padding: 12px;
            margin: 10px 0;
            border: 2px solid #ddd;
            border-radius: 6px;
            width: 100%;
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        input:focus, button:hover {
            border-color: #4CAF50;
            background-color: #4CAF50;
            color: white;
        }

        button {
            background-color: #28a745;
            color: white;
            cursor: pointer;
            font-weight: bold;
        }

        button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }

        .form-container {
            display: flex;
            flex-direction: column;
        }

        .form-container input {
            margin-bottom: 15px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th, td {
            padding: 12px;
            text-align: center;
            border: 1px solid #ddd;
        }

        th {
            background-color: #4CAF50;
            color: white;
        }

        tr:nth-child(even) {
            background-color: #f9f9f9;
        }

        tr:hover {
            background-color: #e1f5e1;
        }

        @keyframes fadeIn {
            0% { opacity: 0; }
            100% { opacity: 1; }
        }
    </style>
</head>
<body>

    <!-- Login Page -->
    <div class="container" id="loginPage">
        <h1>Login</h1>
        <p>Enter your username to track your health data.</p>
        <input type="text" id="username" placeholder="Enter Username">
        <button onclick="login()">Login</button>
    </div>

    <!-- Health Tracker (Hidden until login) -->
    <div class="container" id="healthTracker" style="display: none;">
        <h1>Health Tracker</h1>
        <p>Welcome, <span id="userDisplay"></span>! <button onclick="logout()">Logout</button></p>

        <div class="form-container">
            <label for="date">Date:</label>
            <input type="date" id="date" required>

            <label for="weight">Weight (kg):</label>
            <input type="number" id="weight" required>

            <label for="calories">Calories Consumed:</label>
            <input type="number" id="calories" required>

            <label for="exercise">Exercise (mins):</label>
            <input type="number" id="exercise" required>

            <label for="water">Water Intake (liters):</label>
            <input type="number" step="0.1" id="water" required>

            <button type="submit" onclick="saveData()">Save Data</button>
        </div>
    </div>

    <!-- User's Health Records -->
    <div class="container" id="recordsContainer" style="display: none;">
        <h2>Your Health Records</h2>
        <table id="healthTable">
            <thead>
                <tr>
                    <th>Date</th>
                    <th>Weight (kg)</th>
                    <th>Calories</th>
                    <th>Exercise (mins)</th>
                    <th>Water (liters)</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
    </div>

    <script>
        let currentUser = null;

        function login() {
            let username = document.getElementById("username").value.trim();
            if (username === "") {
                alert("Please enter a username.");
                return;
            }

            currentUser = username;
            localStorage.setItem("currentUser", username);
            document.getElementById("userDisplay").innerText = username;

            document.getElementById("loginPage").style.display = "none";
            document.getElementById("healthTracker").style.display = "block";
            document.getElementById("recordsContainer").style.display = "block";

            displayRecords();
        }

        function logout() {
            localStorage.removeItem("currentUser");
            currentUser = null;
            document.getElementById("loginPage").style.display = "block";
            document.getElementById("healthTracker").style.display = "none";
            document.getElementById("recordsContainer").style.display = "none";
        }

        function saveData() {
            if (!currentUser) {
                alert("Please log in first.");
                return;
            }

            let date = document.getElementById("date").value;
            let weight = document.getElementById("weight").value;
            let calories = document.getElementById("calories").value;
            let exercise = document.getElementById("exercise").value;
            let water = document.getElementById("water").value;

            let healthData = { date, weight, calories, exercise, water };
            let storedData = JSON.parse(localStorage.getItem(currentUser)) || [];
            storedData.push(healthData);
            localStorage.setItem(currentUser, JSON.stringify(storedData));

            displayRecords();
            document.getElementById("healthForm").reset();
        }

        function displayRecords() {
            if (!currentUser) return;

            let tableBody = document.querySelector("#healthTable tbody");
            tableBody.innerHTML = "";
            
            let storedData = JSON.parse(localStorage.getItem(currentUser)) || [];
            
            storedData.forEach(record => {
                let row = `<tr>
                    <td>${record.date}</td>
                    <td>${record.weight}</td>
                    <td>${record.calories}</td>
                    <td>${record.exercise}</td>
                    <td>${record.water}</td>
                </tr>`;
                tableBody.innerHTML += row;
            });
        }

        document.addEventListener("DOMContentLoaded", function() {
            let savedUser = localStorage.getItem("currentUser");
            if (savedUser) {
                currentUser = savedUser;
                document.getElementById("userDisplay").innerText = currentUser;
                document.getElementById("loginPage").style.display = "none";
                document.getElementById("healthTracker").style.display = "block";
                document.getElementById("recordsContainer").style.display = "block";
                displayRecords();
            }
        });
    </script>

</body>
</html>

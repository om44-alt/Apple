<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Health Tracker - Login</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(45deg, #6a11cb, #2575fc);
            animation: gradientBackground 6s ease infinite;
            color: white;
            text-align: center;
            padding: 50px;
            margin: 0;
        }
        
        @keyframes gradientBackground {
            0% { background: linear-gradient(45deg, #6a11cb, #2575fc); }
            50% { background: linear-gradient(45deg, #ff7e5f, #feb47b); }
            100% { background: linear-gradient(45deg, #6a11cb, #2575fc); }
        }

        .container {
            background: rgba(255, 255, 255, 0.9);
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0px 15px 30px rgba(0, 0, 0, 0.1);
            max-width: 500px;
            margin: auto;
            transition: transform 0.3s ease-in-out;
        }

        .container:hover {
            transform: scale(1.05);
        }

        h1, h2 {
            color: #333;
            text-transform: uppercase;
            margin-bottom: 20px;
        }

        input, button {
            padding: 12px;
            margin: 8px 0;
            border: none;
            border-radius: 5px;
            width: 90%;
            font-size: 1rem;
            transition: background-color 0.3s ease;
        }

        input:focus, button:focus {
            outline: none;
            border: 2px solid #2575fc;
        }

        button {
            background-color: #28a745;
            color: white;
            cursor: pointer;
            transition: transform 0.3s ease;
        }

        button:hover {
            background-color: #218838;
            transform: scale(1.05);
        }

        button:active {
            transform: scale(1);
        }

        table {
            width: 100%;
            margin-top: 20px;
            border-collapse: collapse;
            animation: fadeIn 1s ease;
        }

        th, td {
            padding: 12px;
            border: 1px solid #ddd;
            text-align: center;
        }

        th {
            background-color: #2575fc;
            color: white;
        }

        tr:hover {
            background-color: #f1f1f1;
            transform: scale(1.02);
            transition: transform 0.3s ease;
        }

        @keyframes fadeIn {
            0% { opacity: 0; }
            100% { opacity: 1; }
        }

        .input-container {
            position: relative;
            margin-bottom: 20px;
        }

        .input-container input {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ddd;
            background-color: transparent;
            color: white;
            font-size: 1rem;
        }

        .input-container label {
            position: absolute;
            left: 10px;
            top: 12px;
            color: #aaa;
            transition: 0.3s;
        }

        .input-container input:focus + label,
        .input-container input:not(:placeholder-shown) + label {
            top: -8px;
            left: 10px;
            font-size: 0.85rem;
            color: #2575fc;
        }
    </style>
</head>
<body>

    <!-- Login Page -->
    <div class="container" id="loginPage">
        <h1>Login</h1>
        <p>Enter your username to track your health data.</p>
        <div class="input-container">
            <input type="text" id="username" placeholder=" " required>
            <label for="username">Enter Username</label>
        </div>
        <button onclick="login()">Login</button>
    </div>

    <!-- Health Tracker (Hidden until login) -->
    <div class="container" id="healthTracker" style="display: none;">
        <h1>Health Tracker</h1>
        <p>Welcome, <span id="userDisplay"></span>! <button onclick="logout()">Logout</button></p>

        <form id="healthForm">
            <div class="input-container">
                <input type="date" id="date" required>
                <label for="date">Date</label>
            </div>

            <div class="input-container">
                <input type="number" id="weight" required>
                <label for="weight">Weight (kg)</label>
            </div>

            <div class="input-container">
                <input type="number" id="calories" required>
                <label for="calories">Calories Consumed</label>
            </div>

            <div class="input-container">
                <input type="number" id="exercise" required>
                <label for="exercise">Exercise (mins)</label>
            </div>

            <div class="input-container">
                <input type="number" step="0.1" id="water" required>
                <label for="water">Water Intake (liters)</label>
            </div>

            <button type="submit">Save Data</button>
        </form>
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
            console.log("Login function called. Username:", username);  
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

        document.getElementById("healthForm").addEventListener("submit", function(event) {
            event.preventDefault();

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
        });

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

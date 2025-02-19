<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Health Tracker - Login</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #6e7c7e, #a9c1c4);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            color: #333;
            overflow: hidden;
        }

        .container {
            background: rgba(255, 255, 255, 0.9);
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.1);
            max-width: 100%;
            width: 90%;
            transition: transform 0.5s ease;
        }

        h1, h2 {
            color: #28a745;
            font-size: 24px;
            margin-bottom: 15px;
        }

        input, button {
            padding: 12px;
            margin: 8px 0;
            border: 1px solid #ccc;
            border-radius: 6px;
            width: 100%;
            font-size: 16px;
        }

        button {
            background-color: #28a745;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        button:hover {
            background-color: #218838;
        }

        table {
            width: 100%;
            margin-top: 20px;
            border-collapse: collapse;
        }

        th, td {
            padding: 10px;
            border: 1px solid #ddd;
            text-align: center;
        }

        th {
            background-color: #28a745;
            color: white;
        }

        .form-section, .record-section {
            display: none;
        }

        .show {
            display: block;
        }

        /* Mobile view adjustments */
        @media (max-width: 768px) {
            body {
                padding: 10px;
            }

            .container {
                padding: 20px;
            }

            h1, h2 {
                font-size: 20px;
            }

            input, button {
                padding: 10px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>

    <!-- Login Page -->
    <div class="container form-section" id="loginPage">
        <h1>Login</h1>
        <p>Enter your username to track your health data.</p>
        <input type="text" id="username" placeholder="Enter Username">
        <button onclick="login()">Login</button>
    </div>

    <!-- Health Tracker (Hidden until login) -->
    <div class="container form-section" id="healthTracker">
        <h1>Health Tracker</h1>
        <p>Welcome, <span id="userDisplay"></span>! <button onclick="logout()">Logout</button></p>

        <form id="healthForm">
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

            <button type="submit">Save Data</button>
        </form>
    </div>

    <!-- User's Health Records -->
    <div class="container record-section" id="recordsContainer">
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

            document.getElementById("loginPage").classList.remove("show");
            document.getElementById("healthTracker").classList.add("show");
            document.getElementById("recordsContainer").classList.add("show");

            displayRecords();
        }

        function logout() {
            localStorage.removeItem("currentUser");
            currentUser = null;
            document.getElementById("loginPage").classList.add("show");
            document.getElementById("healthTracker").classList.remove("show");
            document.getElementById("recordsContainer").classList.remove("show");
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
                document.getElementById("loginPage").classList.remove("show");
                document.getElementById("healthTracker").classList.add("show");
                document.getElementById("recordsContainer").classList.add("show");
                displayRecords();
            }
        });
    </script>

</body>
</html>

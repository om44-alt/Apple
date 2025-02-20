<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Health Tracker - Login</title>
    <style>
        body { font-family: Arial, sans-serif; background-color: #f4f4f4; text-align: center; padding: 20px; }
        .container { background: white; padding: 20px; border-radius: 10px; box-shadow: 0px 0px 10px gray; max-width: 500px; margin: auto; }
        h1, h2 { color: #333; }
        input, button { padding: 10px; margin: 5px 0; border: 1px solid #ccc; border-radius: 5px; width: 90%; }
        button { background-color: #28a745; color: white; cursor: pointer; }
        button:hover { background-color: #218838; }
        table { width: 100%; margin-top: 20px; border-collapse: collapse; }
        th, td { padding: 10px; border: 1px solid #ddd; text-align: center; }
        th { background-color: #28a745; color: white; }
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

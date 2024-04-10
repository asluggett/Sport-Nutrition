# Sport-Nutriton
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nutrition Carbohydrate Calculator</title>
    <style>
        /* Add your CSS styles here */
    </style>
</head>
<body>
<h1>Pre-Training Carbohydrate Calculator</h1>
<p>
    For calculating individual carbohydrate requirements for before or after
    training. How to use this calculator: For session intensity, using 5-zone
    model...
</p>

<form id="nutritionForm">
    <label for="mass">Mass (kg):</label>
    <input type="number" id="mass" name="mass" min="1" step="0.1" required/><br/><br/>

    <label for="hours">Number of hours before training:</label>
    <input type="number" id="hours" name="hours" min="0" required/>
    <label for="minutes">Number of minutes before training:</label>
    <input type="number" id="minutes" name="minutes" min="0" max="59" step="1" required/>
    <br/><br/>

    <button type="submit">Calculate</button>
</form>

<div id="result"></div>

<script>
    // JavaScript code goes here
    document.getElementById("nutritionForm").addEventListener("submit", function (event) {
        event.preventDefault(); // Prevent form submission for now

        var mass = parseFloat(document.getElementById("mass").value); // Get mass input
        var hours = parseInt(document.getElementById("hours").value); // Get hours input
        var minutes = parseInt(document.getElementById("minutes").value); // Get minutes input

        // Validate inputs
        if (isNaN(mass) || isNaN(hours) || isNaN(minutes) || hours < 0 || minutes < 0 || minutes >= 60) {
            // Handle invalid input
            document.getElementById("result").innerHTML = "Please enter valid values for mass, hours, and minutes.";
            return;
        }

        // Calculate total minutes
        var preTrainingTime = hours * 60 + minutes;

        // Calculate total hours including fractional parts due to minutes
        var totalHours = Math.floor(preTrainingTime / 60); // Get the total number of hours
        var remainingMinutes = preTrainingTime % 60; // Get the remaining minutes after calculating hours
        var hours = totalHours + remainingMinutes / 60;

        // Calculate carbohydrate intake based on pre-training time and mass
        var CHO = calculatePreTrainingCHO(preTrainingTime, mass);

        // Calculate carbohydrate intake range
        var result = calculateCHOrange(CHO, hours);

        // Display the result
        document.getElementById("result").innerHTML = result;
    });

    // Function to calculate carbohydrate intake based on pre-training time and mass
    function calculatePreTrainingCHO(preTrainingTime, mass) {
        var CHO;
        var hours = preTrainingTime / 60;

        if (hours < 1 + 0.001) {
            // If less than 1 hour before training
            CHO = 1 * hours * mass; // Convert to number
        } else if (hours >= 1 && hours <= 2) {
            // If between 1 and 2 hours before training
            CHO = 1 * hours * mass; // Convert to number
        } else if (hours > 2 && hours <= 3) {
            // If between 2 and 3 hours before training
            CHO = 2 * hours * mass; // Convert to number
        } else if (hours > 3 && hours <= 4) {
            // If between 3 and 4 hours before training
            CHO = 3 * hours * mass; // Convert to number
        } else if (hours > 4) {
            // If more than 4 hours before training
            CHO = 4 * hours * mass; // Convert to number
        }

        return CHO; // Convert to string after all calculations are done
    }

    // Function to calculate carbohydrate intake range
    function calculateCHOrange(CHO, hours) {
        var lowerCHO = CHO - CHO * 0.1;
        var result = "";

        if (Math.floor(hours) < 1) {
            result +=
                "Given you are exercising in less than 1 hour, you should consume: " +
                CHO.toFixed(0) +
                " grams of carbohydrate.<br/><br/>However, if you are prone to gastrointestinal issues, consider consuming less.<br/><br/>For next time, we recommend you consume your final meal more than 1 hour prior to exercise.<br/><br/>";
        }

        result +=
            "Based on the amount of time before training, you can consume about " +
            CHO.toFixed(0) +
            " grams of carbohydrate.<br/><br/>";
        result += "We recommend the following range to also be suitable:<br/>";
        result +=
            lowerCHO.toFixed(0) +
            " to " +
            CHO.toFixed(0) +
            " grams of carbohydrate";
        return result;
    }
</script>
</body>
</html>

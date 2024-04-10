# Sport-Nutriton
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Nutrition Carbohydrate Calculator</title>
    <style>
      /* Add your CSS styles here */
    </style>
  </head>

  <body>
    <h1>Pre-Training Carbohydrate Calculator</h1>
    <p>
      For calculating individual carbohydrate requirements for before training.
    </p>

    <form id="nutritionForm">
      <label for="mass">Mass (kg):</label>
      <input
        type="number"
        id="mass"
        name="mass"
        min="1"
        step="0.1"
        required
      /><br /><br />

      <label for="hours">Number of hours before training:</label>
      <input type="number" id="hours" name="hours" min="0" required />
      <label for="minutes">Number of minutes before training:</label>
      <input
        type="number"
        id="minutes"
        name="minutes"
        min="0"
        max="59"
        step="1"
        required
      />
      <br /><br />

      <button type="submit">Calculate</button>
    </form>

    <div id="result"></div>
    <script>
      // JavaScript code goes here
      document
        .getElementById("nutritionForm")
        .addEventListener("submit", function (event) {
          event.preventDefault(); // Prevent form submission for now
          var mass = parseFloat(document.getElementById("mass").value); // Get mass input
          var hours = parseInt(document.getElementById("hours").value); // Get hours input
          var minutes = parseInt(document.getElementById("minutes").value); // Get minutes input


          // Calculate total minutes
          var preTrainingTime = hours * 60 + minutes;
          // Calculate total hours including fractional parts due to minutes
          var totalHours = Math.floor(preTrainingTime / 60); // Get the total number of hours
          var remainingMinutes = preTrainingTime % 60; // Get the remaining minutes after calculating hours
          // Correctly calculate total hours
          var hoursWithMinutes = totalHours + remainingMinutes / 60;
          // Check if there are any minutes entered to decide whether to use hoursWithMinutes or just totalHours
          var hours = remainingMinutes > 0 ? hoursWithMinutes : totalHours;
          // Check if the input is a valid number
          if (!isNaN(preTrainingTime)) {
            // Do something with the preTrainingTime value, for example, display it
            console.log(
              "Minutes before training: " +
                preTrainingTime +
                " minutes, about " +
                hours.toFixed(1) +
                " hours"
            );
            // Calculate carbohydrate intake based on pre-training time and mass
            var CHO = calculatePreTrainingCHO(preTrainingTime, mass);
            console.log(
              "Based on your body mass of: " +
                mass +
                " your calculated carbohydrate intake to consume before training is: " +
                CHO.toFixed(0) +
                " grams"
            );
            // Calculate carbohydrate intake range
            var result = calculateCHOrange(CHO, hours);
            console.log(result);
          } else {
            // Handle invalid input
            console.error(
              "Invalid input. Please enter valid numbers for hours and minutes."
            );
          }
          // Function to calculate carbohydrate intake based on pre-training time and mass
          function calculatePreTrainingCHO(preTrainingTime, mass) {
            var CHO;
            var hours = preTrainingTime / 60;
            if (hours < 1 + 0.001) {
              // If less than 1 hour before training
              CHO = 1 * mass; // Convert to number
            } else if (hours >= 1 && hours <= 2) {
              // If between 1 and 2 hours before training
              CHO = 1 * mass; // Convert to number
            } else if (hours > 2 && hours <= 3) {
              // If between 2 and 3 hours before training
              CHO = 2 * mass; // Convert to number
            } else if (hours > 3 && hours <= 4) {
              // If between 3 and 4 hours before training
              CHO = 3 * mass; // Convert to number
            } else if (hours > 4) {
              // If more than 4 hours before training
              CHO = 4 * mass; // Convert to number
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
                " grams of carbohydrate.\nHowever, if you are prone to gastrointestinal issues, consider consuming less.\nFor next time, we recommend you consume your final meal more than 1 hour prior to exercise.\n\n";
            }
            result +=
              "Based on the amount of time before training, you can consume about " +
              CHO.toFixed(0) +
              " grams of carbohydrate.\n";
            result += "We recommend the following range to also be suitable:\n";
            result +=
              lowerCHO.toFixed(0) +
              " to " +
              CHO.toFixed(0) +
              " grams of carbohydrate";
            return result;
          }
        });
    </script>
  </body>
</html>

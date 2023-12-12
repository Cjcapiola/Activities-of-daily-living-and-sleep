## Enter Your Activity Data

<form id="activityForm">
  <label for="intensityMinutes">Intensity Minutes (per day):</label><br>
  <input type="number" id="intensityMinutes" name="intensityMinutes" required><br>

  <label for="swimming">Number of Swimming Sessions (per week):</label><br>
  <input type="number" id="swimming" name="swimming" required><br><br>

  <input type="button" value="Get Recommendations" onclick="getRecommendations()">
</form>

<div id="recommendations">
  <!-- Recommendations will be displayed here -->
</div>
<script>
  function getRecommendations() {
    var intensityMinutes = document.getElementById('intensityMinutes').value;
    var swimming = document.getElementById('swimming').value;
    var recommendations = '';

    // Based on research findings
    if (intensityMinutes >= 30) {
      recommendations += 'Your intensity minutes are good. Keep it up!<br>';
    } else {
      recommendations += 'Increase your daily intensity minutes to at least 30.<br>';
    }

    if (swimming >= 2) {
      recommendations += 'Great amount of swimming sessions!';
    } else {
      recommendations += 'Consider adding more swimming sessions to your routine, at least 2 per week.';
    }

    document.getElementById('recommendations').innerHTML = recommendations;
  }
</script>

# FinalProjectCSE

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Complaints Box</title>
  <style>
    body { font-family: Arial, Georgia, 'Times New Roman', Times, serif; margin: 20px; }
    .container { max-width: 600px; margin: auto; text-align: center; }
    h1 { font-family: 'Courier New', Courier, monospace; font-size: 2em; }
    #topic-description { font-family: 'Verdana', sans-serif; font-size: 1.2em; }
    .question { margin: 20px 0; font-size: 1.2em; font-family: 'Tahoma', sans-serif; }
    .response { margin: 10px 0; width: 80%; }
  </style>
</head>
<body>
  <div class="container">
    <h1>Complaints and Feedback Box</h1>
    <p id="topic-description">Please provide your complaint or feedback. Your input helps improve the office environment.</p>

    <div id="question-container">
      <p id="question" class="question"></p>
      <textarea id="response" class="response" placeholder="Write your complaint or feedback here"></textarea>
      <button onclick="nextQuestion()">Submit</button>
    </div>

    <button onclick="resetData()">Reset</button>
    <button onclick="exportData()">Export as CSV</button>

    <canvas id="chart" width="400" height="200"></canvas>
  </div>

  <script>
    const questions = ["Please describe your complaint or feedback:"];
    let currentQuestionIndex = 0;
    let responses = JSON.parse(localStorage.getItem('responses')) || [];

    // Ensure responses is an array of arrays
    if (!Array.isArray(responses) || !responses.every(row => Array.isArray(row))) {
      responses = [];
    }

    // Load the first question
    document.getElementById("question").textContent = questions[currentQuestionIndex];

    function nextQuestion() {
      const responseInput = document.getElementById("response");
      const response = responseInput.value.trim();

      // Validate that the response is not empty
      if (response === '') {
        alert("Please provide some feedback.");
        return;
      }

      // Add the response to the list
      responses.push([response]);

      localStorage.setItem('responses', JSON.stringify(responses));

      alert("Your feedback has been submitted! Thank you.");

      // Clear the input field
      responseInput.value = '';
    }

    function resetData() {
      responses = [];
      localStorage.removeItem('responses');
      alert("Data has been reset!");
      document.getElementById("response").value = '';
    }

    function exportData() {
      // Ensure responses is well-structured
      if (!Array.isArray(responses) || !responses.every(row => Array.isArray(row))) {
        alert("Data structure error. Resetting data.");
        resetData();
        return;
      }

      const csvContent = "data:text/csv;charset=utf-8," + responses.map(row => row.join(",")).join("\n");
      const encodedUri = encodeURI(csvContent);
      const link = document.createElement("a");
      link.setAttribute("href", encodedUri);
      link.setAttribute("download", "feedback.csv");
      document.body.appendChild(link);
      link.click();
    }
  </script>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FlipCode Calculator</title>
  <style>
    :root {
      --primary-color: #00ffff;
      --secondary-color: #00ff9f;
      --background-dark: #1e1e1e;
      --background-light: #2a2a2a;
      --text-light: #ffffff;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      background: var(--background-dark);
      color: var(--secondary-color);
      font-family: 'Courier New', monospace;
      margin: 0;
      padding: 0;
      line-height: 1.6;
    }

    .container {
      width: 100%;
      max-width: 600px;
      margin: 0 auto;
      padding: 20px;
    }

    header {
      text-align: center;
      padding: 20px 0;
      border-bottom: 1px solid var(--primary-color);
      margin-bottom: 20px;
    }

    h1 {
      color: var(--primary-color);
      text-shadow: 0 0 10px rgba(0, 255, 255, 0.5);
      font-size: 2rem;
    }

    .calculator-form {
      display: grid;
      gap: 15px;
    }

    .form-group {
      display: flex;
      flex-direction: column;
    }

    label {
      color: var(--primary-color);
      margin-bottom: 5px;
      font-weight: bold;
    }

    input {
      width: 100%;
      padding: 12px;
      background: var(--background-light);
      border: 1px solid #444;
      border-radius: 5px;
      color: var(--text-light);
      font-size: 16px;
      transition: all 0.3s ease;
    }

    input:focus {
      border-color: var(--primary-color);
      box-shadow: 0 0 8px rgba(0, 255, 255, 0.5);
      outline: none;
    }

    button {
      margin-top: 10px;
      width: 100%;
      padding: 15px;
      background-color: var(--primary-color);
      color: var(--background-dark);
      border: none;
      font-weight: bold;
      font-size: 18px;
      border-radius: 5px;
      cursor: pointer;
      transition: all 0.3s ease;
      text-transform: uppercase;
      letter-spacing: 1px;
    }

    button:hover {
      background-color: #00cccc;
      box-shadow: 0 0 15px rgba(0, 255, 255, 0.7);
    }

    .results {
      margin-top: 25px;
      padding: 15px;
      background: rgba(0, 255, 255, 0.05);
      border-radius: 5px;
      border: 1px solid var(--primary-color);
      box-shadow: 0 0 10px rgba(0, 255, 255, 0.2);
      opacity: 0;
      transition: opacity 0.5s ease;
    }

    .results.visible {
      opacity: 1;
    }

    .result-row {
      display: flex;
      justify-content: space-between;
      margin: 10px 0;
      padding-bottom: 10px;
      border-bottom: 1px dashed rgba(0, 255, 255, 0.3);
    }

    .result-row:last-child {
      border-bottom: none;
    }

    .result-label {
      color: var(--primary-color);
      font-weight: bold;
    }

    .result-value {
      color: var(--text-light);
      font-weight: bold;
    }

    .highlight {
      color: var(--secondary-color);
      font-weight: bold;
      font-size: 1.1em;
    }

    .save-history {
      margin-top: 15px;
      padding: 10px;
      background: rgba(0, 255, 159, 0.1);
      border-radius: 5px;
      text-align: center;
    }

    .history-container {
      margin-top: 20px;
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.5s ease;
    }

    .history-container.visible {
      max-height: 500px;
      overflow-y: auto;
    }

    .history-item {
      padding: 10px;
      margin: 10px 0;
      background: var(--background-light);
      border-radius: 5px;
      border-left: 3px solid var(--primary-color);
    }

    .history-date {
      font-size: 0.8em;
      color: #888;
      text-align: right;
    }

    /* Mobile Responsiveness */
    @media (max-width: 768px) {
      .container {
        padding: 15px;
      }

      h1 {
        font-size: 1.5rem;
      }

      input, button {
        padding: 12px;
        font-size: 16px;
      }
    }

    /* Extra small devices */
    @media (max-width: 480px) {
      h1 {
        font-size: 1.3rem;
      }

      .calculator-form {
        gap: 10px;
      }

      .form-group {
        margin-bottom: 8px;
      }
    }

    /* Dark mode toggle */
    .theme-toggle {
      position: absolute;
      top: 20px;
      right: 20px;
      background: none;
      border: none;
      color: var(--primary-color);
      cursor: pointer;
      font-size: 24px;
      padding: 5px;
      width: auto;
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>FlipCode Calculator</h1>
    </header>

    <div class="calculator-form">
      <div class="form-group">
        <label for="weight">Weight (g):</label>
        <input type="number" id="weight" value="56.7" step="0.1">
      </div>

      <div class="form-group">
        <label for="cost">Cost ($):</label>
        <input type="number" id="cost" value="360" step="0.01">
      </div>

      <div class="form-group">
        <label for="bagSize">Bag Size (g):</label>
        <input type="number" id="bagSize" value="0.1" step="0.01">
      </div>

      <div class="form-group">
        <label for="bagPrice">Bag Price ($):</label>
        <input type="number" id="bagPrice" value="10" step="0.01">
      </div>

      <button id="calculateButton" onclick="runFlip()">Calculate Flip</button>
    </div>

    <div class="results" id="results">
      <div class="result-row">
        <span class="result-label">Number of Bags:</span>
        <span class="result-value" id="numBags">0</span>
      </div>
      <div class="result-row">
        <span class="result-label">Revenue:</span>
        <span class="result-value">$<span id="revenue">0.00</span></span>
      </div>
      <div class="result-row">
        <span class="result-label">Profit:</span>
        <span class="result-value">$<span id="profit" class="highlight">0.00</span></span>
      </div>
      <div class="result-row">
        <span class="result-label">ROI:</span>
        <span class="result-value"><span id="roi">0</span>%</span>
      </div>
      <div class="result-row">
        <span class="result-label">Reinvestable Weight:</span>
        <span class="result-value"><span id="reinvestWeight">0.00</span>g</span>
      </div>
      <div class="save-history" id="saveHistory">
        <button onclick="saveToHistory()">Save This Calculation</button>
      </div>
    </div>

    <div class="history-container" id="historyContainer">
      <h2 style="color: var(--primary-color); margin-bottom: 10px;">Calculation History</h2>
      <div id="historyItems"></div>
    </div>
  </div>

  <script>
    // Check if localStorage is available
    function isLocalStorageAvailable() {
      try {
        const test = 'test';
        localStorage.setItem(test, test);
        localStorage.removeItem(test);
        return true;
      } catch(e) {
        return false;
      }
    }

    // Load history from localStorage
    function loadHistory() {
      if (!isLocalStorageAvailable()) return [];
      
      const history = localStorage.getItem('flipCodeHistory');
      return history ? JSON.parse(history) : [];
    }

    // Save history to localStorage
    function saveHistory(history) {
      if (!isLocalStorageAvailable()) return;
      
      localStorage.setItem('flipCodeHistory', JSON.stringify(history));
    }

    // Display history items
    function displayHistory() {
      const history = loadHistory();
      const historyItems = document.getElementById('historyItems');
      historyItems.innerHTML = '';
      
      if (history.length === 0) {
        historyItems.innerHTML = '<p style="color: #888;">No saved calculations yet.</p>';
        return;
      }
      
      history.forEach((item, index) => {
        const historyItem = document.createElement('div');
        historyItem.className = 'history-item';
        historyItem.innerHTML = `
          <div class="result-row">
            <span class="result-label">Weight: ${item.weight}g</span>
            <span class="result-label">Cost: $${item.cost}</span>
          </div>
          <div class="result-row">
            <span class="result-label">Bag Size: ${item.bagSize}g</span>
            <span class="result-label">Bag Price: $${item.bagPrice}</span>
          </div>
          <div class="result-row">
            <span class="result-value">Profit: $${item.profit}</span>
            <span class="result-value">ROI: ${item.roi}%</span>
          </div>
          <div class="history-date">${new Date(item.date).toLocaleString()}</div>
          <div style="display: flex; justify-content: space-between; margin-top: 10px;">
            <button onclick="loadCalculation(${index})">Load</button>
            <button onclick="deleteCalculation(${index})">Delete</button>
          </div>
        `;
        historyItems.appendChild(historyItem);
      });
      
      // Show history container
      document.getElementById('historyContainer').classList.add('visible');
    }

    // Save current calculation to history
    function saveToHistory() {
      const weight = parseFloat(document.getElementById('weight').value);
      const cost = parseFloat(document.getElementById('cost').value);
      const bagSize = parseFloat(document.getElementById('bagSize').value);
      const bagPrice = parseFloat(document.getElementById('bagPrice').value);
      const profit = parseFloat(document.getElementById('profit').textContent);
      const roi = parseFloat(document.getElementById('roi').textContent);
      const reinvestWeight = parseFloat(document.getElementById('reinvestWeight').textContent);
      
      const calculation = {
        weight,
        cost,
        bagSize,
        bagPrice,
        profit,
        roi,
        reinvestWeight,
        date: new Date().toISOString()
      };
      
      const history = loadHistory();
      history.unshift(calculation); // Add to beginning of array
      
      // Limit history to 20 items
      if (history.length > 20) {
        history.pop();
      }
      
      saveHistory(history);
      displayHistory();
    }

    // Load a calculation from history
    function loadCalculation(index) {
      const history = loadHistory();
      const item = history[index];
      
      document.getElementById('weight').value = item.weight;
      document.getElementById('cost').value = item.cost;
      document.getElementById('bagSize').value = item.bagSize;
      document.getElementById('bagPrice').value = item.bagPrice;
      
      runFlip();
    }

    // Delete a calculation from history
    function deleteCalculation(index) {
      const history = loadHistory();
      history.splice(index, 1);
      saveHistory(history);
      displayHistory();
    }

    // Calculate flip
    function runFlip() {
      // Get input values
      const weight = parseFloat(document.getElementById('weight').value);
      const cost = parseFloat(document.getElementById('cost').value);
      const bagSize = parseFloat(document.getElementById('bagSize').value);
      const bagPrice = parseFloat(document.getElementById('bagPrice').value);
      
      // Validate inputs
      if (isNaN(weight) || isNaN(cost) || isNaN(bagSize) || isNaN(bagPrice) || 
          weight <= 0 || cost <= 0 || bagSize <= 0 || bagPrice <= 0) {
        alert("Please enter valid positive numbers for all fields.");
        return;
      }
      
      // Calculate results
      const numBags = weight / bagSize;
      const revenue = numBags * bagPrice;
      const profit = revenue - cost;
      const roi = ((profit / cost) * 100);
      const reinvestWeight = profit / (cost / weight);
      
      // Display results
      document.getElementById('numBags').textContent = numBags.toFixed(0);
      document.getElementById('revenue').textContent = revenue.toFixed(2);
      document.getElementById('profit').textContent = profit.toFixed(2);
      document.getElementById('roi').textContent = roi.toFixed(1);
      document.getElementById('reinvestWeight').textContent = reinvestWeight.toFixed(2);
      
      // Make results visible
      document.getElementById('results').classList.add('visible');
      
      // Vibrate on mobile devices for feedback
      if (navigator.vibrate) {
        navigator.vibrate(100);
      }
    }

    // Add event listeners for keyboard support
    document.addEventListener('DOMContentLoaded', function() {
      // Enable Enter key to calculate
      document.querySelectorAll('input').forEach(input => {
        input.addEventListener('keypress', function(e) {
          if (e.key === 'Enter') {
            runFlip();
          }
        });
      });
      
      // Display history if available
      if (loadHistory().length > 0) {
        displayHistory();
      }
    });
  </script>
</body>
</html>

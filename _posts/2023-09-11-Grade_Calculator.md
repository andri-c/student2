---
toc: false
comments: true
layout: post
title: Grade Calculator
description: This is a grade calculator the helps calculate your average grade.
courses: { compsci: {week: 4} }
type: hacks
---


<!-- Help Message -->
<h3>Input scores, press tab to add each new number.</h3>
<!-- Totals -->
<ul>
  <li>
    Total : <span id="total">0.0</span>
    Count : <span id="count">0.0</span>
    Average : <span id="average">0.0</span>
    Maximum : <span id="maximum">0.0</span>
    Minimum : <span id="minimum">0.0</span>
  </li>
</ul>
<!-- Rows added using scores ID -->
<div id="scores">
  <!-- javascript generated inputs -->
</div>

<!-- Clear button -->
<button id="clearButton">Clear</button>

<script>
// Variables to keep track of maximum and minimum scores
var maxScore = -Infinity;
var minScore = Infinity;

// Executes on input event and calculates totals
function calculator(event) {
    var key = event.key;
    // Check if the pressed key is the "Tab" key (key code 9) or "Enter" key (key code 13)
    if (key === "Tab" || key === "Enter") { 
        event.preventDefault(); // Prevent default behavior (tabbing to the next element)
   
        var array = document.getElementsByName('score'); // setup array of scores
        var total = 0;  // running total
        var count = 0;  // count of input elements with valid values

        for (var i = 0; i < array.length; i++) {  // iterate through array
            var value = array[i].value;
            if (parseFloat(value)) {
                var parsedValue = parseFloat(value);
                total += parsedValue;  // add to running total
                count++;

                // Update maximum and minimum scores
                if (parsedValue > maxScore) {
                    maxScore = parsedValue;
                }
                if (parsedValue < minScore) {
                    minScore = parsedValue;
                }
            }
        }

        // update totals, maximum, and minimum
        document.getElementById('total').innerHTML = total.toFixed(2); // show two decimals
        document.getElementById('count').innerHTML = count;

        if (count > 0) {
            document.getElementById('average').innerHTML = (total / count).toFixed(2);
            document.getElementById('maximum').innerHTML = maxScore.toFixed(2);
            document.getElementById('minimum').innerHTML = minScore.toFixed(2);
        } else {
            document.getElementById('average').innerHTML = "0.0";
            document.getElementById('maximum').innerHTML = "0.0";
            document.getElementById('minimum').innerHTML = "0.0";
        }

        // adds newInputLine, only if all array values satisfy parseFloat 
        if (count === document.getElementsByName('score').length) {
            newInputLine(count); // make a new input line
        }
    }
}

// Creates a new input box
function newInputLine(index) {
    // Add a label for each score element
    var title = document.createElement('label');
    title.htmlFor = index;
    title.innerHTML = index + ". ";    
    document.getElementById("scores").appendChild(title); // add to HTML

    // Setup score element and attributes
    var score = document.createElement("input"); // input element
    score.id =  index;  // id of input element
    score.onkeydown = calculator // Each key triggers event (using function as a value)
    score.type = "number"; // Use text type to allow typing multiple characters
    score.name = "score";  // name is used to group all "score" elements (array)
    score.style.textAlign = "right";
    score.style.width = "5em";
    document.getElementById("scores").appendChild(score);  // add to HTML

    // Create and add blank line after input box
    var br = document.createElement("br");  // line break element
    document.getElementById("scores").appendChild(br); // add to HTML

    // Set focus on the new input line
    document.getElementById(index).focus();
}

// Function to clear input fields and reset the table
function clearTable() {
    var scoresContainer = document.getElementById('scores');
    scoresContainer.innerHTML = ''; // Remove all existing input lines
    document.getElementById('total').innerHTML = '0.0';
    document.getElementById('count').innerHTML = '0.0';
    document.getElementById('average').innerHTML = '0.0';
    document.getElementById('maximum').innerHTML = '0.0'; // Reset maximum
    document.getElementById('minimum').innerHTML = '0.0'; // Reset minimum
    maxScore = -Infinity; // Reset maximum score
    minScore = Infinity; // Reset minimum score
    // Create a new input line after clearing
    newInputLine(0);
}

// Attach the clearTable function to the "Clear" button
var clearButton = document.getElementById('clearButton');
clearButton.addEventListener('click', clearTable);

// Creates 1st input box on Window load
newInputLine(0);

</script>

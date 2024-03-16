# JavaScript Quiz App
This quiz app works as a template for quiz functionality. Here's a quick summary of how the user progresses through the quiz in this template:
1. User presses "Start Quiz" button
2. Directions appear, informing the user of the rules
3. User begins quiz
4. Each question is on a 15 second timer, and only one answer can be selected
5. User completes the quiz and is shown how many answers out of 5 they got correct
6. User can retry or quit

Here's a few cases where this code would be useful:
- Collecting user feedback
- Quizzing your audience of their knowledge
- Adding a gamified element to your website

This template is only meant to be an example. The code has several features, outlined below, which can be tweaked or removed entirely. 

## Functional Features
- showQuestions: Gets each question from questions.js. The HTML itself of each option is written as a string inside que_tag and option_tag. These two variables are then passed into their respective parent HTML elements. 
- optionSelected: Handles the user's selected option. This pauses the counter, determines whether or not the user's choice is correct, and prevents the user from clicking on other options. If the user's choice is correct, then it turns green and adds 1 point to the user's score. If it's not correct, the choice turns red and the correct answer turns green. Finally, the next question button is shown. 
- showResult: Handles the end of the quiz of the quiz. First, the function hides the HTML that displays the quiz itself. Then, it shows the results HTML and performs logic to determine whether or not the user scored above 1 or below 1. It passes different HTML strings based on the logic.
- startTimer: Handles functionality for the timer. Utilizes the global method setInterval() to execute the timer function after 1000 ms. Timer is nested within this function, which determines when the user has run out of time. It decrements the value contained within the HTML of timeCount. Finally, if the time value becomes less than 0, similar logic occurs to when the user selects an incorrect answer.
- startTimerLine: Handles functionality for the time progress bar. This also has a nested timer function. Essentially, as the time value increments, the CSS property width of the progress bar is increased.
- queCounter: Gets the amount of questions present in questions.js and passes the number to an HTML string so that it can be displayed on the quiz. 
## File Structure

## Explanation of Codebase

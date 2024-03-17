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
- Quizzing your audience of their knowledge on a certain subject
- Adding a gamified element to your website

This template is only meant to be an example. The code has several features, outlined below, which can be tweaked or removed entirely. 

## Functional Features
- `showQuestions`: Gets each question from `questions.js`. The HTML itself of each option is written as a string inside `que_tag` and `option_tag`. These two variables are then passed into their respective parent HTML elements. 
- `optionSelected`: Handles the user's selected option. This pauses the counter, determines whether or not the user's choice is correct, and prevents the user from clicking on other options. If the user's choice is correct, then it turns green and adds 1 point to the user's score. If it's not correct, the choice turns red and the correct answer turns green. Finally, the next question button is shown. 
- `showResult`: Handles the end of the quiz. First, the function hides the HTML that displays the quiz itself. Then, it shows the results HTML and performs logic to determine whether or not the user scored above 1 or below 1. It passes different HTML strings based on the logic.
- `startTimer`: Handles functionality for the timer. Utilizes the global method `setInterval()` to execute the timer function after 1000 ms. Timer is nested within this function, which determines when the user has run out of time. It decrements the value contained within the HTML of `timeCount`. Finally, if the time value becomes less than 0, similar logic occurs to when the user selects an incorrect answer.
- `startTimerLine`: Handles functionality for the time progress bar. This also has a nested timer function. Essentially, as the time value increments, the CSS property width of the progress bar is increased.
- `queCounter`: Gets the amount of questions present in `questions.js` and passes the number to an HTML string so that it can be displayed on the quiz. 

## File Structure
The file structure of this application is rather simple. 

- `index.html` Is used to define all of the static elements of the quiz.
- `style.css` applies styling to the html.
- `quizApp.js` is the code part of the application that defines what happens when the user clicks buttons, what answers are correct, etc.
- `questions.js` is an array that defines the poperties of each question so that they can be used in `quizApp.js`. 

`index.html` Is linked to each of these files. `style.css` Is, obviously, linked as a stylesheet. The two JS files are linked in script tags using the `defer` attribute, which loads the JS after the webpage itself has finished loading. 

`quizApp.js` Interacts with `questions.js`. This interaction is required so that certain functions in `quizApp` can identify what question is being clicked on, what the correct answer is, how the suer is progressing through the quiz, etc. 

In addition, `quizApp` directly interacts with the DOM. There are several variables which store HTML elements based on their class names so that the JS code can get their values as well as modify the HTML using the `innerHTML()` method. 

## Explanation of Codebase

### index.html
Each of these boxes appear separately. That is, as the user progresses through them by clicking `.next-btn` the current "box" `div` is hidden using JS and CSS while the next one is shown. 

#### Instruction Box
This div provides no functionality other than to inform the user of the rules of the quiz. The parent div itself is split into three other divs so that the information can be organized within the box. 

#### Quiz Box
This is the main div that the user will see. It contains all of the questions as well as their options. The divs with class `que_text`, `option_list`, and `total_que` are empty so that their child elements can be dynamically generated using JS. 

#### Result Box
This is the final div the user sees. The div `score_text` is left empty so that its content can be dynamically generated. 

### style.css
If you would like to change the styling of the application, this is the file you will want to go to. 

As a note, a portion of the classes defined in this file will only affect content that is dynamically generated. If you're attempting to modify one of those classes, keep in my that you'll have to use the string in `quizApp.js` as a reference. Here's a list of all such occurences in `quizApp.js`:
- Line 93, `showQuestions` function
- Line 128 & 129, tick & cross icons
- line 132, `optionSelected` function
- Line 249, `queCounter` function 

The functionality for the "box" divs to be hidden and shown is done through this code: 
``` css
.info_box.activeInfo,
.quiz_box.activeQuiz,
.result_box.activeResult {
  opacity: 1;
  z-index: 5;
  pointer-events: auto;
  transform: translate(-50%, -50%) scale(1);
}
```
Since JS interacts directly with the DOM, all that needs to be done is to add or remove the class from its respective element. The main attribute to take note of is opacity. For example, in `.info-box` the opacity is set to 0, which makes the `div` completely invisible. When `.activeInfo` is added to the element the opacity is overriden to be 1. 

This same idea is applied to identifying correct and incorrect options. Here is the css for the `.correct` and `.incorrect` classes:

``` css
section .option_list .option.correct {
  color: #155724;
  background: #d4edda;
  border: 1px solid #c3e6cb;
}

section .option_list .option.incorrect {
  color: #721c24;
  background: #f8d7da;
  border: 1px solid #f5c6cb;
}
```

All JavaScript has to do is add these classes to the proper elements in HTML, since these classes will override any styles `.option` has. 

Some classes defined have pseudoclasses. Here is an example of a pseudoclass:
``` css
.buttons button.restart:hover {
  background: #8304d1;
}
```
Here is the original class:
``` css
.buttons button.restart {
  color: #fff;
  background: #a020f0;
}
```

The styles in the pseudoclass are applied when a certain condition is met. In this case, it's when the user hovers their cursor over the button.

You may also notice that this button lacks a space between it and its class, `.restart`. This is because `button.restart` selects all buttons that have the restart class, while if there was a space between `button` and `.restart` that would select all elements with the restart class that are children of a button. 

### questions.js
This file follows a structure that makes it easy to add in new questions. Here is a template for a new question:
``` javascript
{
    numb: n,
    question: "",
    answer: "",
    options: [
      "",
      "",
      "",
      "",
    ],
  },
```

To add it in, simply make a new line above or below the `// Duplicate the object declaration and initialization above for more questions` line and paste it. 

If you wish to have more than 4 options (or maybe less than 4), you will also have to modify the `showQuestions` function in `quizApp.js`, which is covered in the `quizApp.js` section below. As a disclaimer, each question must have the same amount of options each. This is due to how `showQuestions` works. 

As a slightly more in-depth explanation, this file is just a JSON array. It is accessed in `quizApp.js` to dynamically build the HTML required for each question and option to be displayed on the webpage. 

### quizApp.js
This is the file where you will have to go in order to change many of the functionalities of the app itself. 
All of the HTML elements are grabbed in the beginning by their class names. This is how JS directly interacts with the DOM. 
The first thing that happens in this file is that each button is given an event listener. These event listeners define a function to call when the event occurs. These functions all call other functions defined later in the document. 
You can read a summary of what each of these functions do in the Functional Features section. 

Provided below are some guidlines for a couple common changes you are likely to want to make to this file. 

#### Changing the amount of options available for each question
This requires you to also modify the question structure in `questions.js`. Each question needs to have the same amount of options. 

To make the required change in this file, navigate to the `showQuestions` function. Here is a template for an option: 
``` javascript
'<div class="option"><span>' +
    questions[index].options[n] +
    "</span></div>" +
```

Paste this in the `let option tag = ` variable. Ensure that you change "n" to the proper option number. Additionally, if the option you're pasting is the last option, change the final "+" to a ";". 

#### Changing the amount of time for each question
This is a very simple change. 
1. Line 33, `startTimer(15);`
2. Line 38 `let timeValue = 15;`
3. Line 50 `timeValue = 15;`

Change 15 to whatever you desire. This value is in seconds. 

If you wish to remove the time requirement altogether, simply remove these lines, as well as any `startTimerLine` calls. 

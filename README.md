<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Learning Adventure</title>
<style>
body{font-family:Arial;background:#7ec8ff;margin:0;text-align:center}
#game{max-width:900px;margin:auto;background:white;padding:25px;border-radius:14px;margin-top:30px;box-shadow:0 10px 25px rgba(0,0,0,.2)}
h1{margin-top:0}
button{padding:10px 18px;margin:8px;font-size:16px;border:none;border-radius:8px;background:#4CAF50;color:white;cursor:pointer}
button:hover{background:#388e3c}
.choice{display:block;margin:8px auto;width:70%;background:#eee;color:black}
.choice:hover{background:#ddd}
#passage{font-size:18px;margin:20px}
#map{display:flex;flex-wrap:wrap;justify-content:center;margin:15px 0}
.levelNode{width:40px;height:40px;margin:5px;border-radius:50%;display:flex;align-items:center;justify-content:center;background:#ddd;font-weight:bold}
.levelComplete{background:#4CAF50;color:white}
#helpBox{display:none;background:#fff3cd;border:2px solid #f0c36d;padding:15px;border-radius:10px;margin-top:10px}
</style>
</head>

<body>
<div id="game">
<h1>📚 Learning Adventure</h1>

<p id="intro">Press Start to begin your learning adventure!</p>

<h3 id="level">Level: 1</h3>
<h3 id="score">Score: 0</h3>

<div id="map"></div>

<p id="passage"></p>
<h3 id="question"></h3>
<div id="choices"></div>

<button id="startBtn" onclick="startGame()">Start Game</button>
<button onclick="showHelp()">Help</button>

<div id="helpBox">⏸ Game Paused.<br>Ask an adult, teacher, or older sibling for help. When you are ready press Resume.</div>
<button id="resumeBtn" style="display:none" onclick="resumeGame()">Resume</button>

<p id="feedback"></p>

</div>

<script>

let score = Number(localStorage.getItem("score")) || 0
let level = Number(localStorage.getItem("level")) || 1

let currentQuestionIndex = 0
let incorrectQueue = []
let currentQuestion = null

const questionBank = [

// Level 1 easier
{
level:1,
type:"math",
question:"You have 5 apples. You get 4 more. How many apples now?",
choices:["9","7","8","10"],
answer:"9"
},

{
level:1,
type:"math",
question:"There are 12 cookies. 3 are eaten. How many are left?",
choices:["9","8","10","7"],
answer:"9"
},

{
level:1,
type:"reading",
passage:"Liam was exhausted after running the race. He sat down and drank a big bottle of water.",
question:"What does the word 'exhausted' most likely mean?",
choices:["Very tired","Very excited","Very angry","Very hungry"],
answer:"Very tired"
},

{
level:1,
type:"reading",
passage:"Sophie dropped her ice cream on the ground. A little boy nearby started to cry because he lost his toy.",
question:"What would be a kind thing for Sophie to do?",
choices:["Help the boy look for the toy","Ignore him","Laugh at him","Walk away"],
answer:"Help the boy look for the toy"
},

// Level 2
{
level:2,
type:"math",
question:"Each box has 6 pencils. If you have 3 boxes how many pencils total?",
choices:["18","12","15","20"],
answer:"18"
},

{
level:2,
type:"math",
question:"You read 14 pages Monday and 11 Tuesday. How many total?",
choices:["25","24","26","23"],
answer:"25"
},

{
level:2,
type:"reading",
passage:"The storm clouds grew darker and the wind started blowing hard. Emma quickly ran inside the house.",
question:"Why did Emma go inside?",
choices:["A storm was coming","She wanted food","She was bored","She saw a friend"],
answer:"A storm was coming"
},

{
level:2,
type:"reading",
passage:"Ben studied his spelling words every night before the test on Friday.",
question:"What is Ben's goal?",
choices:["To do well on the test","To skip school","To avoid homework","To play games"],
answer:"To do well on the test"
}

]

function save(){
localStorage.setItem("score",score)
localStorage.setItem("level",level)
}

function renderMap(){

let html=""

for(let i=1;i<=20;i++){

let className="levelNode"

if(i<level) className+=" levelComplete"

html+=`<div class="${className}">${i}</div>`

}

document.getElementById("map").innerHTML=html

}

function startGame(){

document.getElementById("startBtn").style.display="none"
nextQuestion()

}

function getQuestionsForLevel(){

return questionBank.filter(q=>q.level===level)

}

function nextQuestion(){

let pool

if(incorrectQueue.length>0){

pool=incorrectQueue

}else{

pool=getQuestionsForLevel()

}

if(currentQuestionIndex>=pool.length){

if(incorrectQueue.length>0){

pool=incorrectQueue
currentQuestionIndex=0
incorrectQueue=[]

}else{

level++
currentQuestionIndex=0
save()
renderMap()

pool=getQuestionsForLevel()

}

}

currentQuestion=pool[currentQuestionIndex]

showQuestion(currentQuestion)

}

function showQuestion(q){

document.getElementById("feedback").innerText=""

if(q.passage){

document.getElementById("passage").innerText=q.passage

}else{

document.getElementById("passage").innerText=""

}


document.getElementById("question").innerText=q.question

let html=""

q.choices.forEach(c=>{

html+=`<button class='choice' onclick="answer('${c}')">${c}</button>`

})

document.getElementById("choices").innerHTML=html

}

function answer(choice){

if(choice===currentQuestion.answer){

score++

currentQuestionIndex++

}else{

incorrectQueue.push(currentQuestion)
currentQuestionIndex++

}

updateUI()

setTimeout(nextQuestion,600)

}

function updateUI(){

document.getElementById("score").innerText="Score: "+score

document.getElementById("level").innerText="Level: "+level

save()

}

function showHelp(){

document.getElementById("helpBox").style.display="block"
document.getElementById("resumeBtn").style.display="inline-block"

document.getElementById("choices").innerHTML=""

}

function resumeGame(){

document.getElementById("helpBox").style.display="none"
document.getElementById("resumeBtn").style.display="none"

showQuestion(currentQuestion)

}

renderMap()
updateUI()

</script>

</body>
</html>

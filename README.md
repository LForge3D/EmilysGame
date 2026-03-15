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

// reset progress if questions don't exist for stored level
let savedLevel = Number(localStorage.getItem("level")) || 1
if(savedLevel > 3) savedLevel = 1

let score = Number(localStorage.getItem("score")) || 0
let level = savedLevel

let currentQuestions = []
let incorrectQuestions = []
let currentIndex = 0
let currentQuestion = null

const questionBank = [

// LEVEL 1
{
level:1,
passage:"Liam ran the whole race without stopping. After finishing, he sat down and drank water because he felt exhausted.",
question:"Based on the story, what does the word 'exhausted' most likely mean?",
choices:["Very tired","Very happy","Very angry","Very loud"],
answer:"Very tired"
},

{
level:1,
question:"You have 8 marbles. Your friend gives you 5 more. How many marbles do you have now?",
choices:["13","11","12","14"],
answer:"13"
},

{
level:1,
passage:"Sofia noticed the classroom fish swimming slowly near the top of the tank and the water looked dirty.",
question:"What should Sofia probably tell the teacher?",
choices:["The fish tank needs cleaning","The fish wants candy","The fish needs music","Nothing is wrong"],
answer:"The fish tank needs cleaning"
},

{
level:1,
question:"There are 15 cookies. 6 get eaten. How many are left?",
choices:["9","8","10","7"],
answer:"9"
},

// LEVEL 2
{
level:2,
question:"Each box has 6 crayons. You buy 4 boxes. How many crayons do you have?",
choices:["24","20","18","22"],
answer:"24"
},

{
level:2,
passage:"The sky became very dark and thunder started rumbling. Jake quickly closed the windows.",
question:"Why did Jake close the windows?",
choices:["A storm was coming","He was bored","He wanted to sleep","He saw a bird"],
answer:"A storm was coming"
},

{
level:2,
question:"You read 16 pages Monday and 18 pages Tuesday. How many pages total?",
choices:["34","32","36","30"],
answer:"34"
},

{
level:2,
passage:"During lunch a new student sat alone and looked nervous.",
question:"What would be the kindest thing to do?",
choices:["Invite them to sit with you","Ignore them","Laugh at them","Leave the room"],
answer:"Invite them to sit with you"
},

// LEVEL 3
{
level:3,
question:"A pack has 9 trading cards. If you buy 5 packs how many cards do you get?",
choices:["45","40","42","48"],
answer:"45"
},

{
level:3,
passage:"The ground was soaked and puddles were everywhere after the heavy rain.",
question:"What does the word 'soaked' most likely mean?",
choices:["Very wet","Very dry","Very clean","Very hot"],
answer:"Very wet"
},

{
level:3,
question:"You save $7 each week for 6 weeks. How much money did you save?",
choices:["42","36","40","48"],
answer:"42"
}

]

function save(){
localStorage.setItem("score",score)
localStorage.setItem("level",level)
}

function renderMap(){
let html=""
for(let i=1;i<=20;i++){
let c="levelNode"
if(i<level) c+=" levelComplete"
html+=`<div class="${c}">${i}</div>`
}
document.getElementById("map").innerHTML=html
}

function startGame(){
document.getElementById("startBtn").style.display="none"
loadLevel()
showQuestion()
}

function loadLevel(){
currentQuestions = questionBank.filter(q=>q.level===level)
incorrectQuestions = []
currentIndex = 0
}

function getNextQuestion(){

if(currentIndex < currentQuestions.length){
return currentQuestions[currentIndex]
}

if(incorrectQuestions.length > 0){
currentQuestions = [...incorrectQuestions]
incorrectQuestions = []
currentIndex = 0
return currentQuestions[currentIndex]
}

level++
loadLevel()
return currentQuestions[currentIndex]

}

function showQuestion(){

currentQuestion = getNextQuestion()

if(!currentQuestion){
document.getElementById("question").innerText="Game complete!"
return
}

currentIndex++

document.getElementById("feedback").innerText=""

document.getElementById("passage").innerText=currentQuestion.passage || ""

document.getElementById("question").innerText=currentQuestion.question

let html=""

currentQuestion.choices.forEach(c=>{
html+=`<button class='choice' onclick="answer('${c}')">${c}</button>`
})


document.getElementById("choices").innerHTML=html

updateUI()

}

function answer(choice){

if(choice===currentQuestion.answer){
score++
document.getElementById("feedback").innerText="✅ Correct"
}else{
incorrectQuestions.push(currentQuestion)
document.getElementById("feedback").innerText="❌ Incorrect"
}

save()

setTimeout(showQuestion,800)

}

function updateUI(){

document.getElementById("score").innerText="Score: "+score

document.getElementById("level").innerText="Level: "+level

renderMap()

}

function showHelp(){

document.getElementById("helpBox").style.display="block"
document.getElementById("resumeBtn").style.display="inline-block"

document.getElementById("choices").innerHTML=""

}

function resumeGame(){

document.getElementById("helpBox").style.display="none"
document.getElementById("resumeBtn").style.display="none"

showQuestion()

}

renderMap()
updateUI()

</script>

</body>
</html>

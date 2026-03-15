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
.levelNode{width:50px;height:50px;margin:6px;border-radius:50%;display:flex;align-items:center;justify-content:center;background:#ddd;font-weight:bold}
.levelComplete{background:#4CAF50;color:white}
#badges{margin-top:10px}
.badge{display:inline-block;background:#ffd54f;padding:6px 10px;border-radius:8px;margin:4px;font-size:14px}
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

<button onclick="nextQuestion()">Start / Next Question</button>

<p id="feedback"></p>

<h3>🏆 Badges</h3>
<div id="badges"></div>

</div>

<script>

let score = Number(localStorage.getItem("score")) || 0
let level = Number(localStorage.getItem("level")) || 1

let correctAnswer = ""

function save(){
localStorage.setItem("score",score)
localStorage.setItem("level",level)
}

function updateUI(){

document.getElementById("score").innerText="Score: "+score

document.getElementById("level").innerText="Level: "+level

renderMap()
checkBadges()

}

function renderMap(){

let mapHTML=""

for(let i=1;i<=20;i++){

let className="levelNode"

if(i<level) className+=" levelComplete"

mapHTML+=`<div class="${className}">${i}</div>`

}

document.getElementById("map").innerHTML=mapHTML

}

function generateMath(level){

let a=Math.floor(Math.random()*10*level)+2
let b=Math.floor(Math.random()*10)+2

let answer=a*b

return{
passage:"",
question:`If you buy ${a} packs of stickers and each pack has ${b} stickers, how many stickers do you have?`,
choices:[answer,answer+5,answer-3,answer+7].sort(()=>Math.random()-.5),
answer:String(answer)
}

}

function readingBank(){

return[
{
passage:"Maya noticed her friend looked sad and wasn't talking during recess.",
question:"What would be the BEST thing Maya could do?",
choices:[
"Ignore her",
"Ask if she is okay",
"Laugh at her",
"Tell everyone"
],
answer:"Ask if she is okay"
},

{
passage:"The class pet hamster has an empty water bottle.",
question:"What should the students do?",
choices:[
"Give it water",
"Put it outside",
"Ignore it",
"Turn off lights"
],
answer:"Give it water"
},

{
passage:"Jordan studied every night for his spelling test.",
question:"Why did Jordan study each night?",
choices:[
"To do well on the test",
"Because he was bored",
"To skip school",
"To lose the test"
],
answer:"To do well on the test"
}

]

}

function nextQuestion(){

let type=Math.random()

let q

if(type<.5){

q=generateMath(level)

}else{

let bank=readingBank()

q=bank[Math.floor(Math.random()*bank.length)]

}

correctAnswer=q.answer

let choiceHTML=""

q.choices.forEach(c=>{
choiceHTML+=`<button class="choice" onclick="checkAnswer('${c}')">${c}</button>`
})


document.getElementById("passage").innerText=q.passage

document.getElementById("question").innerText=q.question

document.getElementById("choices").innerHTML=choiceHTML

document.getElementById("feedback").innerText=""

document.getElementById("intro").innerText=""

}

function checkAnswer(choice){

if(choice==correctAnswer){

score++

if(score%5===0) level++

document.getElementById("feedback").innerText="✅ Correct!"

}else{

document.getElementById("feedback").innerText="❌ Incorrect"

}

save()
updateUI()

}

function checkBadges(){

let badges=[]

if(score>=5) badges.push("Starter Brain 🧠")
if(score>=15) badges.push("Math Explorer ➗")
if(score>=30) badges.push("Reading Hero 📚")
if(score>=50) badges.push("Learning Master 🏆")

let html=""

badges.forEach(b=>{
html+=`<div class="badge">${b}</div>`
})

document.getElementById("badges").innerHTML=html

}

updateUI()

</script>

</body>
</html>

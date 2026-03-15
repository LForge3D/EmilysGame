<!DOCTYPE html>
<html>
<head>
<title>Reading & Math Challenge</title>

<style>
body{
font-family:Arial;
background:#8ecae6;
text-align:center;
padding:30px;
}

#game{
background:white;
padding:30px;
border-radius:15px;
max-width:700px;
margin:auto;
box-shadow:0 0 15px rgba(0,0,0,0.2);
}

button{
padding:10px 18px;
margin:8px;
border:none;
border-radius:8px;
background:#4CAF50;
color:white;
font-size:16px;
cursor:pointer;
}

button:hover{
background:#388e3c;
}

textarea{
width:80%;
height:80px;
font-size:16px;
padding:10px;
}

#passage{
font-size:18px;
margin:20px;
}

</style>
</head>

<body>

<div id="game">

<h1>📚 Reading & Math Challenge</h1>

<p>
Read the story or problem.  
Type your answer.  
A helper can approve your answer if it makes sense!
</p>

<h3 id="level">Level: 1</h3>
<h3 id="score">Score: 0</h3>

<p id="passage"></p>
<h3 id="question"></h3>

<textarea id="answer" placeholder="Type your answer here"></textarea>

<br>

<button onclick="approve()">✔ Approve Answer</button>
<button onclick="next()">Next Question</button>

<p id="feedback"></p>

</div>

<script>

let score = 0
let level = 1

const readingEasy = [

{
passage:"Sam forgot his lunch at home. At school he started to feel hungry.",
question:"What could Sam do to solve his problem?"
},

{
passage:"A strong wind knocked over Mia’s bike in the yard.",
question:"What should Mia do next?"
}

]

const readingMedium = [

{
passage:"Lucas saw a new student sitting alone at lunch.",
question:"What would be a kind thing for Lucas to do?"
},

{
passage:"The classroom plants looked dry and the soil was cracking.",
question:"What should the students do?"
}

]

const mathEasy = [

{
q:"You have 4 cookies. Your friend gives you 3 more. How many do you have now?"
},

{
q:"There are 5 birds on a fence. 2 fly away. How many are left?"
}

]

const mathMedium = [

{
q:"A box has 6 toy cars. You buy 3 boxes. How many cars do you have?"
},

{
q:"You read 8 pages on Monday and 9 pages on Tuesday. How many pages total?"
}

]

function next(){

document.getElementById("answer").value=""
document.getElementById("feedback").innerText=""

let type = Math.random()

if(level === 1){

if(type < .5){

let r = readingEasy[Math.floor(Math.random()*readingEasy.length)]

document.getElementById("passage").innerText=r.passage
document.getElementById("question").innerText=r.question

}else{

let m = mathEasy[Math.floor(Math.random()*mathEasy.length)]

document.getElementById("passage").innerText=""
document.getElementById("question").innerText=m.q

}

}

else{

if(type < .5){

let r = readingMedium[Math.floor(Math.random()*readingMedium.length)]

document.getElementById("passage").innerText=r.passage
document.getElementById("question").innerText=r.question

}else{

let m = mathMedium[Math.floor(Math.random()*mathMedium.length)]

document.getElementById("passage").innerText=""
document.getElementById("question").innerText=m.q

}

}

}

function approve(){

score++

if(score % 5 === 0){
level++
}

document.getElementById("score").innerText="Score: " + score
document.getElementById("level").innerText="Level: " + level
document.getElementById("feedback").innerText="✅ Answer Approved!"

}

next()

</script>

</body>
</html>

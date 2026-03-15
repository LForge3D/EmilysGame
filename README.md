<!DOCTYPE html>
<html>
<head>
<title>Super Learning Game</title>
<style>
body{
font-family: Arial;
background: linear-gradient(135deg,#6ec6ff,#b3e5fc);
text-align:center;
padding:40px;
}

#game{
background:white;
padding:30px;
border-radius:15px;
max-width:500px;
margin:auto;
box-shadow:0 5px 20px rgba(0,0,0,0.2);
}

button{
padding:10px 20px;
margin:10px;
border:none;
border-radius:8px;
background:#4CAF50;
color:white;
font-size:16px;
cursor:pointer;
}

button:hover{
background:#45a049;
}

input{
padding:8px;
font-size:16px;
}

#score{
font-size:20px;
margin-top:15px;
}
</style>
</head>

<body>

<div id="game">
<h1>🎮 Super Learning Game</h1>

<h3 id="question">Press Start!</h3>

<input id="answer" placeholder="Type your answer">
<br>

<button onclick="checkAnswer()">Submit</button>
<button onclick="nextQuestion()">Next Question</button>

<p id="feedback"></p>
<p id="score">Score: 0</p>

</div>

<script>

let score = 0;
let currentAnswer = "";

const readingQuestions = [
{q:"What color is the sky on a sunny day?",a:"blue"},
{q:"What animal says 'meow'?",a:"cat"},
{q:"What do bees make?",a:"honey"},
{q:"What do you read with?",a:"eyes"}
];

function randomMath(){

let type = Math.floor(Math.random()*3);

let a = Math.floor(Math.random()*10)+1;
let b = Math.floor(Math.random()*10)+1;

if(type===0){
currentAnswer = a+b;
return a+" + "+b+" = ?";
}

if(type===1){
currentAnswer = a-b;
return a+" - "+b+" = ?";
}

currentAnswer = a*b;
return a+" × "+b+" = ?";
}

function randomReading(){
let r = readingQuestions[Math.floor(Math.random()*readingQuestions.length)];
currentAnswer = r.a;
return r.q;
}

function nextQuestion(){

document.getElementById("feedback").innerText="";
document.getElementById("answer").value="";

if(Math.random()>0.5){
document.getElementById("question").innerText=randomMath();
}else{
document.getElementById("question").innerText=randomReading();
}

}

function checkAnswer(){

let user = document.getElementById("answer").value.toLowerCase();

if(user == currentAnswer){
score++;
document.getElementById("feedback").innerText="✅ Correct!";
}else{
document.getElementById("feedback").innerText="❌ Try again!";
}

document.getElementById("score").innerText="Score: "+score;

}

</script>

</body>
</html>

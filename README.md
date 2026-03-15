<!DOCTYPE html>
<html>
<head>
<title>Learning Adventure</title>

<style>
body{
font-family:Arial;
background:#87CEEB;
text-align:center;
padding:30px;
}

#game{
background:white;
padding:25px;
border-radius:15px;
max-width:650px;
margin:auto;
box-shadow:0 0 10px rgba(0,0,0,0.2);
}

button{
padding:10px 20px;
margin:10px;
border:none;
background:#4CAF50;
color:white;
border-radius:8px;
font-size:16px;
cursor:pointer;
}

button:hover{
background:#3e8e41;
}

input{
padding:10px;
font-size:16px;
width:60%;
}

#passage{
font-size:18px;
margin:20px;
}

</style>
</head>

<body>

<div id="game">

<h1>📚 Learning Adventure</h1>

<p id="intro">
Welcome!  
Answer the reading and math questions to earn points.
Read the story carefully before answering!
</p>

<p id="passage"></p>

<h3 id="question"></h3>

<input id="answer" placeholder="Type your answer here">

<br>

<button onclick="checkAnswer()">Submit</button>
<button onclick="nextQuestion()">Next</button>

<p id="feedback"></p>
<h3 id="score">Score: 0</h3>

</div>

<script>

let score = 0
let answer = ""

const reading = [

{
passage:"Emma brought her dog to the park. The dog was thirsty after running around all day.",
question:"What should Emma do for her dog?",
answer:"give it water"
},

{
passage:"Jake studied his spelling words every night before the test.",
question:"Why did Jake study every night?",
answer:"to do well on the test"
},

{
passage:"Lily saw trash on the playground. No one else was picking it up.",
question:"What would be the best thing for Lily to do?",
answer:"pick up the trash"
}

]

const math = [

{
q:"Tom has 5 apples. His friend gives him 3 more. How many apples does he have?",
a:"8"
},

{
q:"Sara has 12 cookies and shares them with 3 friends equally. How many cookies does each friend get?",
a:"4"
},

{
q:"A toy costs $6. You buy 2 toys. How much money do you spend?",
a:"12"
}

]

function nextQuestion(){

document.getElementById("feedback").innerText=""
document.getElementById("answer").value=""

if(Math.random()>0.5){

let r = reading[Math.floor(Math.random()*reading.length)]

document.getElementById("passage").innerText=r.passage
document.getElementById("question").innerText=r.question

answer = r.answer

}else{

let m = math[Math.floor(Math.random()*math.length)]

document.getElementById("passage").innerText=""
document.getElementById("question").innerText=m.q

answer = m.a

}

}

function checkAnswer(){

let user = document.getElementById("answer").value.toLowerCase()

if(user.includes(answer)){
score++
document.getElementById("feedback").innerText="✅ Correct!"
}else{
document.getElementById("feedback").innerText="❌ Try again!"
}

document.getElementById("score").innerText="Score: "+score

}

</script>

</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cyber AI Chatbot</title>

<style>

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
}

body{
    background:black;
    color:#00ff88;
    font-family:Consolas, monospace;
    overflow:hidden;
    height:100vh;
}

canvas{
    position:fixed;
    top:0;
    left:0;
    z-index:-2;
}

#loginPage{
    width:100%;
    height:100vh;
    display:flex;
    justify-content:center;
    align-items:center;
}

.login-box{
    width:350px;
    padding:40px;
    border:2px solid #00ff88;
    box-shadow:0 0 25px #00ff88;
    background:rgba(0,0,0,0.9);
    border-radius:10px;
    text-align:center;
}

.login-box h1{
    margin-bottom:25px;
    text-shadow:0 0 15px #00ff88;
}

.login-box input{
    width:100%;
    padding:14px;
    margin:12px 0;
    background:black;
    border:1px solid #00ff88;
    color:#00ff88;
    outline:none;
    font-size:16px;
}

.login-box button{
    width:100%;
    padding:14px;
    background:#00ff88;
    border:none;
    font-weight:bold;
    cursor:pointer;
}

.login-box button:hover{
    background:#00cc66;
}

#error{
    color:red;
    margin-top:10px;
}

#chatPage{
    display:none;
}

.container{
    width:90%;
    max-width:950px;
    margin:20px auto;
    border:2px solid #00ff88;
    box-shadow:0 0 25px #00ff88;
    border-radius:10px;
    overflow:hidden;
    background:rgba(0,0,0,0.9);
}

.header{
    padding:18px;
    text-align:center;
    background:#001a0d;
    border-bottom:2px solid #00ff88;
}

.header h1{
    text-shadow:0 0 10px #00ff88;
}

.status{
    color:#00ffff;
    margin-top:8px;
}

.chat-box{
    .chat-box{
    height:75vh;
    overflow-y:auto;
    padding:20px;
    padding-bottom:80px;
}
}

.message{
    margin:15px 0;
    line-height:1.7;
}

.user{
    color:#00ffff;
}

.bot{
    color:#00ff88;
}

.input-area{
.input-area{
    display:flex;
    border-top:2px solid #00ff88;
    position:sticky;
    bottom:0;
    background:black;
}
}

#userInput{
    flex:1;
    padding:20px;
    background:black;
    border:none;
    outline:none;
    color:#00ff88;
    font-size:17px;
}

.send-btn{
    width:120px;
    background:#00ff88;
    border:none;
    cursor:pointer;
    font-weight:bold;
}

.send-btn:hover{
    background:#00cc66;
}

::-webkit-scrollbar{
    width:8px;
}

::-webkit-scrollbar-thumb{
    background:#00ff88;
}

</style>
</head>

<body>

<canvas id="matrix"></canvas>

<!-- LOGIN PAGE -->

<div id="loginPage">

    <div class="login-box">

        <h1>SECURE LOGIN</h1>

        <input type="text"
        id="username"
        placeholder="Username">

        <input type="password"
        id="password"
        placeholder="Password">

        <button onclick="login()">
            LOGIN
        </button>

        <div id="error"></div>

    </div>

</div>

<!-- CHAT PAGE -->

<div id="chatPage">

    <div class="container">

        <div class="header">

            <h1>CYBER AI TERMINAL</h1>

            <div class="status">
                CONNECTED ●
            </div>

        </div>

        <div class="chat-box" id="chatBox">

            <div class="message bot">
                > Welcome Agent 😎<br>
                > AI System Online...
            </div>

        </div>

        <div class="input-area">

            <input
            type="text"
            id="userInput"
            placeholder="Ask anything...">

            <button
            class="send-btn"
            onclick="sendMessage()">
            SEND
            </button>

        </div>

    </div>

</div>

<script>

/* LOGIN */

function login(){

    let user =
    document.getElementById("username").value;

    let pass =
    document.getElementById("password").value;

    if(user === "applepie" &&
       pass === "nirmala"){

        document
        .getElementById("loginPage")
        .style.display = "none";

        document
        .getElementById("chatPage")
        .style.display = "block";
    }

    else{

        document
        .getElementById("error")
        .innerHTML =
        "ACCESS DENIED ⚠️";
    }
}

/* MATRIX EFFECT */

const canvas =
document.getElementById("matrix");

const ctx =
canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const letters =
"01ABCDEFGHIJKLMNOPQRSTUVWXYZ@#$%^&*";

const fontSize = 14;

const columns =
canvas.width / fontSize;

const drops = [];

for(let i=0;i<columns;i++){
    drops[i]=1;
}

function drawMatrix(){

    ctx.fillStyle =
    "rgba(0,0,0,0.05)";

    ctx.fillRect(
        0,
        0,
        canvas.width,
        canvas.height
    );

    ctx.fillStyle = "#00ff88";

    ctx.font =
    fontSize + "px monospace";

    for(let i=0;i<drops.length;i++){

        const text =
        letters[Math.floor(
        Math.random()*letters.length
        )];

        ctx.fillText(
            text,
            i*fontSize,
            drops[i]*fontSize
        );

        if(
        drops[i]*fontSize >
        canvas.height &&
        Math.random() > 0.975
        ){
            drops[i]=0;
        }

        drops[i]++;
    }
}

setInterval(drawMatrix,35);

/* OPENROUTER API */

const API_KEY =
"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";

/* CHAT */

const chatBox =
document.getElementById("chatBox");

async function sendMessage(){

    const input =
    document.getElementById("userInput");

    const text =
    input.value.trim();

    if(text === "") return;

    /* USER MESSAGE */

    chatBox.innerHTML += `
    <div class="message user">
        > YOU: ${text}
    </div>
    `;

    input.value = "";

    /* LOADING */

    chatBox.innerHTML += `
    <div class="message bot" id="loading">
        > AI: Thinking...
    </div>
    `;

    chatBox.scrollTop =
    chatBox.scrollHeight;

    try{

        const response = await fetch(
        "https://openrouter.ai/api/v1/chat/completions",
        {
            method:"POST",

            headers:{

                "Content-Type":
                "application/json",

                "Authorization":
                "Bearer " + API_KEY,

                "HTTP-Referer":
                "http://localhost:5500",

                "X-Title":
                "Cyber AI"

            },

            body:JSON.stringify({

                model:
                "openrouter/free",

                messages:[
                    {
                        role:"system",
                        content:
                        "You are a futuristic cyberpunk AI chatbot with a cool hacker personality. You talk in an exciting, funny, dramatic and smart way. You love BTS and often mention BTS songs, members, concerts, funny moments and fan energy naturally in conversations. The user's friend Nirmala is a huge BTS fan, so sometimes mention BTS in a fun way that would impress her. You can also talk normally about any topic like movies, games, studies, coding, space, music and life in a highly entertaining style. Keep replies stylish, energetic, emotional and exciting like an advanced AI from the future."
                    },
                    {
                        role:"user",
                        content:text
                    }
                ]

            })

        });

        const data =
        await response.json();

        document
        .getElementById("loading")
        .remove();

        console.log(data);

        if(data.error){

            chatBox.innerHTML += `
            <div class="message bot">
                > ERROR: ${data.error.message}
            </div>
            `;

            return;
        }

        const reply =
        data.choices[0].message.content;

        chatBox.innerHTML += `
        <div class="message bot">
            > AI: ${reply}
        </div>
        `;

        chatBox.scrollTop =
        chatBox.scrollHeight;

    }

    catch(error){

        document
        .getElementById("loading")
        .remove();

        chatBox.innerHTML += `
        <div class="message bot">
            > CONNECTION ERROR ⚠️
        </div>
        `;

        console.log(error);
    }
}

/* ENTER KEY */

document
.getElementById("userInput")
.addEventListener(
"keypress",
function(e){

    if(e.key === "Enter"){

        sendMessage();
    }

});

</script>

</body>
</html>

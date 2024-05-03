# simon-says-game
Code structure of simon says game.
[NOTE----The code is written in External CSS & External JS file.]


## HTML code:-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simon Says Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Simon Says Game</h1>
    <h2>Press any key to the game.</h2>

    <div class="btn-container">
        <div class="line-one">
            <div class="btn red" type="button" id="red">1</div>
            <div class="btn yellow" type="button" id="yellow">2</div>
        </div>
        <div class="line-two">
            <div class="btn green" type="button" id="green">3</div>
            <div class="btn purple" type="button" id="purple">4</div>
        </div>
    </div>

    <script src="app.js"></script>
</body>
</html>


## CSS code:-
body{
    text-align: center;
    background-color: black;
    color: white;
}
.btn{
    height: 200px;
    width: 200px;
    border-radius: 20%;
    border: 10px solid white;
    margin: 2.5rem;
}
.btn-container{
    display: flex;
    justify-content: center;
}
.red{
    background-color: #d95980;
}
.yellow{
    background-color: #f99b45;
}
.green{
    background-color: #63aac0;
}
.purple{
    background-color: #819ff9;
}
.flash{
    background-color: grey;
}
.userFlash{
    background-color: gold;
}


## JS code:-
let gameSeq = [];
let userSeq = [];

let btns = ["yellow", "red", "purple", "green"];

let started = false;
let level = 0;
let h2 = document.querySelector("h2");

document.addEventListener("keypress", function(){
    if(started == false){
        console.log(`Game is started.`);
        started = true;

        levelUp();
    }
});

function gameFlash(btn){
    btn.classList.add("flash");
    setTimeout(function(){
        btn.classList.remove("flash");
    }, 350);
};

function userFlash(btn){
    btn.classList.add("userFlash");
    setTimeout(function(){
        btn.classList.remove("userFlash");
    }, 350);
};

function levelUp(){
    userSeq = [];
    level++;
    h2.innerText = `Level ${level}`;

    let rdmIdx = Math.floor(Math.random() * 3);
    let rdmColor = btns[rdmIdx];
    let rdmBtn = document.querySelector(`.${rdmColor}`);
    // console.log(rdmIdx);
    // console.log(rdmColor);
    // console.log(rdmBtn);
    gameSeq.push(rdmColor);
    console.log(gameSeq);
    gameFlash(rdmBtn);
};

function checkAns(idx){
    if(userSeq[idx] === gameSeq[idx]){
        // console.log(`Same Value`);
        if(userSeq.length == gameSeq.length){
            setTimeout(levelUp, 1000);
            // levelUp();
        }
    }else{
        h2.innerHTML=`Game Over! Your score was <b>${level}</b>. <br> Press any key to restart.`;
        document.querySelector("body").style.backgroundColor="red";
        setTimeout(function(){
        document.querySelector("body").style.backgroundColor="black";
        }, 150)
        reset();
    }
}

function btnPress(){
    let btn = this;
    userFlash(btn);

    userColor = btn.getAttribute("id");
    userSeq.push(userColor);

    checkAns(userSeq.length-1);
};

let allBtns = document.querySelectorAll(".btn");
for(btn of allBtns){
    btn.addEventListener("click", btnPress);
};

function reset(){
    started = false;
    gameSeq = [];
    userSeq = [];
    level = 0;
}

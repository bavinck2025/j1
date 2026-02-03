<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Mini Mario</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
margin:0;
background:#5cc9ff;
overflow:hidden;
}

canvas {
display:block;
margin:auto;
background:#87ceeb;
}

.controls {
position:fixed;
bottom:20px;
width:100%;
display:flex;
justify-content:space-around;
}

.btn {
width:70px;
height:70px;
background:rgba(0,0,0,0.5);
border-radius:50%;
color:white;
font-size:30px;
display:flex;
align-items:center;
justify-content:center;
user-select:none;
}
</style>
</head>

<body>

<canvas id="game" width="400" height="500"></canvas>

<div class="controls">
<div class="btn" id="left">◀</div>
<div class="btn" id="jump">⬆</div>
<div class="btn" id="right">▶</div>
</div>

<script>
const c = document.getElementById("game");
const ctx = c.getContext("2d");

let player = {x:50,y:350,w:30,h:30,vy:0,onGround:false};
let gravity = 0.6;
let platforms = [
{x:0,y:420,w:400,h:20},
{x:100,y:330,w:100,h:15},
{x:260,y:260,w:100,h:15}
];

let left=false,right=false;

document.getElementById("left").ontouchstart=()=>left=true;
document.getElementById("left").ontouchend=()=>left=false;

document.getElementById("right").ontouchstart=()=>right=true;
document.getElementById("right").ontouchend=()=>right=false;

document.getElementById("jump").ontouchstart=()=>{
if(player.onGround){
player.vy=-12;
player.onGround=false;
}
};

function update(){
if(left) player.x-=3;
if(right) player.x+=3;

player.vy+=gravity;
player.y+=player.vy;

player.onGround=false;

platforms.forEach(p=>{
if(player.x<p.x+p.w &&
player.x+player.w>p.x &&
player.y<p.y+p.h &&
player.y+player.h>p.y){
player.y=p.y-player.h;
player.vy=0;
player.onGround=true;
}
});

if(player.y>500){
player.x=50;
player.y=350;
}
}

function draw(){
ctx.clearRect(0,0,400,500);

ctx.fillStyle="red";
ctx.fillRect(player.x,player.y,player.w,player.h);

ctx.fillStyle="green";
platforms.forEach(p=>{
ctx.fillRect(p.x,p.y,p.w,p.h);
});
}

function loop(){
update();
draw();
requestAnimationFrame(loop);
}

loop();
</script>

</body>
</html>

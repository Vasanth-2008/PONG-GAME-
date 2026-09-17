<!DOCTYPE html>
<html>
<head>
<style>
  body{margin:0;background:#000;display:flex;justify-content:center;align-items:center;height:100vh;}
  canvas{background:#111;border:2px solid #0f0;}
</style>
</head>
<body>
<canvas id="game" width="600" height="400"></canvas>
<script>
const c = document.getElementById("game");
const ctx = c.getContext("2d");

let player = {x:10, y:150, w:10, h:100, dy:0};
let cpu = {x:580, y:150, w:10, h:100};
let ball = {x:300, y:200, r:8, dx:4, dy:4};
let pScore=0, cScore=0;

document.addEventListener("mousemove", e=>{
  let rect = c.getBoundingClientRect();
  player.y = e.clientY - rect.top - player.h/2;
});

function update(){
  ball.x += ball.dx;
  ball.y += ball.dy;

  if(ball.y < 0 || ball.y > c.height) ball.dy *= -1;

  // player paddle hit
  if(ball.x < player.x+player.w && ball.y > player.y && ball.y < player.y+player.h){
    ball.dx *= -1;
  }
  // cpu paddle hit
  if(ball.x > cpu.x-8 && ball.y > cpu.y && ball.y < cpu.y+cpu.h){
    ball.dx *= -1;
  }
  // cpu AI follows ball
  if(cpu.y+cpu.h/2 < ball.y) cpu.y += 3;
  else cpu.y -= 3;

  // score
  if(ball.x < 0){ cScore++; resetBall(); }
  if(ball.x > c.width){ pScore++; resetBall(); }
}

function resetBall(){
  ball.x = 300; ball.y = 200;
  ball.dx *= -1;
}

function draw(){
  ctx.clearRect(0,0,c.width,c.height);
  ctx.fillStyle="#0f0";
  ctx.fillRect(player.x, player.y, player.w, player.h);
  ctx.fillRect(cpu.x, cpu.y, cpu.w, cpu.h);
  ctx.beginPath();
  ctx.arc(ball.x, ball.y, ball.r, 0, Math.PI*2);
  ctx.fill();
  ctx.font="30px Arial";
  ctx.fillText(pScore, 250, 50);
  ctx.fillText(cScore, 330, 50);
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

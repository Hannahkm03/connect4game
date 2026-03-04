<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Connect Four</title>

<style>

body{
text-align:center;
font-family:Arial;
background:#f4f4f4;
}

h1{
margin-bottom:20px;
}

#board{
display:grid;
grid-template-columns:repeat(7,80px);
grid-template-rows:repeat(6,80px);
gap:8px;
background:green;
padding:15px;
border-radius:12px;
width:max-content;
margin:auto;
}

.cell{
width:80px;
height:80px;
background:white;
border-radius:50%;
position:relative;
cursor:pointer;
}

.piece{
width:80px;
height:80px;
border-radius:50%;
position:absolute;
top:-500px;
left:0;
animation:drop .4s forwards;
}

.pink{
background:hotpink;
}

.blue{
background:dodgerblue;
}

@keyframes drop{
from{transform:translateY(-500px);}
to{transform:translateY(0);}
}

#status{
font-size:22px;
margin-top:20px;
}

#restart{
margin-top:15px;
padding:10px 20px;
font-size:16px;
border:none;
border-radius:6px;
background:green;
color:white;
cursor:pointer;
}

#restart:hover{
background:darkgreen;
}

</style>
</head>

<body>

<h1>Connect Four</h1>

<div id="board"></div>

<p id="status">Pink's Turn</p>

<button id="restart">Restart Game</button>

<script>

const rows = 6
const cols = 7

let board = []
let currentPlayer = "pink"

const boardDiv = document.getElementById("board")
const statusText = document.getElementById("status")
const restartBtn = document.getElementById("restart")

function initBoard(){

boardDiv.innerHTML=""
board=[]

for(let r=0;r<rows;r++){

board[r]=[]

for(let c=0;c<cols;c++){

board[r][c]=null

const cell=document.createElement("div")
cell.classList.add("cell")

cell.dataset.row=r
cell.dataset.col=c

cell.addEventListener("click",handleMove)

boardDiv.appendChild(cell)

}
}

currentPlayer="pink"
statusText.textContent="Pink's Turn"
boardDiv.style.pointerEvents="auto"

}

function handleMove(event){

const col=event.target.dataset.col

for(let r=rows-1;r>=0;r--){

if(!board[r][col]){

board[r][col]=currentPlayer

const cell=document.querySelector(`[data-row='${r}'][data-col='${col}']`)

const piece=document.createElement("div")
piece.classList.add("piece",currentPlayer)

cell.appendChild(piece)

if(checkWin(r,col)){

statusText.textContent=currentPlayer+" wins!"
boardDiv.style.pointerEvents="none"
return

}

currentPlayer=currentPlayer==="pink"?"blue":"pink"
statusText.textContent=currentPlayer+"'s Turn"

return

}

}

}

function checkWin(r,c){

function count(dr,dc){

let total=0
let row=r+dr
let col=c+dc

while(
row>=0 &&
row<rows &&
col>=0 &&
col<cols &&
board[row][col]===currentPlayer
){

total++
row+=dr
col+=dc

}

return total

}

const directions=[
[[0,1],[0,-1]],
[[1,0],[-1,0]],
[[1,1],[-1,-1]],
[[1,-1],[-1,1]]
]

for(let dir of directions){

let total=
1+
count(dir[0][0],dir[0][1])+
count(dir[1][0],dir[1][1])

if(total>=4) return true

}

return false

}

restartBtn.addEventListener("click",initBoard)

initBoard()

</script>

</body>
</html>

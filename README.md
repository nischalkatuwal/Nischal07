<html><head><base href="https://websim.tictactoe.com/"><title>Tic Tac Toe</title>
<style>
body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #f0f0f0;
}

.game-container {
    text-align: center;
}

.board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-gap: 5px;
    margin-bottom: 20px;
}

.cell {
    width: 100px;
    height: 100px;
    background-color: #fff;
    border: 2px solid #333;
    font-size: 48px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.cell:hover {
    background-color: #f0f0f0;
}

#status {
    font-size: 24px;
    margin-bottom: 20px;
}

#reset {
    font-size: 18px;
    padding: 10px 20px;
    background-color: #4CAF50;
    color: white;
    border: none;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

#reset:hover {
    background-color: #45a049;
}
</style>
</head>
<body>
<div class="game-container">
    <h1>Tic Tac Toe</h1>
    <div id="status">Player X's turn</div>
    <div class="board" id="board">
        <div class="cell" data-cell-index="0"></div>
        <div class="cell" data-cell-index="1"></div>
        <div class="cell" data-cell-index="2"></div>
        <div class="cell" data-cell-index="3"></div>
        <div class="cell" data-cell-index="4"></div>
        <div class="cell" data-cell-index="5"></div>
        <div class="cell" data-cell-index="6"></div>
        <div class="cell" data-cell-index="7"></div>
        <div class="cell" data-cell-index="8"></div>
    </div>
    <button id="reset">Reset Game</button>
</div>

<script>
const statusDisplay = document.querySelector('#status');
const cells = document.querySelectorAll('.cell');
const resetButton = document.querySelector('#reset');

let gameActive = true;
let currentPlayer = "X";
let gameState = ["", "", "", "", "", "", "", "", ""];

const winningConditions = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
];

function handleCellPlayed(clickedCell, clickedCellIndex) {
    gameState[clickedCellIndex] = currentPlayer;
    clickedCell.innerHTML = currentPlayer;
}

function handlePlayerChange() {
    currentPlayer = currentPlayer === "X" ? "O" : "X";
    statusDisplay.innerHTML = `Player ${currentPlayer}'s turn`;
}

function handleResultValidation() {
    let roundWon = false;
    for (let i = 0; i <= 7; i++) {
        const winCondition = winningConditions[i];
        let a = gameState[winCondition[0]];
        let b = gameState[winCondition[1]];
        let c = gameState[winCondition[2]];
        if (a === '' || b === '' || c === '') {
            continue;
        }
        if (a === b && b === c) {
            roundWon = true;
            break
        }
    }

    if (roundWon) {
        statusDisplay.innerHTML = `Player ${currentPlayer} has won!`;
        gameActive = false;
        return;
    }

    let roundDraw = !gameState.includes("");
    if (roundDraw) {
        statusDisplay.innerHTML = `Game ended in a draw!`;
        gameActive = false;
        return;
    }

    handlePlayerChange();
}

function handleCellClick(clickedCellEvent) {
    const clickedCell = clickedCellEvent.target;
    const clickedCellIndex = parseInt(clickedCell.getAttribute('data-cell-index'));

    if (gameState[clickedCellIndex] !== "" || !gameActive) {
        return;
    }

    handleCellPlayed(clickedCell, clickedCellIndex);
    handleResultValidation();
}

function handleRestartGame() {
    gameActive = true;
    currentPlayer = "X";
    gameState = ["", "", "", "", "", "", "", "", ""];
    statusDisplay.innerHTML = `Player ${currentPlayer}'s turn`;
    cells.forEach(cell => cell.innerHTML = "");
}

cells.forEach(cell => cell.addEventListener('click', handleCellClick));
resetButton.addEventListener('click', handleRestartGame);
</script>
</body></html>

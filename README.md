<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic-Tac-Toe</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>

    <h1>Tic-Tac-Toe</h1>
    <div class="game-container">
        <div class="board" id="board">
            <div class="cell" data-index="0"></div>
            <div class="cell" data-index="1"></div>
            <div class="cell" data-index="2"></div>
            <div class="cell" data-index="3"></div>
            <div class="cell" data-index="4"></div>
            <div class="cell" data-index="5"></div>
            <div class="cell" data-index="6"></div>
            <div class="cell" data-index="7"></div>
            <div class="cell" data-index="8"></div>
        </div>
        <p id="status">Player X's Turn</p>
        <button id="reset">Restart Game</button>
    </div>

    <script src="script.js"></script>
</body>
</html>

/* General styles */
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background: #f4f4f4;
    margin: 0;
    padding: 20px;
}

/* Game board container */
.game-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

/* Tic-Tac-Toe board */
.board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    gap: 5px;
    margin-top: 20px;
}

/* Individual cell styling */
.cell {
    width: 100px;
    height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    background: white;
    border-radius: 10px;
    cursor: pointer;
    transition: background 0.3s;
}

.cell:hover {
    background: #ddd;
}

/* Status message */
#status {
    font-size: 1.2rem;
    margin-top: 20px;
}

/* Reset button */
#reset {
    margin-top: 10px;
    padding: 10px 20px;
    font-size: 1rem;
    background: red;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: 0.3s;
}

#reset:hover {
    background: darkred;
}

// Game variables
const board = document.getElementById("board");
const cells = document.querySelectorAll(".cell");
const statusText = document.getElementById("status");
const resetButton = document.getElementById("reset");

let currentPlayer = "X";
let gameBoard = ["", "", "", "", "", "", "", "", ""];
let isGameActive = true;

// Winning combinations
const winningConditions = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
    [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
    [0, 4, 8], [2, 4, 6]             // Diagonals
];

// Function to handle cell clicks
function handleCellClick(event) {
    const index = event.target.dataset.index;

    // If the cell is already taken or the game is over, do nothing
    if (gameBoard[index] !== "" || !isGameActive) return;

    // Mark the cell with the current player's symbol
    gameBoard[index] = currentPlayer;
    event.target.textContent = currentPlayer;

    // Check for a win or draw
    checkGameResult();

    // Switch player if game is still active
    if (isGameActive) {
        currentPlayer = currentPlayer === "X" ? "O" : "X";
        statusText.textContent = `Player ${currentPlayer}'s Turn`;
    }
}

// Function to check for a win or draw
function checkGameResult() {
    let roundWon = false;

    // Check all winning conditions
    for (let condition of winningConditions) {
        const [a, b, c] = condition;
        if (gameBoard[a] && gameBoard[a] === gameBoard[b] && gameBoard[a] === gameBoard[c]) {
            roundWon = true;
            break;
        }
    }

    // If a player has won
    if (roundWon) {
        statusText.textContent = `Player ${currentPlayer} Wins!`;
        isGameActive = false;
        return;
    }

    // If all cells are filled and no one won, it's a draw
    if (!gameBoard.includes("")) {
        statusText.textContent = "It's a Draw!";
        isGameActive = false;
        return;
    }
}

// Function to restart the game
function resetGame() {
    gameBoard = ["", "", "", "", "", "", "", "", ""];
    isGameActive = true;
    currentPlayer = "X";
    statusText.textContent = "Player X's Turn";
    cells.forEach(cell => cell.textContent = "");
}

// Add event listeners
cells.forEach(cell => cell.addEventListener("click", handleCellClick));
resetButton.addEventListener("click", resetGame);

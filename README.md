### 1. **Game Elements:**
   - **HTML elements** are used to represent the game board (boxes), buttons (reset, new game), and a message container (to show who won or if it's a draw).
   - **Boxes** represent the cells of the Tic Tac Toe grid. Each box can be clicked to place an "X" or "O".
   - **Buttons** are for resetting the game or starting a new one.
   - **Message Container** shows who won or if the game ended in a draw.

### 2. **Game Flow and Logic:**
   - **turnO** variable tracks whose turn it is:
     - **true** means it’s Player O's turn.
     - **false** means it’s Player X's turn.
   - **winPatterns** holds the possible ways to win in Tic Tac Toe (rows, columns, diagonals).
   
### 3. **Game Functions:**
   - **resetGame()**:
     - Resets the game to its initial state.
     - Makes all boxes available to play again and hides the message (if any).
   
   - **boxes.forEach()**:
     - Adds a click event listener to each box.
     - When a box is clicked, the current player's symbol ("X" or "O") is placed inside the box, and that box becomes disabled (you can't click it again).
     - After each move, the game checks if there is a winner.
   
   - **disableBoxes()**:
     - Disables all boxes (so no one can click on them after the game ends).

   - **enableBoxes()**:
     - Enables all boxes again for a new game.

   - **showWinner(winner)**:
     - Displays a message when a winner is found and disables all the boxes.

   - **checkWinner()**:
     - This checks if any of the winning patterns are met (a row, column, or diagonal of "X"s or "O"s).
     - If there is a winner, it calls **showWinner()**.
   
### 4. **Reset/New Game Buttons:**
   - Both the **reset button** and **new game button** are set to trigger the **resetGame()** function.
   
### 5. **Winning Logic:**
   - The game checks each winning combination (like first row, second row, etc.). If any of the combinations have the same symbol (all "X"s or all "O"s), a winner is found.
   
### 6. **Message Display:**
   - The message container displays who won after the game ends (either Player "X" or Player "O").
   - After the winner is announced, all boxes are disabled so no further moves can be made.

---

### To Summarize:
- The game switches turns between **Player O** and **Player X**.
- The game checks for a winner after every move.
- If there's a winner or a draw, it shows a message and disables the boxes.
- The game can be reset with a button, and a new game can be started.

### 1. **Selecting the Elements (HTML)**

```javascript
let boxes = document.querySelectorAll(".box");
let resetBtn = document.querySelector("#reset-btn");
let newGameBtn = document.querySelector("#new-btn");
let msgContainer = document.querySelector(".msg-container");
let msg = document.querySelector("#msg");
```

- **boxes**: This selects all the game boxes (the cells where players will place their X's and O's).
- **resetBtn** and **newGameBtn**: These are buttons that allow the user to reset the game or start a new one.
- **msgContainer**: This is the container where the message will be shown, like who won the game.
- **msg**: This is where the winner’s message will be displayed (like "Player X wins!" or "It’s a draw!").

---

### 2. **Game Variables**

```javascript
let turnO = true; // Player X starts with false, Player O is true
```

- **turnO**: This variable keeps track of whose turn it is. If `turnO` is `true`, it’s Player O's turn. If `false`, it’s Player X’s turn.

---

### 3. **Winning Patterns**

```javascript
const winPatterns = [
    [0,1,2],
    [0,3,6],
    [0,4,8],
    [1,4,7],
    [2,5,8],
    [2,4,6],
    [3,4,5],
    [6,7,8]
];
```

- **winPatterns**: This is a list of all the possible winning combinations of box positions on the board. For example, [0,1,2] means the first row (boxes 1, 2, and 3) on the board, and [0,3,6] is the first column.

---

### 4. **Reset the Game**

```javascript
const resetGame = () => {
    turnO = true;
    enableBoxes();
    msgContainer.classList.add("hide");
};
```

- **resetGame()**: This function resets the game to its initial state.
    - **turnO = true**: The turn is reset to Player O (so Player O will play first).
    - **enableBoxes()**: It makes all the boxes clickable again.
    - **msgContainer.classList.add("hide")**: Hides the message container when the game is reset.

---

### 5. **Box Click Event**

```javascript
boxes.forEach((box) => {
    box.addEventListener("click", () => {
        console.log("Box was clicked");
        if(turnO) { // Player O's turn
            box.innerText = "O";
            turnO = false; // Next turn is for Player X
        } else { // Player X's turn
            box.innerText = "X";
            turnO = true; // Next turn is for Player O
        }
        box.disabled = true; // Disable the box after it’s clicked (so it can’t be clicked again)

        checkWinner(); // Check if the current player has won after this move
    });
});
```

- **boxes.forEach()**: This loop goes through each box (cell) on the game board.
- **box.addEventListener("click")**: When a box is clicked, it checks whose turn it is.
    - If it’s **Player O's turn**, it places "O" in the box, and it’s now **Player X's turn**.
    - If it’s **Player X's turn**, it places "X" in the box, and it’s now **Player O's turn**.
- **box.disabled = true**: Once a player places their mark in the box, it’s disabled so no one can click it again.
- **checkWinner()**: After every move, the game checks if there’s a winner.

---

### 6. **Disable Boxes (After Game Ends)**

```javascript
const disableBoxes = () => {
    for (let box of boxes) {
        box.disabled = true;
    }
};
```

- **disableBoxes()**: This function makes all the boxes unclickable (disabled) after the game ends, whether it's a win or a draw.

---

### 7. **Enable Boxes (For New Game)**

```javascript
const enableBoxes = () => {
    for (let box of boxes) {
        box.disabled = false; // Make the boxes clickable again
        box.innerText = ""; // Clear the text inside each box
    }
};
```

- **enableBoxes()**: This function is used to reset the board for a new game. It makes all the boxes clickable again and removes any text from the boxes (clears the "X" or "O").

---

### 8. **Display Winner Message**

```javascript
const showWinner = (winner) => {
    msg.innerText = `Congratulations, Winner is ${winner}`;
    msgContainer.classList.remove("hide"); // Show the message
    disableBoxes(); // Disable the boxes so no one can play anymore
};
```

- **showWinner(winner)**: This function shows the message declaring who the winner is (either Player "X" or Player "O"). It also disables the boxes so no further moves can be made.

---

### 9. **Check for Winner**

```javascript
const checkWinner = () => {
    for (let pattern of winPatterns) {
        let pos1Val = boxes[pattern[0]].innerText;
        let pos2Val = boxes[pattern[1]].innerText;
        let pos3Val = boxes[pattern[2]].innerText;

        if (pos1Val !== "" && pos2Val !== "" && pos3Val !== "") {
            if (pos1Val === pos2Val && pos2Val === pos3Val) {
                console.log("Winner", pos1Val);
                showWinner(pos1Val);
            }
        }
    }
};
```

- **checkWinner()**: This function checks all possible winning patterns.
    - It looks at the boxes defined in **winPatterns**.
    - If three boxes have the same symbol (either "X" or "O") in a row, column, or diagonal, the game declares a winner by calling the **showWinner()** function.

---

### 10. **Buttons for Reset and New Game**

```javascript
newGameBtn.addEventListener("click", resetGame);
resetBtn.addEventListener("click", resetGame);
```

- **newGameBtn.addEventListener()** and **resetBtn.addEventListener()**: These buttons trigger the **resetGame()** function when clicked. This will reset the game and allow players to start a fresh game.

---

### Final Summary:

1. **Game Setup**: You have 9 boxes for the Tic Tac Toe grid, and two buttons to reset or start a new game.
2. **Gameplay**: Players take turns clicking the boxes, placing "X" and "O" in them.
3. **Winner Check**: The game checks if there’s a winner after every move. If a player wins, a message is displayed.
4. **Reset/New Game**: You can reset the game or start a new one using the buttons.

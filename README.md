# js-sudoku
Thus repo is a solution to a [leetCode problem](https://leetcode.com/problems/sudoku-solver/ "Sudoku Solver")

![problem.png](https://github.com/FuTTiiZ/js-sudoku/raw/main/problem.png)

### Solution:
```js
// Returns whether or not we're allowed to write 'val' into the specified cell
const isValid = (board, x, y, val) => {
  // We're running this 9 step loop, as we're going to iterate in 9 steps multiple times
  // This includes iterating over the row, column and "box" of the cell
  for (let i = 0; i < 9; i++) {
    if (board[y][i] === val) return false // Here, we're using 'i' to iterate over the row
    if (board[i][x] === val) return false // Here, we're using 'i' to iterate over the column
    
    // Here, we're using 'i' to iterate over each cell in the 9x9 "box" the specified cell is a part of
    const boxCell = board[3 * Math.floor(y/3) + Math.floor(i/3)][3 * Math.floor(x/3) + i % 3]
    
    if (boxCell === val) return false
  }
  
  // If we've not returned out of the function yet, it must be valid 
  return true
}

const solveSudoku = board => {
  // Loop over each cell on the board
  for (let y = 0; y < 9; y++)
    for (let x = 0; x < 9; x++) {
      
      // If it's not empty, we'll continue to the next cell
      if (board[y][x] !== '.') continue

      // Try every number 'n' from [1;9]
      for (let n = 1; n < 10; n++) {
        const nStr = n+'' // Convert it to a string
        
        // If we're unable to write it into the cell,
        // we'll continue to the next number
        if (!isValid(board, x, y, nStr)) continue
        
        // Try writing it to the board
        board[y][x] = nStr
        
        // Run 'solveSudoku' with the modified board
        // This will start a recursive tree continuing 
        // until we've either found a solution, or reached
        // a dead end
        const step = solveSudoku(board)
        
        // If at any point in the recursive tree we've
        // reached the return statement on line 60, and
        // therefore a solution, we finally return the
        // current step as the solution
        if (step) return step
        
        // If we've come this far, but haven't reached
        // the solution, we must've reached a dead end
        // Therefore we reset the value of the original
        // cell we modified, and start over
        board[y][x] = '.'
      }
      
      return false
    }
      
  return board
}
```

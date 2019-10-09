/*_______VARIABLES_______ */

// global variable that keeps track of the game status
let board;

// enum representing flags that a cell can have
const flags = {
  // no user-setted flags
  NONE: '.',
  // user-setted <F> flag
  FLAG: 'F',
  // user-setted <?> flag
  QUESTION_MARK: '?'
}

/*_______CLI_FUNCTIONS_______ */

// correctly displays a <cell> in the CLI
const displayCell = (cell) => {
  if (cell.discovered) {
    return cell.value;
  } else {
    return cell.flag;
  }
}

// correctly displays a <line> in the CLI
const displayLine = (line, outputLine) => {
  line.map(cell => {
    outputLine = outputLine.concat('|' + displayCell(cell) + '| ');
  });
  console.log(outputLine);
  console.log("---------------------------------------");
}

// display in readable way the Mined Field <board>, it keeps into account the discovered or not cells and displays them properly in the CLI
const show = (board) => {
  console.log("----------------------------------------");
  let lastLine = " " + " " + " " + " ";
  board.map(line => {
    lastLine = lastLine.concat("|" + board.indexOf(line) + " " + " ");
    let outputLine = "|" + board.indexOf(line) + " " + " ";
    displayLine(line, outputLine);
  });
  console.log(lastLine);
  console.log("----------------------------------------");
}

// CLI welcome message for the user and CLI commands list with instructions
const welcome = () => {
  console.log("Welcome to \n\n" + "XX     XX  XX  XX    XX  XXXXXXX XXXXX\nXXX   XXX  XX  XXX   XX  XX      XX  XX\nXX XXX XX  XX  XX X  XX  XX      XX   XX\nXX  X  XX  XX  XX  X XX  XXXXX   XX   XX\nXX     XX  XX  XX   XXX  XX      XX   XX\nXX     XX  XX  XX    XX  XX      XX   XX\nXX     XX  XX  XX    XX  XX      XX  XX\nXX     XX  XX  XX    XX  XXXXXXX XXXXX\n\n\nXXXXXXX XX  XXXXXXX  XX      XXXXX\nXX      XX  XX       XX      XX  XX\nXX      XX  XX       XX      XX   XX\nXXXXX   XX  XXXXX    XX      XX   XX\nXX      XX  XX       XX      XX   XX\nXX      XX  XX       XX      XX   XX\nXX      XX  XX       XX      XX  XX\nXX      XX  XXXXXXX  XXXXXXX XXXXX\n");
  cmd();
  show(board);
}

// prints the list of available commands
const cmd = () => {
  console.log("List of commands: \n" +
    '\n' +
    "1. to discover a cell type:\n   discover([row index], [line index])" +
    '\n' +
    '\n' +
    "2. to set a flag on a cell type:\n   setFlag([row index], [column index])" +
    '\n' +
    '\n' +
    "3. to set a question mark on a cell type:\n   setMark([row index], [column index])" +
    '\n' +
    '\n' +
    "4. to delete a mark on a cell type:\n   removeFlag([row index], [column index])" +
    '\n' +
    '\n' +
    "5. to restart the game type:\n   start()" +
    '\n' +
    '\n' +
    "6. to see this list again type:\n   cmd()" +
    '\n' +
    '\n');
}

/*_______GAME_LOGIC_______ */

// if no errors are thrown, returns the cell inside given <board> with specified <line> and <column>
const getCell = (board, line, column) => {
  // cloning <board>
  const newBoard = [...board];
  // checks user inputs <line> and <column> range 
  if (!(validateIndex(line, newBoard) && validateIndex(column, newBoard))) {
    // throws a proper error when board limits are exceeded
    throw new Error("Invalid line or column format! Numbers outside of range.");
  } else {
    const cell = newBoard[line][column];
    return cell;
  }
}

// called by <discoverCell> when it discovers an empty value cell
// checks surroundings of the empty cell inside given <board> with specified <line> and <column> to discover more
const discoverAdjacent = (board, line, column) => {
  // cloning board
  let newBoard = [...board];
  // prepared surroundings indexes
  let leftColumn = column - 1;
  let rightColumn = column + 1;
  let upperLine = line - 1;
  let lowerLine = line + 1;

  // from now on all IFs are needed to validate the cell indexes
  if (validateIndex(leftColumn, newBoard)) {
    // discovers left side cell on the same line
    newBoard = discoverCell(newBoard, line, leftColumn);
    if (validateIndex(lowerLine, newBoard)) {
      // discovers lower left corner cell
      newBoard = discoverCell(newBoard, lowerLine, leftColumn);
    }
    if (validateIndex(upperLine, newBoard)) {
      // discovers upper left corner cell
      newBoard = discoverCell(newBoard, upperLine, leftColumn);
    }
  }
  if (validateIndex(rightColumn, newBoard)) {
    // discovers left side of the same line
    newBoard = discoverCell(newBoard, line, rightColumn);
    if (validateIndex(lowerLine, newBoard)) {
      // discovers lower right corner cell
      newBoard = discoverCell(newBoard, lowerLine, rightColumn);
    }
    if (validateIndex(upperLine, newBoard)) {
      // discovers upper right cell
      newBoard = discoverCell(newBoard, upperLine, rightColumn);
    }
  }
  if (validateIndex(lowerLine, newBoard)) {
    // discovers lower cell on the same column
    newBoard = discoverCell(newBoard, lowerLine, column);
  }
  if (validateIndex(upperLine, newBoard)) {
    // discovers upper cell on the same column
    newBoard = discoverCell(newBoard, upperLine, column);
  }
  // returns the newly constructed board
  return newBoard;
}

// discovers the cell in the given <line> and <column> of the provided game <board> and returns a new board instance containing all mutations needed
const discoverCell = (board, line, column) => {
  // cloning board 
  let newBoard = [...board];
  // cloning interessed line
  let newLine = [...newBoard[line]];
  // cloning interessed cell
  const cell = Object.assign({}, getCell(newBoard, line, column));

  // if the cell has already been discovered 
  if (cell.discovered) {
    // no need to do anything
    return newBoard;
  } else {
    // otherwise sets appropriate property's values of the game board
    cell.discovered = true;
    newLine[column] = cell;
    newBoard[line] = newLine;
  }
  // checks for losing condition
  if (cell.value === "*") {
    console.log("You lose! :(\n");
    return newBoard;
  }
  // checks if further discoverings in the surroundings are needed
  if (cell.value === " ") {
    // discovers cells surrounding an empty-valued cell
    newBoard = discoverAdjacent(newBoard, line, column);
  }
  // returns the new board filled with the just-discovered cells
  return newBoard;
}

// sets the flag property of the cell in provided <line> and <column> of the game <board> to the given <flagType>
const markCell = (board, line, column, flagType) => {
  // cloning board 
  let newBoard = [...board];
  // cloning interessed line
  let newLine = [...newBoard[line]];
  // cloning interessed cell
  const cell = Object.assign({}, getCell(newBoard, line, column));
  if (cell.discovered) {
    // if the cell has already been discovered, so it cannot be marked and signals it to the user
    console.log("Cannot add a flag on a discovered cell!\n");
    return newBoard;
  } else {
    // otherwise sets appropriate property's value of the game board
    cell.flag = flagType;
    newLine[column] = cell
    newBoard[line] = newLine;
  }
  return newBoard;
}

// checks if all cells undiscovered in the <board> have the bomb value
const checkWin = (board) => {
  const newBoard = [...board];
  if (newBoard.find(line => {
    return line.find(cell => {
      return cell.value !== "*" && cell.discovered === false
    });
  })) {
    return false;
  } else {
    return true;
  }
}

/*_______UTILS_______ */

// validates the cell in the given <line> and <column> of the <board> (if possible), otherwise it outputs an error message
const validateCell = (board, line, column) => {
  try {
    // throws invalid indexes error
    getCell(board, line, column);
  } catch (error) {
    // re-throws it to the caller
    throw new Error(error.message);
  }
}

const validateIndex = (index, board) => {
  return index >= 0 && index < board.length
}

// returns a random number included between <start> value and <end> value, if no parameter is provided it will return a random value between 1 and 6
function getRandomNum(start, end) {
  let s = start || 1;
  let e = end || 6;
  return Math.floor(Math.random() * e) + s;
}

/*_______INITIALIZER_______ */

// initializes the game by randomly selecting a map from the JSON collection of maps and assignes it to the global object, representing the game status 
const start = () => {
  // parsing from JSON
  const fileContents = fs.readFileSync('maps.json', 'utf-8');
  const maps = JSON.parse(fileContents);
  // selecting random map for the game board
  board = maps[getRandomNum(0, maps.length - 1)];
  // setting properties needed for the game logic
  board.map(line => {
    (line.map(cell => {
    cell.discovered = false;
    cell.flag = flags.NONE;
  }));
  });
  // outpust in CLI the welcome message for the user and the list of commands
  welcome();
}

/*_______WRAPPERS_______ */

// template function to create all operations needed to play
// takes as input <func> that is the function to be excuted
// returns another function that takes as inputs the user input and further <args> if needed
const operation = (func) => {
  return (line, column, args) => {
    // validates user inputs
    validateCell(board, line, column);
    // executes the <curried??> function
    board = func(board, line, column, args);
    // checks if conditions for the win are satisfied
    if (checkWin(board)) {
      console.log("You win! :D\n");
    }
    // displays board in CLI
    show(board);
  }
}

// discovers cell in the given <line> and <column> of the board
const discover = (line, column) => {
  const discovering = operation(discoverCell);
  try {
    // throws invalid indexes error
    discovering(line, column);
  } catch (error) {
    // signals it to the user
    console.log(error.message);
  }
}

// marks with a flag (F) the cell in the given <line> and <column> of the board
const setFlag = (line, column) => {
  const mark = operation(markCell);
  try {
    // throws invalid indexes error
    mark(line, column, flags.FLAG);
  } catch (error) {
    // signals it to the user
    console.log(error.message);
  }
}

// marks with a flag (?) the cell in the given <line> and <column> of the board
const setMark = (line, column) => {
  const mark = operation(markCell);
  try {
    // throws invalid indexes error
    mark(line, column, flags.QUESTION_MARK);
  } catch (error) {
    // signals it to the user
    console.log(error.message);
  }
}

// unmarks cell in the given <line> and <column> of the board
const removeFlag = (line, column) => {
  const mark = operation(markCell);
  try {
    // throws invalid indexes error
    mark(line, column, flags.NONE);
  } catch (error) {
    // signals it to the user
    console.log(error.message);
  }
}

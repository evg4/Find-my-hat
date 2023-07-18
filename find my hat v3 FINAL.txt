const prompt = require("prompt-sync")({ sigint: true });

const hat = "^";
const hole = "O";
const fieldCharacter = "░";
const pathCharacter = "*";
let y = 0;
let x = 0;
let cont = true;

class Field {
  constructor(Array) {
    this.field = Array;
  }

  print() {
    for (let i = 0; i < this.field.length; i++) {
      console.log(this.field[i].join(""));
    }
  }

  static generateField(height, width) {
    let array = [];
    function getRandSquare() {
      let num = Math.floor(Math.random() * 2);
      switch (num) {
        case 0:
          return "░";
          break;
        case 1:
          return "O";
          break;
      }
    }
    for (let j = 0; j < height; j++) {
      array[j] = [];
      for (let k = 0; k < width; k++) {
        array[j][k] = getRandSquare();
      }
    }
    array[0][0] = "*";
    array[Math.floor(Math.random() * height)][
      Math.floor(Math.random() * width)
    ] = "^";

    return array;
  }
}

const MyField = Field.generateField(3, 4);
let Game = new Field(MyField);
console.log(
  "Instructions: You start at the position marked *. You must move one square at a time until you find your hat ^. If you fall down a hole O, you lose. If you step outside the boundaries of the game, you lose. Press D to go down, U to go up, L to go left and R to go right."
);

while (cont === true) {
  Game.print();
  let move = prompt("Which way?");
  switch (move) {
    case "D":
      y++;
      break;
    case "U":
      y--;
      break;
    case "L":
      x--;
      break;
    case "R":
      x++;
      break;
    default:
      console.log("Type a valid letter: D, U, L or R");
      break;
  }
  if (y < 0 || x < 0) {
    console.log("Out of bounds - you lose!");
    cont = false;
  } else if (Game.field[y][x] === "^") {
    console.log("You won!");
    cont = false;
  } else if (Game.field[y][x] === "O") {
    console.log("You fell down a hole - you lose!");
    cont = false;
  } else {
    Game.field[y][x] = "*";
  }
}

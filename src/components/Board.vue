<template>
  <div>
    <h2 class="message" :class="colour">{{ status }}</h2>
    <table class="board">
      <tr>
        <Cell name="1"></Cell>
        <Cell name="2"></Cell>
        <Cell name="3"></Cell>
      </tr>
      <tr>
        <Cell name="4"></Cell>
        <Cell name="5"></Cell>
        <Cell name="6"></Cell>
      </tr>
      <tr>
        <Cell name="7"></Cell>
        <Cell name="8"></Cell>
        <Cell name="9"></Cell>
      </tr>
    </table>
  </div>
</template>

<script>
import { bus } from '../main.js';
import Cell from './Cell.vue';

export default {
  name: 'Board',
  components: {
    Cell
  },
  data () {
    return {
      player: 'X',
      status: "X's turn",
      isWon: false,
      moves: 0,
      cells: {
        1: '', 2: '', 3: '',
				4: '', 5: '', 6: '',
				7: '', 8: '', 9: ''
      },
      colour: 'turn'
    }
  },
  //runs when the component is created
  created () {
    //listen for clicks
    bus.$on('clicked', (cellNumber) => {
      //set the specific cell to the player (X or O)
      this.cells[cellNumber] = this.player;
      //increment moves to check for draw condition
      this.moves++;
      
      this.game();
    });
  },
  //watchers will fire when the data changes
  watch: {
    //wait for isWon to change to declare a winner
    isWon(){
      this.status = `${this.player} wins!`;
      this.colour = 'win';
    }
  },
  methods: {
    //function to change player
    changePlayer(){
      if (this.player === 'X') {
        this.player = 'O';
        this.status = "O's turn";
      } else {
        this.player = 'X';
        this.status = "X's turn";
      }
    },
    //function for the game
    game(){
      if(this.moves === 9){
      //check winConditions if 9 moves were made to see if there was a winner on the 9th move
        if(this.winConditions() === false){
	  //if winConditions returns false, none of the possible win conditions were met, so the game is a draw
          this.status = "Draw";
          this.colour = "draw";
        } else return;
      } else if (this.winConditions() === false) {
        //if there is no winner and the max number of moves were not made, change the player
        this.changePlayer();
      } else {
        return;
      }
    },
    //function to check for win conditions
    winConditions(){
      //check for horizontal wins
      if (this.cells[1] === this.player && this.cells[2] === this.player && this.cells[3] === this.player) {
        //change isWon so the watcher executes
        this.isWon = true;
        //emit win event to stop clicking of cells in the cell component
        bus.$emit('win');
        return true;
      }
      if (this.cells[4] === this.player && this.cells[5] === this.player && this.cells[6] === this.player) {
        this.isWon = true;
        bus.$emit('win');
        return true;
      }
      if (this.cells[7] === this.player && this.cells[8] === this.player && this.cells[9] === this.player) {
        this.isWon = true;
        bus.$emit('win');
        return true;
      }

      //check for vertical wins
      if (this.cells[1] === this.player && this.cells[4] === this.player && this.cells[7] === this.player) {
        this.isWon = true;
        bus.$emit('win');
        return true;
      }
      if (this.cells[2] === this.player && this.cells[5] === this.player && this.cells[8] === this.player) {
        this.isWon = true;
        bus.$emit('win');
        return true;
      }
      if (this.cells[3] === this.player && this.cells[6] === this.player && this.cells[9] === this.player) {
        this.isWon = true;
        bus.$emit('win');
        return true;
      }

      //check for diagonal wins
      if (this.cells[1] === this.player && this.cells[5] === this.player && this.cells[9] === this.player) {
        this.isWon = true;
        bus.$emit('win');
        return true;
      }
      if (this.cells[3] === this.player && this.cells[5] === this.player && this.cells[7] === this.player) {
        this.isWon = true;
        bus.$emit('win');
        return true;
      }
      
      //return false if no win conditions are met
      return false;
    }
  }
}
</script>

<style>
.board {
  margin: 80px auto;
  color: #fff;
  width: 25%;
  border-collapse: collapse;
  font-size: 3.5em;
  border: 1px solid #2c3e50;
}
.message {
  margin-top: 60px;
  margin-bottom: 0;
}
.win {
  color: green;
}
.draw {
  color: red;
}
.turn {
  color: #2c3e50;
}
</style>

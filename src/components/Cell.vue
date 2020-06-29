<template>
  <td @click="clicked">{{ text }}</td>
</template>

<script>
import { bus } from '../main.js';

export default {
  name: 'Cell',
  //need to get the name of the cell that was clicked on from the board component
  props: ['name'],
  data () {
    return {
      text: '',
      empty: true
    }
  },
  //runs when the component is created
  created(){
    //listen for the win event to be fired to stop a player from placing another letter after someone wins the game
    bus.$on('win', () => {
      this.empty = false;
    });
  },
  methods: {
    clicked(){
      //add logic to check if a cell is empty so you can't change an already occupied cell
      if(this.empty === true){
        //get the player (x or o) from the parent board component and show it in the cell
        this.text = this.$parent.player;

        //set the empty property to false so you can't change the player of that cell anymore
        this.empty = false;

        //emit an event to tell the board component that a new cell was clicked
        bus.$emit('clicked', this.name);
      }
    }
  }
}
</script>

<style>
td {
  width: 33.333%;
  border: 6px solid #2c3e50;
  color: #2c3e50;
  height: 130px;
}
td:hover {
  background-color: #ebebeb;
  cursor: pointer;
}
td:first-of-type {
  border-left-color: transparent;
  border-top-color: transparent;
}
td:nth-of-type(2) {
  border-top-color: transparent;
}
td:nth-of-type(3) {
  border-right-color: transparent;
  border-top-color: transparent;
}
tr:nth-of-type(3) td {
  border-bottom-color: transparent;
}
</style>
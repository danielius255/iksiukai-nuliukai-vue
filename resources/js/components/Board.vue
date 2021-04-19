<template>
    <div id="game-view">
        
    <div id="game-view-info">
    {{infoMessage}}
    </div>
    <div id="game-view-squares">

      <div 
       v-for="(square, i) in squares"
       v-on:click="makeMove(i)"
       v-bind:class="{highlighted: square.isHighlighted}"
       class="game-view-square">
      {{square.value}}
     </div>
       
     </div>
    </div>
    
</template>

<script>
import Square from '../Square';
export default {
  name: "Board",
  data: function() {
    return {
      inProgress: true,
      winner: null,
      currentTurn: "O", 
      movesMade: 0,
      squares: new Array(9).fill().map (s => new Square()),
    };
  },
  methods: {
makeMove: function(i){
    
if(this.inProgress && !this.squares[i].value){
    this.squares[i].value = this.currentTurn;

    this.movesMade++;
    this.checkForWinner();
    this.currentTurn = (this.currentTurn === "O")? "X" : "O";
}
},

checkForWinner: function(){
 const winningCombinations = [ [0,1,2],[3,4,5],[6,7,8],[0,3,6],[1,4,7],[2,5,8],[0,4,8],[2,4,6]]; 

 winningCombinations.forEach((wc)=>{
     const [a,b,c] = wc;
     const sqA = this.squares[a];
     const sqB = this.squares[b];
     const sqC = this.squares[c];

     if (sqA.value && sqA.value === sqB.value && sqA.value === sqC.value){
         this.inProgress=false;
         this.winner = sqA.value; 
         sqA.isHighlighted = sqB.isHighlighted = sqC.isHighlighted = true;
     }
 });
if(this.movesMade === this.squares.length){
    this.inProgress=false; 

}
}
  },
  
  computed: {
            infoMessage: function(){
                if(this.inProgress){
                    return this.currentTurn+'\ eilė';
                }
                if(this.winner){
                    return this.winner +' laimėjo!';
                }
                if (!this.winner && !this.inProgress)
                return 'Lygiosios'
            }

        }
};
</script>
<style>
#game-view{
    width: 500px;
    margin: 0 auto;
    border: 1px solid  #000;
}
#game-view-info{
    padding: 15px;
    font-family: sans-serif;
    font-size: 20px;
    font-weight: bold;
    text-align: center;
    background-color:  #eee;
}
#game-view-squares{
    height: 500px;
    display: flex;
    flex-wrap: wrap;
    padding: 25px;
    box-sizing: border-box;
}
.game-view-square{
    width:33.33%;
    height: 33.33%;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    font-family: cursive;
    font-size: 75px;
    text-transform: uppercase;
    border-radius: 15px;
    cursor: pointer;
    user-select: none;
    -moz-user-select: none;
}
.game-view-square.highlighted {color: green}
.game-view-square:hover {background-color:  #eee}
.game-view-square:nth-child(-n+6){border-bottom: 15px solid purple;}
.game-view-square:nth-child(3n+1),
.game-view-square:nth-child(3n+2){border-right: 15px solid purple;}
</style>
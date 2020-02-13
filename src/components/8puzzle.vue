<template>
<div class="b-puzzle-parent">
    8 Puzzle here..
    <div class="b-puzzle-board">
    <div class="b-puzzle">
        <div class="b-puzzle-hor">
            <div 
            v-for="(cube, i) in cubes" 
            :key="i"
            >
                <div class="b-puzzle-ver">
                    <div 
                    v-for="(c, j) in cube" 
                    :key="j"
                    >
                        <div 
                        @click="swap(i, j)" 
                        class="b-puzzle-block"
                        :class="{'b-background-block':(c != -1), 'b-background-blank':(c == -1)}"
                        >
                            <div class="b-puzzle-block-child">
                                <div v-if="c !== -1">
                                    {{ cubes[i][j] }}
                                </div>
                                <div v-else>
                                    {{ "  - Hello -  " }}                
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    </div>
    <div 
    :text="text"
    :class="{'b-correct-move':(text === 'correct move'), 'b-in-correct-move' : (text === 'Incorrect move')}"
    style="margin-bottom : 24px"
    >
    {{ text }}
    </div>
    <div
    :moves="moves"
    >
        Number Of Moves : {{ moves }}
        <button
        @click="shuffle()"
        >
        Shuffle
        </button>
    </div>
</div>
</template>
<script>
export default {
    data(){
        return{
            cubes : [
                [1, 2, 3],
                [4, -1 , 5],
                [7, 8, 6]
            ],
            goal : [
                [1, 2, 3],
                [4, 5, 6],
                [7, 8, -1]
            ],
            i : 0,
            text : ' --- ',
            moves : 0,
        }
    },
    methods : {
        swap(i, j){
            var par = {
                    x : -1,
                    y : -1
                }
            // console.log('check : ' + this.check(i, j).x + ' , ' + this.check(i, j).y);
            // console.log('par : ' + par.x + ' ' + par.y);
            // console.log(par.x === this.check(i, j).x)
            // if(this.check(i, j).x == par.x && this.check(i, j).y == par.y) console.log('Hello there!!');
            // console.log('check : ' + this.check(i, j).x + ' ' + this.check(i, j).y);

            if(this.check(i, j).x != par.x && this.check(i, j).y != par.y){
                this.moves = this.moves + 1;
                this.text = 'correct move'
                // console.log(par.x, par.y);
                par = this.check(i, j);
                var c = this.cubes[i][j];
                this.$set(this.cubes[i], j, this.cubes[par.x][par.y]);
                this.$set(this.cubes[par.x], par.y, c);
            }
            
        },
        check(i, j){
            var par= {
                x : -1,
                y : -1,
            };
            if(j!=0 && this.cubes[i][j-1] == -1){
                par.x = i;
                par.y = j-1
                // return par;
            }
            else if(j!=2 && this.cubes[i][j+1] == -1){
                par.x = i;
                par.y = j+1;
                // return par;
            }
            else if(i!=0 && this.cubes[i-1][j] == -1){
                par.x = i-1;
                par.y = j;
                // return par;
            }
            else if(i!=2 && this.cubes[i+1][j] == -1){
                par.x = i+1;
                par.y = j;
                
                // return par;
            } 
            else   this.text = 'Incorrect move';
            // console.log('par : ' + par.x + ' ' + par.y)
            return par;
    
        },
        shuffle(){
            for(var i = 0; i < 3; ++i){
                for(var j = 0; j < 3; ++j){
                    this.cubes[i][j] = -1;
                }
            }
            var c = 0, x = 0, randomnumber = Math.floor(Math.random() * (1 - 9 + 1)) + 9;
            for( i = 0; i < 3; ++i){
                for( j = 0; j < 3; ++j){
                    while(c === 0){
                        randomnumber = Math.floor(Math.random() * (1 - 9 + 1)) + 9;
                        for(var k = 0; k < 3; ++k){
                            for(var l = 0; l < 3; ++l){
                                if(this.cubes[k][l] == randomnumber) x = 1;
                            }
                        }
                        if(x === 0) c = 1;
                    }
                    this.cubes[i][j] = randomnumber;
                    c = 0;
                    x = 0;
                }
            }
            for( i = 0; i < 3; ++i){
                for( j = 0; j < 3; ++j){
                    if(this.cubes[i][j] == 9) this.cubes[i][j] = -1;
                }
                // console.log('hello world');
                
            }
            
        }
    },
    
    computed : {

    },
}
</script>
<style>
.b-puzzle-hor{
    display: flex;
    flex-direction: row;
    /* width: 300px; */
    /* background-color: red; */
}
.b-puzzle-ver{
    /* height: 300px; */
}
.b-puzzle-block{
    height: 100px;
    width: 100px;
    display: flex;
    flex-direction: row;
    justify-content: center;
    border-style: solid;
    border-width: 1.5px;
    border-color: #82d0be;
    box-shadow: 2px 3px #888880;
    cursor: pointer;
    margin: 1px 1px 1px 1px;
    align-content: center;
    align-items: center;
}
.b-puzzle-board{
    margin-top: 24px;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    margin-bottom: 64px;
}
.b-correct-move{
    color: green;
    margin-bottom: 24px;
}
.b-in-correct-move{
    margin-bottom: 24px;
    color: red;
}
.b-background-block{
    background-color: aquamarine;
}
.b-background-blank{
    background-color: cadetblue;
}
@media screen and (max-width: 647px) {
  .b-puzzle-block{
      height: 50px;
      width: 50px;
  }
}

</style>
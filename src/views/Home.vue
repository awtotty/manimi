<template>
  <v-container class="gray darken-4">
    <v-row>
      <!-- MOBJECTS -->
      <v-col
        cols="12"
        sm="3"
        lg="2"
      >
        <v-sheet
          rounded="lg"
          min-height="50"
          elevation="2"
        >

          <!-- start mobjects menu -->
          <v-list
              rounded="lg"
              dense
              na
          >
            <v-list-item-group 
            v-model="shapeItemsModel"
            mandatory
            >
              <v-list-item
                v-for="item in shapeItems"
                :key="item.usage"
                @click="setCurrShapeString(item.name)"
              >
                <v-list-item-icon>
                  <v-icon v-text="item.icon"></v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title v-text="item.name"></v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
          <!-- end mobjects menu -->

        </v-sheet>
      </v-col>

      <!-- SCENE -->
      <v-col
        ref="hello"
        cols="12"
        sm="6"
        lg="8"
      >
        <v-sheet
          height="85vh"
          rounded="lg"
        >
          <v-container id="scene-container">

            <!-- start scene -->
            <vue-p5
              id="p5-ctx"
              @setup="setup"
              @draw="draw"
              @keypressed="keyPressed"
              @mousepressed="mousePressed"
              @mousereleased="mouseReleased"
            >
            </vue-p5>
            <!-- end scene -->

          </v-container>
        </v-sheet>
      </v-col>

      <!-- ANIMS -->
      <v-col
        cols="12"
        sm="3"
        lg="2"
      >
        <v-sheet
          rounded="lg"
          min-height="50" 
          elevation="2"
        >

          <!-- start anims menu -->
          <v-list
          class="text-center"
          rounded="lg"
          dense
          v-if="this.selectedMobject != null 
                && this.selectedMobject.typeStr.toLowerCase() != 'mouse'" 
          >
              <!-- selectedMobject type -->
              <v-list-item>
                  <v-list-item-content>
                  <v-list-item-text>{{ selectedMobject.typeStr }}</v-list-item-text>
                  </v-list-item-content>
              </v-list-item>

              <v-divider></v-divider>

              <!-- selectedMobject anims -->
              <v-list-item-group 
              v-model="selectedMobject.stateModel"
              mandatory
              >
                <v-list-item
                  v-for="(state, i) in selectedMobject.states"
                  :key="i"
                >
                  <v-list-item-content>
                    <v-list-item-text v-item="state">Pos: ({{state.x}}, {{state.y}})</v-list-item-text>
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>


              <!-- anim trash and add buttons -->
              <v-list-item> 
                <v-row align="center" justify="space-around">
                  <v-tooltip bottom>
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn
                        class="mx-2"
                        rounded
                        dark
                        small 
                        color="#525893"
                        v-bind="attrs"
                        v-on="on"
                        @click="addStateToMobject(selectedMobject, selectedMobject.stateModel)"
                      >
                        <v-icon dark>
                          mdi-plus
                        </v-icon>
                      </v-btn>
                    </template>
                    <span>Add Animation</span>
                  </v-tooltip>

                  <v-tooltip bottom>
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn
                        class="mx-2"
                        rounded
                        dark
                        small 
                        color="#e07a5f"
                        v-bind="attrs"
                        v-on="on"
                        @click="deleteStateFromMobject(selectedMobject, selectedMobject.stateModel)"
                      >
                        <v-icon dark>
                          mdi-delete
                        </v-icon>
                      </v-btn>
                    </template>
                    <span>Delete Animation</span>
                  </v-tooltip>
                </v-row>
              </v-list-item>

              <v-divider></v-divider>

              <!-- delete shape button -->
              <v-list-item>
                <v-tooltip bottom>
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn
                      class="white--text"
                      block
                      rounded
                      color="#e07a5f"
                      v-bind="attrs"
                      v-on="on"
                      @click="deleteShapeAndCleanup(selectedMobject)"
                    >
                      Delete
                    </v-btn>
                  </template>
                  <span>Delete Object</span>
                </v-tooltip>
              </v-list-item>
          </v-list>
          <!-- end anims menu -->

        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
import VueP5 from "vue-p5";

export default ({
  name: "Home",

  components: {
    "vue-p5": VueP5
  },

  data: () => ({
    shapeItemsModel: 0, 
    shapeItems: [
      { name: 'Mouse', icon: 'mdi-cursor-default-outline' },
      { name: 'Circle', icon: 'mdi-circle-outline' },
      { name: 'Point', icon: 'mdi-circle-small' },
      { name: 'Square', icon: 'mdi-square-outline' },
      { name: 'Triangle', icon: 'mdi-triangle-outline' },
      // { name: 'Function', icon: 'mdi-function-variant' },
      // { name: 'LaTeX', icon: 'mdi-format-text' },
    ],
    shapes: [],
    animItems: [
      
    ],
    selectedMobject: null, 
    currShapeString: "Mouse", 
    distToToggleMenu: 20, 
  }),

  methods: {
    setup(sketch) {
      let cW = parseInt(this.$refs.hello.offsetWidth, 10);
      let cH = parseInt(this.$refs.hello.offsetHeight, 10);
      console.log(cW + "," + cH); 
      sketch.createCanvas(cW, cH);  
      sketch.rectMode(sketch.CENTER);

      sketch.noFill(); 
      sketch.strokeWeight(5); 
      sketch.stroke("blue"); 
    },

    pointOnCanvas(sketch, x, y) {
      return x < sketch.width
          && x > 0 
          && y < sketch.height
          && y > 0 
    },
    
    draw(sketch) {
      sketch.background("black"); 

      for (let i = 0; i < this.shapes.length; i++) {		
        sketch.noFill(); 
        sketch.strokeWeight(5);
        sketch.stroke("blue"); 

        let currShape = this.shapes[i]; 
        // check for drag
        if (currShape.dragging) {
          if (currShape.stateModel < 0 || currShape.stateModel < 0) {
            console.error("Selected state index is invalid: " + currShape.stateModel); 
            continue; 
          }

          // update current state 
          let newState = currShape.states[currShape.stateModel];  
          newState.x = sketch.int(sketch.mouseX + currShape.offsetX);
          newState.y = sketch.int(sketch.mouseY + currShape.offsetY);

          // NOTE: this has to be done with a remove/add to get Vue to rerender the anim component
          currShape.states.splice(currShape.stateModel, 1, newState); 
        }
        // this.shapes[i] = currShape; 

        // draw shape
        let currState = currShape.states[currShape.stateModel]; 
        if (currShape.typeStr.toLowerCase() === "circle") {
          this.drawCircle(sketch, currState.x, currState.y, 50, 50); 
        }
        else if (currShape.typeStr.toLowerCase() === "point") {
          this.drawPoint(sketch, currState.x, currState.y, 5, 5); 
        }
        else if (currShape.typeStr.toLowerCase() === "triangle") {
          this.drawTriangle(sketch, currState.x, currState.y, 50, 50); 
        }
        else if (currShape.typeStr.toLowerCase() === "square") {
          this.drawRect(sketch, currState.x, currState.y, 50, 50); 
        }
      }
    }, 

    mousePressed(sketch) {
      if (!this.pointOnCanvas(sketch, sketch.mouseX, sketch.mouseY)) {
        return; 
      }

      // using mouse tool? 
      if (this.currShapeString.toLowerCase() === "mouse") {
        // close enough to existing shape? find closest
        let minDist = Infinity; 
        for (let i = 0; i < this.shapes.length; i++) {	
          let shapeState = this.shapes[i].states[this.shapes[i].stateModel]; 
          let d = sketch.dist(sketch.mouseX, sketch.mouseY, shapeState.x, shapeState.y); 
          console.log(d); 
          if (d < this.distToToggleMenu ) {
            console.log("here"); 
            this.selectedMobject = this.shapes[i]; 
            minDist = d; 
          }
        }
        // not close enough, turn off menu
        if (minDist > this.distToToggleMenu) {
          this.selectedMobject = null; 
        }
        else {
          this.selectedMobject.dragging = true; 
          this.selectedMobject.offsetX = this.selectedMobject.states[this.selectedMobject.stateModel].x - sketch.mouseX; 
          this.selectedMobject.offsetY = this.selectedMobject.states[this.selectedMobject.stateModel].y - sketch.mouseY; 
        }
        return; 
      }
      
      // new shape
      this.addShape(  { 
                        typeStr: this.currShapeString, 
                        x: sketch.int(sketch.mouseX), 
                        y: sketch.int(sketch.mouseY), 
                        menuOn: true, 
                        states: [
                          { 
                            x: sketch.int(sketch.mouseX), 
                            y: sketch.int(sketch.mouseY), 
                          }, 
                        ],
                        stateModel: 0, 
                        dragging: false, 
                        offsetX: 0, 
                        offsetY: 0, 
                      } 
                  );
      // update current anim focus
      this.selectedMobject = this.shapes[this.shapes.length-1]; 

      // select mouse
      this.currShapeString = "mouse"; 
      this.shapeItemsModel = 0; 
    }, 

    mouseReleased(sketch) {
      if (this.selectedMobject != null) {
        this.selectedMobject.dragging = false; 
      }
    },

    setCurrShapeString(s) {
        this.currShapeString = s; 
    },

    addShape(shapeObj) {
      this.shapes.push(shapeObj); 
    },
    
    deleteShape(shapeObj) {
      let index = this.shapes.indexOf(shapeObj); 
      if (index > -1) {
        this.shapes.splice(index, 1); 
        return true; 
      }
      return false; 
    },

    deleteShapeAndCleanup(shapeObj) {
      if (this.deleteShape(shapeObj))  {
        this.selectedMobject = null; 
      }
    },

    addStateToMobject(shapeObj, indexToCopy) {
      let newState = Object.assign({}, shapeObj.states[indexToCopy]);
      shapeObj.states.splice(indexToCopy+1, 0, newState);
      shapeObj.stateModel = indexToCopy+1; 
    },

    deleteStateFromMobject(shapeObj, index) {
      let numStates = shapeObj.states.length;  
      // can't delete all anim states 
      if (numStates > 1 && index > -1 && index < numStates) {
        shapeObj.states.splice(index, 1); 
        if (shapeObj.stateModel >= shapeObj.states.length) {
          shapeObj.stateModel = shapeObj.states.length-1; 
        } 
        return true; 
      }
      return false; 
    },

    keyPressed(sketch) {
      if (sketch.key.toLowerCase() === "c")
        this.setCurrShapeString("Circle"); 
      if (sketch.key.toLowerCase() === "r")
        this.setCurrShapeString("Square"); 
      if (sketch.key.toLowerCase() === "p")
        this.setCurrShapeString("Point"); 
      if (sketch.key.toLowerCase() === "t")
        this.setCurrShapeString("Triangle"); 

      if (sketch.key.toLowerCase() === "d")
        console.log(this.shapes);
    },

    drawCircle(sketch, x, y, w, h) {
      sketch.ellipse(x, y, w, h); 
    }, 

    drawTriangle(sketch, x, y, w, h) {
      sketch.triangle(x-w/2, y+h/2, x+w/2, y+h/2, x, y-h/2); 
    }, 

    drawPoint(sketch, x, y, w, h) {
      sketch.ellipse(x, y, w, h); 
    }, 

    drawRect(sketch, x, y, w, h) {
      sketch.rect(x, y, w, h); 
    }, 
  },

  // render(h) {
  //   return h(VueP5, {on: this});
  // }
})

</script>


<style lang="scss" scoped>
#scene-container {
  height: 85vh; 
  border-radius: 8px;
  overflow: hidden;
}
#p5-ctx {
  width: 100vw;
  height: 100vh;
  margin-left: -10px;
  margin-top: -12px;
  overflow: hidden;
}
</style>
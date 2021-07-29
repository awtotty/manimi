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
          rounded
          min-height="50"
          elevation="2"
        >

          <!-- start mobjects menu -->
          <v-list
              rounded
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
        ref="scene"
        cols="12"
        sm="6"
        lg="8"
      >
        <v-sheet
          height="85vh"
          rounded
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
          rounded
          min-height="50" 
          elevation="2"
        >

          <!-- start anims menu -->
          <v-list
          class="text-center"
          rounded
          dense
          v-if="this.selectedMobject != null 
                && this.selectedMobject.typeStr.toLowerCase() != 'mouse'" 
          >
              <!-- selectedMobject type -->
              <v-list-item>
                  <v-list-item-content>
                  <v-list-item>{{ selectedMobject.typeStr }}</v-list-item>
                  </v-list-item-content>
              </v-list-item>

              <v-divider></v-divider>

              <!-- selected state view -->
              <v-list
              class="state-list-stack-long"
              >
                <v-list-item>
                  <v-text-field
                    v-model="selectedMobject.states[selectedMobject.stateModel].x"
                    label="x"
                    class="mt-0 pt-0"
                    type="number"
                  ></v-text-field>
                </v-list-item>

                <v-list-item>
                  <v-text-field
                    v-model="selectedMobject.states[selectedMobject.stateModel].y"
                    label="y"
                    class="mt-0 pt-0"
                    type="number"
                  ></v-text-field>
                </v-list-item>

                <v-list-item>
                  <v-text-field
                    v-model="selectedMobject.states[selectedMobject.stateModel].rot"
                    label="Rotation (deg)"
                    class="mt-0 pt-0"
                    type="number"
                    step="15"
                  ></v-text-field>
                </v-list-item>

                <v-list-item>
                  <v-text-field
                    v-model="selectedMobject.states[selectedMobject.stateModel].size"
                    label="Size"
                    min="0"
                    class="mt-0 pt-0"
                    type="number"
                    step="0.1"
                  ></v-text-field>
                </v-list-item>

                <v-list-item>
                  <v-text-field
                    v-model="selectedMobject.states[selectedMobject.stateModel].color"
                    label="Color"
                    class="mt-0 pt-0"
                    type="text"
                  ></v-text-field>
                </v-list-item>

                <v-list-item>
                  <v-text-field
                    v-model="selectedMobject.states[selectedMobject.stateModel].time"
                    label="Time"
                    min="0"
                    class="mt-0 pt-0"
                    type="number"
                  ></v-text-field>
                </v-list-item>

              </v-list>

              <v-divider></v-divider>

              <!-- selectedMobject anims -->
              <v-list-item-group 
              class="state-list-stack-short"
              v-model="selectedMobject.stateModel"
              mandatory 
              >
                <v-list-item
                  v-for="(state, i) in selectedMobject.states"
                  :key="i"
                >
                State {{i}}
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
                    <span>Add State</span>
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
                    <span>Delete State</span>
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
import axios from 'axios';

export default ({
  name: "Home",

  components: {
    "vue-p5": VueP5
  },

  data: () => ({
    shapeItemsModel: 0, 
    shapeItems: [
      { name: 'Select', icon: 'mdi-cursor-default-outline' },
      { name: 'Circle', icon: 'mdi-circle-outline' },
      { name: 'Point', icon: 'mdi-circle-small' },
      { name: 'Square', icon: 'mdi-square-outline' },
      { name: 'Triangle', icon: 'mdi-triangle-outline' },
      // { name: 'Function', icon: 'mdi-function-variant' },
      // { name: 'LaTeX', icon: 'mdi-format-text' },
    ],
    shapes: [],
    selectedMobject: null, 
    shapeModel: null, 
    currShapeString: "Mouse", 
    distToToggleMenu: 20, 
    colorItems: [
      { name: "blue" } , 
      { name: "red" } , 
    ],
    canvasScaleFactor: 50,  
  }),

  methods: {
    getVid() {
      let vidId = "/m_" + Date.now()
      let server = 'http://localhost:5000/manim'
      // let server = "https://178.128.180.174:5000/manim"
      let path = server + vidId;

      // ask server to create vid
      axios({
        url: path,
        method: 'POST',
        responseType: 'blob',
        data: this.shapes, 
      }).then( (res) => {
        console.log("Server response to vid creation request: " + res.data); 
      }).catch( (error) => {
        console.error(error); 
      });

      // ask server to deliver vid
      axios({
        url: path,
        method: 'GET',
        responseType: 'blob',
      }).then( (res) => {
        console.log("Got download from server"); 
        let fileURL = window.URL.createObjectURL(new Blob([res.data], { type: "video/mp4" } ));
        let fileLink = document.createElement('a');
        fileLink.href = fileURL;
        fileLink.setAttribute('download', 'file.pdf');
        document.body.appendChild(fileLink);
        fileLink.click();
      }).catch( (error) => {
        alert("We were unable to create your video. Please check your scene and try again.");
        console.error(error); 
      });
    },

    setup(sketch) {
      let cW = parseInt(this.$refs.scene.offsetWidth, 10);
      let cH = parseInt(this.$refs.scene.offsetHeight, 10);
      console.log(cW + "," + cH); 
      sketch.createCanvas(cW, cH);  
      sketch.rectMode(sketch.CENTER);
      this.canvasScaleFactor = parseInt(cW / 14, 10); 

      sketch.noFill(); 
      sketch.strokeWeight(5); 
      sketch.frameRate(24); 
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

        let currShape = this.shapes[i]; 
        // check for drag
        if (currShape.dragging) {
          if (currShape.stateModel < 0 || currShape.stateModel < 0) {
            console.error("Selected state index is invalid: " + currShape.stateModel); 
            continue; 
          }

          // update current state 
          let newState = currShape.states[currShape.stateModel];  
          let adjustedCoords = this.convertCanvasCoordsToReal(sketch, sketch.mouseX+currShape.offsetX, sketch.mouseY+currShape.offsetY);
          newState.x = sketch.int(adjustedCoords.x);
          newState.y = sketch.int(adjustedCoords.y);

          // NOTE: this has to be done with a remove/add to get Vue to rerender the anim component
          currShape.states.splice(currShape.stateModel, 1, newState); 
        }

        // draw shape
        if (currShape.stateModel < 0 || currShape.stateModel >= currShape.states.length)
        {
          console.log("OH NO! stateModel=" + currShape.stateModel + "/" + Object.keys(currShape.states[0]).length-1); 
          // TODO: hacky way to get the stateModel to be correct with nested lists 
          // currShape.stateModel %= Object.keys(currShape.states[0]).length-1;  
          // console.log("new stateModel=" + currShape.stateModel); 
          continue;
        }

        let currState = currShape.states[currShape.stateModel]; 
        // sketch.stroke(currState.color); 
        // sketch.fill(currState.color); 

        let adjustedCoords = this.convertRealCoordsToCanvas(sketch, currState.x, currState.y);
        let adjX = adjustedCoords.x;
        let adjY = adjustedCoords.y;
        let size = currState.size * this.canvasScaleFactor; 
        let showGlow = i == this.shapeModel; 
        if (currShape.typeStr.toLowerCase() === "circle") {
          this.drawCircle(sketch, adjX, adjY, size, size, currState.color, showGlow); 
        }
        else if (currShape.typeStr.toLowerCase() === "point") {
          this.drawPoint(sketch, adjX, adjY, size/10, size/10, currState.color, showGlow); 
        }
        else if (currShape.typeStr.toLowerCase() === "triangle") {
          this.drawTriangle(sketch, adjX, adjY, size, size, currState.color, showGlow); 
        }
        else if (currShape.typeStr.toLowerCase() === "square") {
          this.drawRect(sketch, adjX, adjY, size, size, currState.color, showGlow); 
        }
      }
    }, 

    mousePressed(sketch) {
      if (!this.pointOnCanvas(sketch, sketch.mouseX, sketch.mouseY)) {
        return; 
      }

      let adjustedCoords = this.convertCanvasCoordsToReal(sketch, sketch.mouseX, sketch.mouseY); 
      let adjMouseX = adjustedCoords.x; 
      let adjMouseY = adjustedCoords.y; 

      // using mouse tool? 
      if (this.currShapeString.toLowerCase() === "mouse") {
        // close enough to existing shape? find closest
        let minDist = Infinity; 
        for (let i = 0; i < this.shapes.length; i++) {	
          let shapeState = this.shapes[i].states[this.shapes[i].stateModel]; 
          let adjustedShapeCoords = this.convertRealCoordsToCanvas(sketch, shapeState.x, shapeState.y);
          let d = sketch.dist(sketch.mouseX, sketch.mouseY, adjustedShapeCoords.x, adjustedShapeCoords.y); 
          if (d < this.distToToggleMenu ) {
            this.selectedMobject = this.shapes[i]; 
            this.shapeModel = i; 
            minDist = d; 
          }
        }
        // not close enough, turn off menu
        if (minDist > this.distToToggleMenu) {
          this.selectedMobject = null; 
          this.shapeModel = null; 
        }
        else {
          this.selectedMobject.dragging = true; 
          let adjustedShapeCoords = this.convertRealCoordsToCanvas(sketch, 
                                                                    this.selectedMobject.states[this.selectedMobject.stateModel].x,
                                                                    this.selectedMobject.states[this.selectedMobject.stateModel].y);
          this.selectedMobject.offsetX = adjustedShapeCoords.x - sketch.mouseX; 
          this.selectedMobject.offsetY = adjustedShapeCoords.y - sketch.mouseY; 
        }
        return; 
      }
      
      // new shape
      this.addShape(  { 
                        typeStr: this.currShapeString, 
                        states: [
                          { 
                            x: sketch.int(adjMouseX), 
                            y: sketch.int(adjMouseY), 
                            rot: 0, 
                            color: "blue",
                            size: 1.0,
                            time: 0,
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
      this.shapeModel = this.shapes.length-1; 

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
      newState["time"] += 1; 
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
    },

    drawCircle(sketch, x, y, w, h, color, showGlow) {
      if (showGlow) {
        sketch.strokeWeight(10); 
        sketch.stroke(255,255,255,100);
        sketch.ellipse(x, y, w, h); 
      }
      sketch.stroke(color);
      sketch.strokeWeight(5);
      sketch.ellipse(x, y, w, h); 
    }, 

    drawTriangle(sketch, x, y, w, h, color, showGlow) {
      if (showGlow) {
        sketch.strokeWeight(10); 
        sketch.stroke(255,255,255,100);
        sketch.triangle(x-w/2, y+h/2, x+w/2, y+h/2, x, y-h/2); 
      }
      sketch.stroke(color);
      sketch.strokeWeight(5);
      sketch.triangle(x-w/2, y+h/2, x+w/2, y+h/2, x, y-h/2); 
    }, 

    drawPoint(sketch, x, y, w, h, color, showGlow) {
      if (showGlow) {
        sketch.strokeWeight(10); 
        sketch.stroke(255,255,255,100);
        sketch.ellipse(x, y, w, h); 
      }
      sketch.stroke(color);
      sketch.strokeWeight(5);
      sketch.ellipse(x, y, w, h); 
    }, 

    drawRect(sketch, x, y, w, h, color, showGlow) {
      if (showGlow) {
        sketch.strokeWeight(10); 
        sketch.stroke(255,255,255,100);
        sketch.rect(x, y, w, h); 
      }
      sketch.stroke(color);
      sketch.strokeWeight(5);
      sketch.rect(x, y, w, h); 
    }, 

    convertCanvasCoordsToReal(sketch, x, y) {
      return {x: (x-sketch.width/2+20)/this.canvasScaleFactor, y: (-(y-sketch.height/2+20))/this.canvasScaleFactor};
    },

    convertRealCoordsToCanvas(sketch, x, y) {
      return {x: (x*this.canvasScaleFactor+sketch.width/2-20), y: (-y*this.canvasScaleFactor+sketch.height/2-20)};
    }
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
.state-list-stack-long {
  max-height: 40;
  overflow: auto;
}
.state-list-stack-short {
  max-height: 25vh;
  overflow: auto;
}
</style>
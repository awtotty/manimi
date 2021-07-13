<template>
  <!-- <iframe
      id="p5-canvas"
      src="https://openprocessing.org/sketch/1227678/embed/?plusEmbedHash=MWQ5YzJiNzIwNThhYTkwY2NjZWQxMWQyM2Q1NWRhOWU4YWY0NjRkZThhMGQyYTk4MzIwOTlkYTRmZDZhZWNhNjhhMzlhY2RkOTA4N2IxZjRiYmY4ZWE3MTNkZDI3NDI3MTM2YzU0YWRiYTNiNDY5ZjY3ODgyOGY4NzU5ZGYxOGQ1bDJSRklITjJVOGFmMFpyZllrMmM0YXpXbXpKNGJEakFrOTZUTUhaOXZYVjlmNnhkY2pTUFVNVy96NXFLUXh0L3ljaFFWandCUlVTN21kVEhNN0dmZz09&plusEmbedTitle=true"
  ></iframe> -->

  <vue-p5
    id="p5-ctx"
    @setup="setup"
    @draw="draw"
    @keypressed="keyPressed"
    @mousepressed="mousePressed"
  >
  </vue-p5>
</template>



<script>
  import VueP5 from "vue-p5";

  export default ({
    name: "Scene",

    components: {
      "vue-p5": VueP5
    },

    data: () => ({
      color: [0, 200, 0], 
      currShapeString: null,  
      shapes: [],
      distToToggleMenu: 20, 
      menuW: 100, 
      menuH: 200,
    }),

    methods: {
      setup(sketch) {
        sketch.createCanvas(sketch.windowWidth, sketch.windowHeight); 
        sketch.rectMode(sketch.CENTER);
	
	      this.currShapeString = "circle"; 
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

          // draw shape
          if (currShape.typeStr === "circle") {
            this.drawCircle(sketch, currShape.x, currShape.y, 50, 50); 
          }
          else if (currShape.typeStr === "point") {
            this.drawPoint(sketch, currShape.x, currShape.y, 5, 5); 
          }
          else if (currShape.typeStr === "triangle") {
            this.drawTriangle(sketch, currShape.x, currShape.y, 50, 50); 
          }
          else if (currShape.typeStr === "rect") {
            this.drawRect(sketch, currShape.x, currShape.y, 50, 50); 
          }

          if (currShape.menuOn) {
            this.showMenu(sketch, currShape.x, currShape.y, "TODO: fill menu"); 
          }
        }
      }, 

      showMenu(sketch, x, y, contents) {
        sketch.fill("grey"); 
        sketch.noStroke(); 
        sketch.rect(x+this.menuW/2, y+this.menuH/2, this.menuW, this.menuH); 
        sketch.fill("black"); 
        sketch.noStroke(); 
        sketch.text(contents, x+8, y+15);  
      }, 

      mousePressed(sketch) {
        if (!this.pointOnCanvas(sketch, sketch.mouseX, sketch.mouseY)) {
          return; 
        }
        else {
        }

        // close enough to existing shape? 
        for (let i = 0; i < this.shapes.length; i++) {	
          if (sketch.dist(sketch.mouseX, sketch.mouseY, this.shapes[i].x, this.shapes[i].y) < this.distToToggleMenu) {
            this.shapes[i].menuOn = !this.shapes[i].menuOn
            return; 
          }
        }
        
        // turn off all of the old menus
        for (let i = 0; i < this.shapes.length; i++) {	
          this.shapes[i].menuOn = false;
        }
        
        // new shape
        this.shapes.push( { 
                            typeStr: this.currShapeString, 
                            x: sketch.int(sketch.mouseX), 
                            y: sketch.int(sketch.mouseY), 
                            menuOn: true
                          } 
                        );
      }, 

      keyPressed(sketch) {
        if (sketch.key.toLowerCase() === "c")
          this.currShapeString = "circle"; 
        if (sketch.key.toLowerCase() === "r")
          this.currShapeString = "rect"; 
        if (sketch.key.toLowerCase() === "p")
          this.currShapeString = "point"; 
        if (sketch.key.toLowerCase() === "t")
          this.currShapeString = "triangle"; 

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

    render(h) {
      return h(VueP5, {on: this});
    }
  })
</script>


<style lang="scss" scoped>
#p5-ctx {
  width: 105vw;
  height: 105vh;
  margin-left: -10px;
  margin-top: -12px;
  overflow: hidden;
}
</style>
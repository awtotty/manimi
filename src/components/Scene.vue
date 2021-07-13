<template>
  <!-- <iframe
      id="p5-canvas"
      src="https://openprocessing.org/sketch/1227678/embed/?plusEmbedHash=MWQ5YzJiNzIwNThhYTkwY2NjZWQxMWQyM2Q1NWRhOWU4YWY0NjRkZThhMGQyYTk4MzIwOTlkYTRmZDZhZWNhNjhhMzlhY2RkOTA4N2IxZjRiYmY4ZWE3MTNkZDI3NDI3MTM2YzU0YWRiYTNiNDY5ZjY3ODgyOGY4NzU5ZGYxOGQ1bDJSRklITjJVOGFmMFpyZllrMmM0YXpXbXpKNGJEakFrOTZUTUhaOXZYVjlmNnhkY2pTUFVNVy96NXFLUXh0L3ljaFFWandCUlVTN21kVEhNN0dmZz09&plusEmbedTitle=true"
  ></iframe> -->

  <vue-p5
        @preload="preload"
        @setup="setup"
        @draw="draw"
        @keypressed="keyPressed"
        @mousepressed="mousePressed"
        @mousemoved="mouseMoved"
        @mousedragged="mouseDragged">
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
      currShape: null, 
      currShapeString: null,  
      w: null, 
      h: null,  
      shapes: [],
      distToToggleMenu: 20, 
      menuW: 100, 
      menuH: 200,
    }),

    methods: {
      setup(sketch) {
        sketch.createCanvas(400, 400); 
        sketch.rectMode(CENTER);
	
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
        // sketch.background(...this.color);
        // sketch.text('Hello p5!', 20, 20);
        sketch.background("black"); 
	
        for (let i = 0; i < this.shapes.length; i++) {		
          this.setShape(this.shapes[i].str); 
          sketch.noFill(); 
          sketch.strokeWeight(5);
          sketch.stroke("blue"); 
          this.currShape(shapes[i].loc[0], shapes[i].loc[1], w, h); 
          if (this.shapes[i].menuOn) {
            this.showMenu(this.shapes[i].loc[0], this.shapes[i].loc[1],
                    "Pos: (" + this.shapes[i].loc[0] + ", " + this.shapes[i].loc[1] + ")"); 
          }
        }
      }, 

      showMenu(sketch, x, y, contents) {
        sketch.fill("grey"); 
        sketch.noStroke(); 
        sketch.rect(x+this.menuW/2, y+this.menuH/2, this.menuW, this.menuH); 
        sketch.fill("black"); 
        sketch.noStroke(); 
        sketch.text(contents, this.x+8, this.y+15);  
      }, 

     setShape(typeString) {
        if (typeString === "point") {
          currShape = ellipse;
          this.w = 5; 
          this.h = 5;
        }
        else {
          this.w = 50; 
          this.h = 50; 

          if (typeString === "circle") {
            this.currShape = drawCircle;
          }
          if (typeString === "rect") {
            this.currShape = rect;
          }
          if (typeString === "triangle") {
            this.currShape = drawTriangle;
          }

        }
      }, 

      mousePressed(sketch) {
        if (!this.pointOnCanvas(sketch.mouseX, sketch.mouseY)) {
          return; 
        }
        // close enough to existing shape? 
        for (let i = 0; i < this.shapes.length; i++) {	
          if (dist(sketch.mouseX, sketch.mouseY, this.shapes[i].loc[0], this.shapes[i].loc[1]) < this.distToToggleMenu) {
            this.shapes[i].menuOn = !this.shapes[i].menuOn
            return; 
          }
        }
        
        // turn off all of the old menus
        for (let i = 0; i < shapes.length; i++) {	
          this.shapes[i].menuOn = false;
        }
        
        // new shape
        this.shapes.push( {str: this.currShapeString, loc: [int(sketch.mouseX), int(sketch.mouseY)], menuOn: true} );
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
      },

      drawCircle(sketch, x, y, w, h) {
        sketch.ellipse(x, y, w, h); 
      }, 

      drawTriangle(sketch, x, y, w, h) {
        sketch.triangle(x-w/2, y+h/2, x+w/2, y+h/2, x, y-h/2); 
      }, 

    },

    render(h) {
      return h(VueP5, {on: this});
    }
  })
</script>


<style lang="scss" scoped>
#p5-canvas {
  width: 105vw;
  height: 105vh;
  margin-left: -50px;
  margin-top: -100px;
  overflow: hidden;
}
</style>
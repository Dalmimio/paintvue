<template>
    <div id="app">
      <canvas
        ref="myCanvas"
        id="myCanvas"
        @mousemove="draw"
        @mousedown="beginDrawing"
        @mouseup="stopDrawing"
        @mouseout="stopDrawing"
      ></canvas>
      <DrawingToolbar 
      @color-selection="setDrawingColor" @style-selection="setDrawingStyle"/>
    </div>
  </template>
  
  <script>
    import DrawingToolbar from './DrawToolBar.vue'
  
    export default {
      name: 'App',
      components: {
        DrawingToolbar,
      },
      data: function () {
        return {
          canvas: null,
          x: 0,
          y: 0,
          isDrawing: false,
          color: 'rgb(0,0,0)',
          drawStyle: 'freehand',
        }
      },
      methods: {
        draw(e) {
          if (this.isDrawing) {
            if (this.drawStyle === 'freehand') {
              this.drawLine(this.x, this.y, e.offsetX, e.offsetY)
              this.x = e.offsetX
              this.y = e.offsetY
            }
          }
        },
        beginDrawing(e) {
          this.x = e.offsetX
          this.y = e.offsetY
          this.isDrawing = true
        },
        stopDrawing(e) {
          if (this.isDrawing) {
            if (this.drawStyle === 'freehand') {
              this.drawLine(this.x, this.y, e.offsetX, e.offsetY)
              this.x = 0
              this.y = 0
              this.isDrawing = false
            } else if (this.drawStyle === 'rectangle') {
              this.drawRect(this.x, this.y, e.offsetX, e.offsetY)
              this.isDrawing = false
            }
          }
        },
        drawLine(x1, y1, x2, y2) {
          let ctx = this.canvas
          ctx.beginPath()
          ctx.strokeStyle = this.color
          ctx.moveTo(x1, y1)
          ctx.lineTo(x2, y2)
          ctx.stroke()
          ctx.closePath()
          console.log(this.canvas);
        },
        drawRect(x1, y1, x2, y2) {
          let ctx = this.canvas
          ctx.beginPath()
          ctx.strokeStyle = this.color
          ctx.strokeRect(x1, y1, x2 - x1, y2 - y1);
        },
        setDrawingColor(newColor) {
          this.color = newColor
        },
        setDrawingStyle(styleType) {
          this.drawStyle = styleType
        },
        saveImg (){
          // let ctx = this.canvas
          // ctx.clearRect(0, 0, context.canvas.width, context.canvas.height)
        }
      },
      mounted() {
        let canvas = this.$refs['myCanvas']
        canvas.width = window.innerWidth * 0.6
        canvas.height = window.innerHeight * 0.6
        this.canvas = canvas.getContext('2d')
        console.log(this.canvas)
      },
    }
  </script>
  
  <style>
    * {
      margin: 0px;
      padding: 0px;
      box-sizing: border-box;
    }
    #app {
      display: flex;
      height: 100vh;
      width: 100vw;
    }
    #myCanvas {
      border: 1px solid grey;
      background-color: #FFF;
      margin: 5px 0px 5px 5px;
    }
  </style>
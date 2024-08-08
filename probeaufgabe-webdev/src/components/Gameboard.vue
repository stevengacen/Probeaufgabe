<template>
    <div class="canvas-container">
        <Transition name="slide-fade">
            <InfoSidebar 
                v-if="selectedSquare"
                :square="selectedSquare"
                @update-square="updateSquare"
            />
        </Transition>
        <canvas 
            ref="canvas" 
            class="canvas" 
            @mousedown="startDrag" 
            @mousemove="handleMouseMove" 
            @mouseup="endDrag" 
            @mouseleave="endDrag" 
            @wheel="zoom" 
            @click="handleCanvasClick">
        </canvas>
    </div>
</template>
  
<script>
    import gridData from '@/assets/playingfield_example.json'; // Import the JSON file
    import InfoSidebar from '@/components/InfoSidebar.vue';
  
    export default {
        name: 'Gameboard',
        components: {
            InfoSidebar,
        },
    
        data() {
            return {
                isDragging: false,
                startX: 0,
                startY: 0,
                offsetX: 0,
                offsetY: 0,
                scale: 0.9,
                minScale: 0.5,
                maxScale: 5,
                grid: [],
                textures: {}, // Store textures based on color
                squareMargin: 20,
                selectedSquare: null, 
                isMiddleMousePressed: false
            };
        },
    
        mounted() {
            this.loadTextures(); // Load the texture first
            window.addEventListener('resize', this.onWindowResize);
        },
        
        methods: {

            onWindowResize() {
                const canvas = this.$refs.canvas;
                if (canvas) {
                    canvas.width = window.innerWidth;
                    canvas.height = window.innerHeight;
                    if (window.innerWidth <= 768) {
                        this.scale = 0.4;
                        this.centerGrid(canvas, this.scale);
                    } else if (window.innerWidth <= 1024) {
                        this.scale = 0.6;
                        this.centerGrid(canvas, this.scale);
                    } else {
                        this.scale = 0.9;
                        this.centerGrid(canvas, this.scale);
                    }
                    this.redrawCanvas();
                }
            },

            loadTextures() {
                const colors = ['red', 'green', 'blue', 'grey', 'black', 'pink'];
                let loadedTextures = 0;

                colors.forEach(color => {
                    const img = new Image();
                    img.src = require(`@/assets/colors/${color}.svg`);
                    img.onload = () => {
                        this.textures[color] = img;
                        loadedTextures++;
                        if (loadedTextures === colors.length) {
                            this.initializeCanvas(); // Initialize the canvas after all textures are loaded
                            this.initializeGrid(); // Initialize the grid after all textures are loaded
                        }
                    };
                });
            },

            centerGrid(canvas, scale) {
                // Center the grid
                const gridSize = 6;
                const squareSize = 127;
                const totalGridSize = gridSize * squareSize + (gridSize - 1) * this.squareMargin;
                this.offsetX = (canvas.width - totalGridSize * scale) / 2;
                this.offsetY = (canvas.height - totalGridSize * scale) / 2;
            },

            centerViewOnSquare(square) {
                const { x, y } = square.coordinates;
                const squareSize = 127;
                const canvas = this.$refs.canvas;

                // Calculate the new offsets to center the square
                const newOffsetX = canvas.width / 2 - (x - 1) * (squareSize + this.squareMargin) * this.scale - squareSize * this.scale / 2;
                const newOffsetY = canvas.height / 2 - (5 - (y - 1)) * (squareSize + this.squareMargin) * this.scale - squareSize * this.scale / 2;

                this.offsetX = newOffsetX;
                this.offsetY = newOffsetY;

                this.redrawCanvas();
            },

            initializeCanvas() {
                this.$nextTick(() => {
                    const canvas = this.$refs.canvas;
                    if (canvas) {
                        if (window.innerWidth <= 768) {
                            this.scale = 0.4;
                        } else if (window.innerWidth <= 1024) {
                            this.scale = 0.6;
                        } else {
                            this.scale = 0.9;
                        }
                        canvas.width = window.innerWidth;
                        canvas.height = window.innerHeight;
                        this.centerGrid(canvas, this.scale);
                        this.redrawCanvas();

                        window.addEventListener('resize', () => {
                            if (canvas) {
                                canvas.width = window.innerWidth;
                                canvas.height = window.innerHeight;
                                this.redrawCanvas();
                            }
                        });
                    }
                });     
            },

            initializeGrid() {
                const squareSize = 127;
                const playingfield = gridData.playingfield;

                // Adjust grid initialization to use the JSON data
                for (const key in playingfield) {
                    if (playingfield.hasOwnProperty(key)) {
                        const square = playingfield[key];
                        const { x, y } = square.coordinates;
                        this.grid.push({
                            coordinates: { x, y },
                            x: (x - 1) * (squareSize + this.squareMargin), // Convert to canvas coordinates
                            y: (5 - (y - 1)) * (squareSize + this.squareMargin), // Convert to canvas coordinates
                            size: squareSize,
                            color: square.color,
                            rotation: square.rotation
                        });
                    }
                }
                this.redrawCanvas();
            },

            redrawCanvas() {
                const canvas = this.$refs.canvas;
                if (canvas) {
                    const context = canvas.getContext('2d');

                    // Clear the entire canvas before redrawing
                    context.save();
                    context.setTransform(1, 0, 0, 1, 0, 0); // Reset transform to default
                    context.clearRect(0, 0, canvas.width, canvas.height); // Clear full canvas
                    context.restore();

                    // Apply transformations for scaling and panning
                    context.setTransform(this.scale, 0, 0, this.scale, this.offsetX, this.offsetY);

                    // Draw each square in the grid
                    this.grid.forEach(square => {
                        this.drawSquare(context, square);
                    });

                    // Highlight the selected square if any
                    if (this.selectedSquare) {
                        this.highlightSquare(context, this.selectedSquare);
                    }
                }
            },

            drawSquare(context, square) {
                const { x, y, size, rotation, color } = square;
                context.save();
                context.translate(x + size / 2, y + size / 2);
                context.rotate((rotation * (-Math.PI)) / 180);
                context.translate(-size / 2, -size / 2);

                const texture = this.textures[color];
                if (texture) {
                    context.drawImage(texture, 0, 0, texture.width, texture.height, 0, 0, size, size);
                } else {
                    context.fillStyle = color; // Fallback color
                    context.fillRect(0, 0, size, size);
                }
                context.restore();
            },

            highlightSquare(context, square) {
                const { x, y, size } = square;
                context.save();
                context.translate(x, y);
                context.strokeStyle = 'black';
                context.lineWidth = 7;

                const radius = 15;
                context.beginPath();
                context.moveTo(radius, 0);
                context.lineTo(size - radius, 0);
                context.quadraticCurveTo(size, 0, size, radius);
                context.lineTo(size, size - radius);
                context.quadraticCurveTo(size, size, size - radius, size);
                context.lineTo(radius, size);
                context.quadraticCurveTo(0, size, 0, size - radius);
                context.lineTo(0, radius);
                context.quadraticCurveTo(0, 0, radius, 0);
                context.closePath();

                context.stroke();
                context.restore();
            },

            startDrag(event) {
                if (event.button === 1) {
                    this.isDragging = true;
                    this.startX = event.clientX;
                    this.startY = event.clientY;
                    this.isMiddleMousePressed = true;
                }    
            },
      
            endDrag(event) {
                if (event.button === 1) {
                    this.isDragging = false;
                    this.isMiddleMousePressed = false;
                }
                
            },
      
            zoom(event) {
                event.preventDefault();
                const scaleChange = event.deltaY * -0.0015;
                const newScale = Math.min(Math.max(this.scale + scaleChange, this.minScale), this.maxScale);
        
                // Calculate the mouse position relative to the canvas
                const canvas = this.$refs.canvas;
                const rect = canvas.getBoundingClientRect();
                const mouseX = event.clientX;
                const mouseY = event.clientY - rect.top;
        
                // Calculate the offset change needed to keep the zoom centered on the cursor
                this.offsetX -= (mouseX - this.offsetX) * (newScale / this.scale - 1);
                this.offsetY -= (mouseY - this.offsetY) * (newScale / this.scale - 1);
        
                this.scale = newScale;
                this.redrawCanvas();
            },

            handleCanvasClick(event) {
                if (event.button === 0) {
                    const canvas = this.$refs.canvas;
                    const rect = canvas.getBoundingClientRect();
                    const x = ((event.clientX - rect.left - this.offsetX + 40 ) / this.scale)*1.04;
                    const y = (event.clientY - rect.top - this.offsetY) / this.scale;

                    this.selectedSquare = null;

                    this.grid.forEach(square => {
                        if (
                            x >= square.x &&
                            x <= square.x + square.size &&
                            y >= square.y &&
                            y <= square.y + square.size
                        ) {
                            this.selectedSquare = square;
                            //this.centerViewOnSquare(square);
                            console.log(`Square clicked: x=${square.x}, y=${square.y}, color=${square.color}, rotation=${square.rotation}`);
                        }
                        
                    });
                this.redrawCanvas();
                }
            },

            handleMouseMove(event) {
                if (this.isMiddleMousePressed) {
                    const canvas = this.$refs.canvas;
                    canvas.style.cursor = 'grab';
                }
                
                if (this.isDragging && this.isMiddleMousePressed) {
                    const dx = event.clientX - this.startX;
                    const dy = event.clientY - this.startY;
                    this.offsetX += dx;
                    this.offsetY += dy;
                    this.startX = event.clientX;
                    this.startY = event.clientY;
                    this.redrawCanvas();
                } else {
                    const canvas = this.$refs.canvas;
                    const rect = canvas.getBoundingClientRect();
                    const x = ((event.clientX - rect.left - this.offsetX + 40 ) / this.scale)*1.04;
                    const y = (event.clientY - rect.top - this.offsetY) / this.scale;

                    const isHovering = this.grid.some(square => {
                        const squareX = square.x;
                        const squareY = square.y;
                        return x >= squareX && x <= squareX + square.size && y >= squareY && y <= squareY + square.size;
                    });
                    canvas.style.cursor = isHovering ? 'pointer' : 'default';
                }
            },

            updateSquare(updatedSquare) {
                const index = this.grid.findIndex(square => 
                square.coordinates.x === updatedSquare.coordinates.x && 
                square.coordinates.y === updatedSquare.coordinates.y
                );
                if (index !== -1) {
                    this.grid.splice(index, 1, updatedSquare);
                    this.redrawCanvas();
                }
            },
                 
        },
    beforeDestroy() {
        window.removeEventListener('resize', this.onWindowResize);
    }  
}
</script>

  
<style>
    .canvas-container {
        position: relative;
        width: 100%;
        height: 100%;
    }
  
    .canvas {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }

    .slide-fade-enter-active {
        transition: all 0.4s ease-out;
    }

    .slide-fade-leave-active {
        transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
    }

    .slide-fade-enter-from,
    .slide-fade-leave-to {
        transform: translateX(20px);
        opacity: 0;
    }

      /* Mobile Styles */
@media (max-width: 768px) {
    .canvas-container {
        width: 100%;
        height: calc(100vh - 150px); /* Adjust based on navbar and footer heights */
    }

}
</style>
  
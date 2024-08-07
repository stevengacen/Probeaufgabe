<template>
    <div class="infoside" v-if="square">
        <div class="infobox">
            <p>Koordinate</p>
            <div class="seperator"></div>
            <div class="coordsinfo">
                <p>X: {{ square.coordinates.x }}</p>
                <p>Y: {{ square.coordinates.y }}</p>
            </div> 
        </div>
        <div class="infobox">
            <p>Farbe</p>
            <div class="seperator"></div>
            <div class="colorinfo"> 
                <div id="colorpreshow"></div>
                <p>{{ translateColor(square.color) }}</p>
            </div>
            <button class="sideButton" @click="changeColor">Farbe ändern</button>
        </div>
        <div class="infobox">
            <p>Rotation</p> 
            <div class="seperator"></div>
            <div class="inforow"> 
                <p>{{ square.rotation }}°</p>
            </div>
            <button class="sideButton" @click="rotateSquare">um 90° drehen</button>
        </div>
    </div>
</template>
  
<script>
  export default {
    name: 'infoside',
    props: {
      square: {
        type: Object,
        default: null
      }
    },
    methods: {
        changeColor() {
            const colors = ['red', 'green', 'blue', 'grey', 'black', 'pink'];
            const currentIndex = colors.indexOf(this.square.color);
            this.square.color = colors[(currentIndex + 1) % colors.length];
            this.$emit('update-square', this.square);
        },
        rotateSquare() {
            this.square.rotation = (this.square.rotation + 90) % 360;
            this.$emit('update-square', this.square);
        },
        translateColor(color) {
            const colorTranslations = {
                red: 'Rot',
                green: 'Grün',
                blue: 'Blau',
                grey: 'Grau',
                black: 'Schwarz',
                pink: 'Pink'
            };
            return colorTranslations[color] || color;
        },
        shiftColor(color) {
            const shiftedColor = {
                red: '#AE3F3F',
                green: '#95C546',
                blue: '#0070C0',
                grey: '#616367',
                black: '#1A1A1A',
                pink: '#DB57D5'
            };
            return shiftedColor[color] || color;
        }
    }
}
</script>
  

<style scoped>
    .content {
        width: 10px;
    }

    .infoside {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        position: absolute;
        top: 0;
        right: 0;
        width: 18%;
        height: 100%;
        background-color: #f9f9f9;
        border-left: 1px solid #ddd;
        box-shadow: -2px 0 5px rgba(0, 0, 0, 0.1);
        z-index: 1000;
        color: #55555A;
        font-size: 16px;
    }

    .infobox {
        display: flex;
        flex-direction: column;
        align-items: center;
        background-color: #d5d5d756;
        margin-top: 10%;
        margin-bottom: 10%;
        width: 65%;
        height: 16%;
        border-radius: 10px;
    }

    .infobox p{
        font-weight: 600;
    }

    .infobox div p{
        font-weight: 400;
    }

    .coordsinfo {
        width: 80%;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
        align-items: center;
    }

    .colorinfo {
        width: 60%;
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
    }

    #colorpreshow {
        height: 25px;
        width: 25px;
        margin-right: 5px;
        background-color: v-bind('shiftColor(square.color)');
        border-radius: 4px;
    }

    .sideButton {
        width: 80%;
        height: 22%;
        font-family: "Roboto" "sans serif";
        font-style: normal;
        font-weight: 500;
        background: transparent;
        border-style: solid;
        border-radius: 5px;
        border-color: #55555a67;
        border-width: 1px;
        cursor: pointer;
        transition: 0.3s all;
    }

    .sideButton:hover {
        background: #d4d4d479;
        transition: 0.3s all;
    }

    .sideButton:active {
        background: #d4d4d4d0;
        transition: 0.1s all;
    }

    .seperator {
        height: 2px;
        width: 80%;
        margin-top: -5px;
        background-color: #55555a67;
    }
</style>
  
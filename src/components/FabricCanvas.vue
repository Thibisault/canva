<!-- FabricCanvas.vue -->
<template>
            <button @click="addText">Ajouter Texte</button>
        <button @click="addIcon">Ajouter Icône</button>
        <input type="file" @change="onBackgroundImageChange" />

    <div class="canvas-container" ref="canvasContainer">

        <canvas ref="canvas"></canvas>
    </div>
  </template>
  
  <script>
  import { fabric } from 'fabric';
  
  export default {
    mounted() {
      this.initCanvas();
    },
    methods: {
        initCanvas() {
      const canvas = new fabric.Canvas(this.$refs.canvas);
      canvas.setHeight(750);
      canvas.setWidth(1050);
      this.canvas = canvas;
    },

    addText() {
    const text = new fabric.IText('Texte ici', {
      left: 50, // Position initiale X
      top: 50, // Position initiale Y
      fontFamily: 'Arial',
      fontSize: 20,
    });
    this.canvas.add(text);
    },

    addIconZone() {
    const zone = new fabric.Rect({
        left: 100,
        top: 100,
        fill: 'transparent',
        width: 200,
        height: 200,
        stroke: 'black',
        strokeWidth: 1
    });
    this.canvas.add(zone);

    

    // Exemple pour ajouter une icône
    fabric.Image.fromURL('icon-url.jpg', (icon) => {
        icon.scaleToWidth(50); // Ajustez la taille comme nécessaire
        icon.set({
        left: zone.left + 10,
        top: zone.top + 10
        });
        this.canvas.add(icon);
    });
    },

    repositionIcons() {
  // Supposons que zoneWidth et zoneHeight définissent la taille de votre zone
    let zoneWidth = 200;
    let zoneHeight = 200;
    let iconsPerRow = 4; // ou calculez dynamiquement en fonction de la largeur de la zone et de la largeur de l'icône
    let iconWidth = zoneWidth / iconsPerRow;
    let iconHeight = iconWidth; // Garder les icônes carrées ou ajuster selon le besoin

    this.icons.forEach((icon, index) => {
        let row = Math.floor(index / iconsPerRow);
        let col = index % iconsPerRow;
        
        icon.set({
        left: 100 + col * iconWidth, // 100 étant la position X de départ de la zone
        top: 100 + row * iconHeight, // 100 étant la position Y de départ de la zone
        scaleX: iconWidth / icon.width,
        scaleY: iconHeight / icon.height
        });
        this.canvas.add(icon);
    });

    this.canvas.renderAll();
    },

    setBackgroundImage(imageURL) {
    fabric.Image.fromURL(imageURL, (img) => {
        this.canvas.setBackgroundImage(img, this.canvas.renderAll.bind(this.canvas), {
        scaleX: this.canvas.width / img.width,
        scaleY: this.canvas.height / img.height
        });
    });
    },
    onBackgroundImageChange(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => this.setBackgroundImage(e.target.result);
        reader.readAsDataURL(file);
      }
    },
    }
  };
  </script>

<style scoped>
.canvas-container {
  border: 1px solid black; /* Applique une bordure noire autour du conteneur du canvas */
  position: relative;
  width: 1050px; /* Largeur du conteneur correspondant au canvas */
  height: 750px; /* Hauteur du conteneur correspondant au canvas */
  display: flex;
  justify-content: center;
  align-items: center;
}
canvas {
  position: absolute;
  top: 0;
  left: 0;
}
</style>
  
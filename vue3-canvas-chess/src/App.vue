<script setup>
import { onMounted } from "vue";
const lineX = 15;
const lineY = 15;

onMounted(() => {
  const chess = document.getElementById("chess");
  const width = chess.offsetWidth;
  const height = chess.offsetHeight;
  const ctx = chess.getContext("2d");
  const ratio = getPixelRatio(ctx);
  ctx.strokeStyle = "#999";
  drawImg(ctx).then(() => {
    drawChessLine(ctx, width, height, ratio);
    let clickTime = 0;

    chess.onclick = (e) => {
      clickTime++;
      const x = e.offsetX;
      const y = e.offsetY;
      const i = Math.floor(x / lineX / ratio);
      const j = Math.floor(y / lineY / ratio);

      drawChessItem(ctx, i, j, ratio, clickTime);
    };
  });
});

const getPixelRatio = (context) => {
  var backingStore =
    context.backingStorePixelRatio ||
    context.webkitBackingStorePixelRatio ||
    context.mozBackingStorePixelRatio ||
    context.msBackingStorePixelRatio ||
    context.oBackingStorePixelRatio ||
    context.backingStorePixelRatio ||
    1;
  return (window.devicePixelRatio || 1) / backingStore;
};

const drawImg = (ctx) => {
  return new Promise((resolve) => {
    let img = new Image();
    img.src = "https://uni-h5.colorui.org/static/logo.png";
    img.onload = () => {
      ctx.drawImage(img, 0, 0, 450, 450);
      resolve();
    };
  });
};

const drawChessItem = (ctx, i, j, ratio, clickTime) => {
  ctx.beginPath();
  ctx.arc(15 + lineY * i * ratio, 15 + lineX * j * ratio, 13, 0, 2 * Math.PI);
  ctx.closePath();
  ctx.fillStyle = clickTime % 2 === 0 ? "#000" : "#fff";
  ctx.fill();
  ctx.stroke();
};

const drawChessLine = (ctx, width, height, ratio) => {
  for (let i = 0; i < lineX; i++) {
    ctx.moveTo(15 + lineY * i * ratio, 15);
    ctx.lineTo(15 + lineY * i * ratio, height - 15);
    ctx.stroke();
  }
  for (let i = 0; i < lineY; i++) {
    ctx.moveTo(15, 15 + lineX * i * ratio);
    ctx.lineTo(width - 15, 15 + lineX * i * ratio);
    ctx.stroke();
  }
};
</script>

<template>
  <canvas id="chess" width="450" height="450"></canvas>
</template>

<style>
canvas {
  display: block;
  box-shadow: 0 0 20px 5px rgba(0, 0, 0, 0.3);
  margin: 20px auto;
}

.img {
  opacity: 0.1;
}
</style>

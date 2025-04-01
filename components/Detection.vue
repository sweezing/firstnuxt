<template>
	<div>
		<video ref="video" class="hidden" width="600" autoplay></video>
		<canvas ref="canvas" width="600" height="400"></canvas>
	</div>
</template>

  <script lang="ts" setup>
  import { ref, onMounted } from 'vue';
  import * as cocoSsd from '@tensorflow-models/coco-ssd';
  import * as tf from '@tensorflow/tfjs';

  if (typeof window !== 'undefined') {
      tf.setBackend('webgl');
  }

  const video = ref<HTMLVideoElement | null>(null);
  const canvas = ref<HTMLCanvasElement | null>(null);
  let model: cocoSsd.ObjectDetection;

  const setupCamera = async () => {
      try {
          const stream = await navigator.mediaDevices.getUserMedia({ video: true });
          if (video.value) {
              video.value.srcObject = stream;
              console.log("Stream is enabled");
          }
      } catch (error) {
          console.error("Error accessing camera: ", error);
      }
  };

  const detectFrame = async () => {
      if (video.value && model) {
          const predictions = await model.detect(video.value);
          renderPredictions(predictions);
          requestAnimationFrame(detectFrame);
      }
  };

  const renderPredictions = (predictions: cocoSsd.DetectedObject[]) => {
      if (canvas.value && video.value) {
          const ctx = canvas.value.getContext('2d');
          if (ctx) {
              ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
              ctx.drawImage(video.value, 0, 0, ctx.canvas.width, ctx.canvas.height);

              predictions.forEach((prediction) => {
                  const [x, y, width, height] = prediction.bbox;
                  ctx.beginPath();
                  ctx.rect(x, y, width, height);
                  ctx.lineWidth = 2;
                  ctx.strokeStyle = 'red';
                  ctx.fillStyle = 'red';
                  ctx.stroke();
                  ctx.fillText(
                      `${prediction.class} (${Math.round(prediction.score * 100)}%)`,
                      x,
                      y > 10 ? y - 5 : 10
                  );
              });
          }
      }
  };

  onMounted(async () => {
      console.log("Initializing...");
      await setupCamera();
      console.log("COCO-SSD model is loading...");
      model = await cocoSsd.load();
      console.log("Model is loaded!");
      detectFrame();
  });
  </script>


  <style scoped>
  video, canvas {
      position: absolute;
      top: 0;
      left: 0;
  }
  canvas {
      pointer-events: none;
  }
  </style>
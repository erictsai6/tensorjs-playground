<style>
.camera video {
  width: 25%;
}
</style>

<script>
import { onMount } from "svelte";
import * as tf from "@tensorflow/tfjs";

let videos = [];
for (let i = 0; i < 8; i++) {
  videos.push({});
}

function setVideoStream(videos, stream) {
  for (let video of videos) {
    video.srcObject = stream;
  }
}

onMount(() => {
  if (navigator.mediaDevices.getUserMedia) {
    navigator.mediaDevices
      .getUserMedia({ video: true })
      .then(function (stream) {
        setVideoStream(videos, stream);
      })
      .catch(function (error) {
        console.log(error);
        window.alert("Something went wrong");
      });
  }
  const shape = [2, 2];
  const b = tf.tensor([1, 2, 3, 4], shape);
  console.log("shape:", b.shape);
  // b.print();

  return () => {
    console.log("Unmounted");
    // clearInterval(interval);
  };
});
</script>

<div class="camera">
  {#each videos as video, i}
    <video bind:this="{videos[i]}" autoplay="true"></video>
  {/each}
</div>

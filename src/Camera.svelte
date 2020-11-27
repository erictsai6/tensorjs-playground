<script>
    import {onMount} from 'svelte';
    
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
            navigator.mediaDevices.getUserMedia({ video: true })
                .then(function (stream) {
                    setVideoStream(videos, stream);
                })
                .catch(function (error) {
                    console.log(error);
                    window.alert('Something went wrong');                
                });
        }
      return () => {
          console.log('Unmounted');
        // clearInterval(interval);
      };
    });
</script>

<style>
    .camera video {
        width: 25%;
    }
</style>

<div class="camera">
    {#each videos as video, i}
        <video bind:this={videos[i]} 
            autoplay="true"></video>    
    {/each}
</div>
  
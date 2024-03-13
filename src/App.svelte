<script lang="ts">
  let height = 3;
  let width = 3;
  $: imageData = Array(height * width).fill(false);

  const clearData = () => {
    imageData = Array(height * width).fill(false);
  }
  let savedImages: Array<{ height: number, width: number, data: boolean[] }> = []
  const saveImage = () => {
    savedImages.push({width, height, data: JSON.parse(JSON.stringify(imageData))});
    savedImages = savedImages;
  }
</script>

<main>

  <div class="card">
    <h1>Drucker</h1>
    <!--savedimages=[{JSON.stringify(savedImages)}]-->
    <div style="display:flex; flex-direction: row; gap: 32px;">
      <div class="printer">
        <h2>Bildinformationen</h2>

        <div style="display:flex; flex-direction: row; gap:8px;">
          <div style="flex: 1 1 auto;">
            <h3>Breite</h3>
            <div class="updownbuttons">
              <button on:click={() => width++} disabled="{width==12}">▲</button>
              {width} Pixel
              <button on:click={() => width--} disabled="{width==3}">▼</button>
            </div>
          </div>
          <div style="flex: 1 1 auto;">
            <h3>Höhe</h3>
            <div class="updownbuttons">
              <button on:click={() => height++} disabled="{height==12}">▲</button>
              {height} Pixel
              <button on:click={() => height--} disabled="{height==3}">▼</button>
            </div>
          </div>
        </div>


        <h2>Bilddaten</h2>
        <div class="bwimage spaced" style="--image-width: {width}; --image-height:{height}; --pixel-size:40px">
          {#each imageData as _, i}
            <div on:click={ () => imageData[i] = !imageData[i] } style="background-color: {imageData[i] ? 'black' : 'white'}"></div>
          {/each}
        </div>
        <div style="display:flex; gap: 10px;">
          <button style="flex: 1 1 auto;" on:click={clearData}>Löschen!</button>
          <button style="flex: 1 1 auto;" on:click={saveImage}>Speichern!</button>
        </div>

      </div>
      <div class="saved-images">
        <h2>Gespeicherte Bilder</h2>
        <!--{#if savedImages.length == 0}-->
        <!--    Keine Bilder bisher gespeichert-->
        <!--{:else}-->
        <div style="height: 480px; overflow-y:auto;">
          {#each savedImages as image, i}
            <h3>Bild {i + 1}</h3>
            <div class="bwimage" style="--image-width: {image.width}; --image-height:{image.height}; --pixel-size:20px">
              {#each image.data as pixel, j}
                <div style="background-color: {pixel == true ? 'black' : 'white'}; width: 20px; height:20px; border: 1px solid #c5c5c5;"></div>
              {/each}
            </div>
          {/each}
        </div>

      </div>
    </div>

  </div>
</main>

<style>

  h2{margin-top: 16px !important;}

  h1 {
    margin: 10px 0;
  }

  .bwimage {
    display: grid;
    grid-template-columns: repeat(var(--image-width, 8), var(--pixel-size, 40px));
    grid-template-rows: repeat(var(--image-height, 8), var(--pixel-size, 40px));
  }

  .bwimage.spaced {
    min-height: calc(var(--pixel-size, 40px) * 12);
    min-width: calc(var(--pixel-size, 40px) * 12);
  }

  .bwimage div {
    /*width:30px;*/
    /*height:20px;*/
    border: calc(var(--pixel-size, 40px) / 20) solid #c5c5c5;
    cursor: pointer;
    user-select: none;
    color: lightgray;
    text-align: end;
    font-size: small;
  }

  .bwimage div:hover {
    border-color: #646cff;
  }

  .updownbuttons {
    display: flex;
    align-items: center;
    flex-direction: column;
  }

  .updownbuttons button {
    flex: 1 1 auto;
    width: 100%;
  }

  .saved-images {

  }
</style>

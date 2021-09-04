<script type="ts">
  export let data = { preview: "", name: "", id: 0, creator: "", urls: {} };
</script>

<div class="weaponDisp">
  <img src={data.preview} alt="Name" />
  <div class="weaponDispFooter">
    <p>
      {data.name}
      {#if data.creator !== undefined} By {data.creator} {/if}
    </p>
    {#each Object.keys(data.urls) as url}
      <a href={data.urls[url]} download={url} target="_blank">Download {url}</a>
      <br />
    {/each}
  </div>
</div>

<style>
  img {
    box-shadow: 0 0 2em #252121 inset;
  }
  .weaponDisp {
    background-color: #333;
    padding: 4em;
    border-radius: 2em;
  }
  .weaponDispFooter {
    background-color: #111;
  }
</style>

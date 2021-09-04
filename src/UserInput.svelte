<script lang="ts">
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  const click = () => {
    dispatch("update", { searchVal });
  };
  let searchVal = "";
</script>

<div class="UserInput">
  <input
    type="text"
    placeholder="Input Search Term"
    bind:value={searchVal}
    on:keyup={click}
  />
  <button on:click={click}>Search</button>
</div>

<style>
  .UserInput {
    background-color: #555;
    padding: 2em;
  }
</style>

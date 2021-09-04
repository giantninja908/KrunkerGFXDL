<script>
</script>

<div class="disp">
  <slot />
</div>

<style>
  .disp {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    display: flex;
    flex-direction: row;
  }
</style>

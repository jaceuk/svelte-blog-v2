<script lang="ts">
  import AnimatedFooterBackground from '@components/AnimatedFooterBackground.svelte';
</script>

<footer>
  <div class="inner">© 2022 Jason Newington</div>
  <AnimatedFooterBackground />
</footer>

<style lang="scss">
  footer {
    position: relative;
    z-index: -1;
    color: var(--color-white);
    text-align: center;
    padding: var(--size-large) 0;
  }

  @keyframes Gradient {
    0% {
      background-position: 0% 50%;
    }
    50% {
      background-position: 100% 50%;
    }
    100% {
      background-position: 0% 50%;
    }
  }
</style>

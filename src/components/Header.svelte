<script lang="ts">
  import AnimatedBackground from '@components/AnimatedBackground.svelte';
</script>

<header>
  <div class="inner">
    <slot class="title" />
    <div class="image">
      <AnimatedBackground />
    </div>
  </div>
</header>
<div class="gradient" />

<style>
  header {
    position: relative;
  }

  .inner {
    display: flex;
    align-items: center;
    gap: 3.2rem;
  }

  .image {
    border-radius: var(--border-radius-super);
    width: 800px;
    height: 800px;
    overflow: hidden;
    transform: rotateY(180deg) rotate(15deg);
    position: absolute;
    bottom: -20px;
    right: -208px;
    z-index: 10;
    outline: 1px solid transparent;
  }

  .gradient {
    background: linear-gradient(180deg, #f2f2f2 0%, rgba(242, 242, 242, 0) 100%);
    height: 122px;
    z-index: 0;
  }
</style>

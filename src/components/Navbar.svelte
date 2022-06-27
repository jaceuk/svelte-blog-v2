<script lang="ts">
  import { page } from '$app/stores';
  import logo from '@images/logo.svg';
</script>

<div class="container">
  <div class="inner">
    <a class="logo-link" href="/">
      <img class="image" src={logo} alt="" />
    </a>

    <nav>
      <a class="link" class:active={$page.url.pathname === '/'} href="/">Home</a>
      <a class="link" class:active={$page.url.pathname === '/blog/posts'} href="/blog/posts">Blog</a>
      <a class="link" class:active={$page.url.pathname === '/contact'} href="/contact">Contact</a>
      <span class="underline" />
    </nav>
  </div>
</div>

<style lang="scss">
  .container {
    padding: var(--size-medium) 0 0 0;
    z-index: 50;
  }

  .inner {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .logo-link {
    z-index: 20;
  }

  .link {
    color: var(--color-white);
    font-size: var(--font-size-body);
    text-decoration: none;
    padding: var(--size-base);
    font-weight: var(--font-weight-bold);
  }

  a:nth-child(1).active ~ .underline {
    transform: translatex(0px);
    transition: transform 0.2s ease-out;
  }

  a:nth-child(2).active ~ .underline {
    transform: translatex(103px);
    transition: transform 0.2s ease-out;
    width: 76px;
  }

  a:nth-child(3).active ~ .underline {
    transform: translatex(195px);
    transition: transform 0.2s ease-out;
    width: 104px;
  }

  nav {
    display: flex;
    gap: var(--size-base);
    align-items: center;
    z-index: 20;
    position: relative;
  }

  .underline {
    position: absolute;
    left: 0;
    bottom: 0;
    width: 87px;
    height: 4px;
    background: var(--color-white);
    border-radius: 28px;
    z-index: -1;
    transition: 9999s transform 0.2s ease-out;
  }
</style>

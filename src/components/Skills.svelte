<script lang="ts">
  import skills from '@data/skills.json';
  import developer from '@data/developer.json';
  import designer from '@data/designer.json';
  import geek from '@data/geek.json';
  import Card from '@components/Card.svelte';
</script>

<div class="inner">
  <Card>
    <h2>{developer.title}</h2>
    <div class="row flex-start">
      <div class="stack">
        {#each developer.text as paragraph}
          <p>{paragraph}</p>
        {/each}
      </div>
      <div class="tags">
        {#each skills.sort() as skill}
          <div class="tag">
            {skill}
          </div>
        {/each}
      </div>
    </div>
  </Card>

  <div class="row">
    <Card>
      <h2>{designer.title}</h2>
      {#each designer.text as paragraph}
        <p>{paragraph}</p>
      {/each}
    </Card>

    <Card>
      <h2>{geek.title}</h2>
      {#each geek.text as paragraph}
        <p>{paragraph}</p>
      {/each}
    </Card>
  </div>
</div>

<style lang="scss">
  .inner {
    padding-bottom: var(--size-extra-large);
    background: url('../images/vader.png') no-repeat right bottom;
    display: flex;
    gap: var(--size-medium);
    flex-direction: column;
  }
</style>

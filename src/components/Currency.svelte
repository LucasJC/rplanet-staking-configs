<script lang="ts">
  export let image: string = "";
  export let alt: string = "";
  export let price: number = 0;
  export let tooltip: string = "";
</script>

<main>
  <div
    class="is-flex is-flex-direction-row is-align-content-center has-tooltip-bottom has-tooltip-info"
    data-tooltip={tooltip}
  >
    <figure class="image is-24x24">
      <img src={image} {alt} />
    </figure>
    <span class="pl-2">{price}</span>
  </div>
</main>

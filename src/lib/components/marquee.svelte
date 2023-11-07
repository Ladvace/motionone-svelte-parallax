<script>
  export let text = "";
  let marqueeText = text.repeat(20);
</script>

<div class="overflow-hidden whitespace-nowrap">
  <div class="marquee-animation text-white font-bold py-4 uppercase">
    {marqueeText}
  </div>
</div>

<style>
  @keyframes marquee {
    from {
      transform: translateX(100%);
    }
    to {
      transform: translateX(-100%);
    }
  }

  .marquee-animation {
    animation: marquee 20s linear infinite;
  }
</style>

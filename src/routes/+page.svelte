<script lang="ts">
  import "../style.css";
  import { onMount } from "svelte";
  import { animate, scroll, timeline } from "motion";
  import sky from "$lib/assets/sky.png";
  import mid from "$lib/assets/mid.png";
  import fore from "$lib/assets/fore.png";

  export const ParallaxDepth = {
    sky: "sky",
    mid: "mid",
    fore: "fore",
  };

  const images = [
    { src: sky, depth: ParallaxDepth.sky, zIndex: 0 },
    { src: mid, depth: ParallaxDepth.mid, zIndex: 1 },
    { src: fore, depth: ParallaxDepth.fore, zIndex: 2 },
  ];

  onMount(() => {
    const container = document.getElementById("main-container");
    if (!container) return;

    const height = container.offsetHeight / 2;

    const sequence = [
      [
        ".depth-sky",
        {
          transform: `translateY(${-(height * 0.35) - height * 0.05}px)`,
          ease: "none",
        },
        {
          transform: `translateY(0)`,
        },
      ],
      [
        ".depth-mid",
        {
          transform: `translateY(${-(height * 0.25)}px)`,
          ease: "none",
        },
        {
          transform: `translateY(0)`,
        },
      ],
      [
        ".depth-fore",
        {
          transform: `translateY(${-(height * 0.35)}px)`,
          ease: "none",
        },
        {
          transform: `translateY(0)`,
        },
      ],
    ];

    images.forEach((image) => {
      let translateY: number = -(height * 0.35) - height * 0.05;

      switch (image.depth) {
        case ParallaxDepth.sky:
          translateY = -(height * 0.35) - height * 0.05;
          break;
        case ParallaxDepth.mid:
          translateY = -(height * 0.25);
          break;
        case ParallaxDepth.fore:
          translateY = -(height * 0.35);
          break;
      }

      scroll(timeline(sequence as any), {
        target: container,
        offset: ["center center", "end end"],
      });
    });
  });
</script>

<main
  id="main-container"
  class="h-[300vh] w-screen overflow-hidden bg-white relative"
>
  {#each images as image}
    <div
      class={`image-container depth-${image.depth}`}
      style={`z-index: ${image.zIndex};`}
    >
      <img src={image.src} alt={`img ${image.depth}`} class="w-full h-full" />
    </div>
  {/each}
</main>

<style>
  .image-container {
    position: absolute;
    width: 100%;
    margin: 0;
    top: 0;
    will-change: transform;
  }

  .image-container.depth-sky {
    top: 0;
    bottom: initial;
  }
</style>

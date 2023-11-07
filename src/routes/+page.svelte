<script lang="ts">
  import "../style.css";
  import { onMount } from "svelte";
  import { ScrollOffset, animate, scroll, timeline } from "motion";
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
    const container = document.getElementById("header-container");
    if (!container) return;

    const height = container.offsetHeight / 2;

    const sequence = [
      [
        ".depth-fore",
        {
          transform: `translateY(${-(height * 0.45)}px)`,
          ease: "none",
        },
        {
          transform: `translateY(0)`,
        },
      ],
      [
        ".depth-mid",
        {
          transform: `translateY(${-(height * 0.35)}px)`,
          ease: "none",
        },
        {
          transform: `translateY(0)`,
        },
        { at: "<" },
      ],
      [
        ".depth-sky",
        {
          transform: `translateY(${-(height * 0.25)}px)`,
          ease: "none",
        },
        {
          transform: `translateY(0)`,
        },
      ],
    ];

    images.forEach((image) => {
      scroll(timeline(sequence as any), {
        target: container,
        offset: ["start start", "end end"],
      });
    });
  });
</script>

<main>
  <header
    id="header-container"
    class="h-screen xl:h-[200vh] w-screen overflow-hidden bg-white relative"
  >
    {#each images as image}
      <div
        class={`image-container depth-${image.depth}`}
        style={`z-index: ${image.zIndex};`}
      >
        <img src={image.src} alt={`img ${image.depth}`} class="w-full h-full" />
      </div>
    {/each}
    <div id="fade" />
  </header>
  <div class="h-screen bg-black" />
</main>

<style>
  :global(*) {
    overscroll-behavior-y: none;
  }

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

  #fade {
    position: absolute;
    height: 50vh;
    width: 100%;
    bottom: 0;
    left: 0;
    right: 0;
    background: linear-gradient(
      to top,
      black 0%,
      rgba(0, 0, 0, 0.738) 19%,
      rgba(0, 0, 0, 0.541) 34%,
      rgba(0, 0, 0, 0.382) 47%,
      rgba(0, 0, 0, 0.278) 56.5%,
      rgba(0, 0, 0, 0.194) 65%,
      rgba(0, 0, 0, 0.126) 73%,
      rgba(0, 0, 0, 0.075) 80.2%,
      rgba(0, 0, 0, 0.042) 86.1%,
      rgba(0, 0, 0, 0.021) 91%,
      rgba(0, 0, 0, 0.008) 95.2%,
      rgba(0, 0, 0, 0.002) 98.2%,
      transparent 100%
    );
    z-index: 10;
  }
</style>

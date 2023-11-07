<script lang="ts">
  import "../style.css";
  import { onMount } from "svelte";
  import { animate, scroll, timeline } from "motion";
  import sky from "$lib/assets/sky.png";
  import mid from "$lib/assets/mid.png";
  import fore from "$lib/assets/fore.png";
  import Lenis from "@studio-freight/lenis";
  import Marquee from "$lib/components/marquee.svelte";

  export const ParallaxDepth = {
    sky: "sky",
    mid: "mid",
    fore: "fore",
  };

  const images = [
    { src: sky, depth: ParallaxDepth.sky, zIndex: 0 },
    { src: mid, depth: ParallaxDepth.mid, zIndex: 1 },
    { src: fore, depth: ParallaxDepth.fore, zIndex: 3 },
  ];

  onMount(() => {
    const container = document.getElementById("header-container");
    if (!container) return;

    const height = container.offsetHeight / 2;

    animate(
      "#loader",
      { opacity: [1, 0], pointerEvents: "none" },
      { duration: 0.4 }
    );

    const sequence = [
      [
        ".depth-fore",
        {
          transform: `translateY(${-(height * 0.4)}px)`,
          ease: "none",
        },
        {
          transform: `translateY(0)`,
        },
      ],
      [
        ".depth-mid",
        {
          transform: `translateY(${-(height * 0.3)}px)`,
          ease: "none",
        },
        {
          transform: `translateY(0)`,
        },
        { at: 0 },
      ],
      [
        ".depth-sky",
        {
          transform: `translateY(${height * 0.1}px)`,
          ease: "none",
        },
        {
          transform: `translateY(0)`,
        },
        { at: 0 },
      ],
    ];

    images.forEach(() => {
      scroll(timeline(sequence as any), {
        target: container,
        offset: ["start start", "end end"],
      });
    });
  });

  onMount(() => {
    const content = document.getElementById("content") as HTMLElement;

    const lenis = new Lenis({
      lerp: 0.5,
      easing: (x) => {
        return x < 0.5 ? 4 * x * x * x : 1 - Math.pow(-2 * x + 2, 3) / 2;
      },
    });

    function raf(time: number) {
      lenis.raf(time);
      requestAnimationFrame(raf);
    }
    requestAnimationFrame(raf);
  });
</script>

<div id="loader" class="w-screen h-screen fixed bg-[#E0B4AA] z-50" />
<main>
  <header
    id="header-container"
    class="h-[80vh] xl:h-[150vh] w-screen overflow-hidden bg-white relative z-10"
  >
    <h1
      class="fixed top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 text-7xl font-bold text-white uppercase z-[2]"
    >
      Infinite
    </h1>
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
  <div id="content" class="h-screen bg-black z-20 relative py-40">
    <div
      class="rotate-[10deg] text-white overflow-hidden font-bold text-5xl bg-orange-600 py-4 box whitespace-nowrap"
    >
      <!-- <Marquee text="very long infinite text" /> -->
      VERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG
      INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE
      TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY
      LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE
      TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY
      LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXT
    </div>
    <div
      class="rotate-[-10deg] text-white overflow-hidden font-bold text-5xl bg-orange-600 py-4 box whitespace-nowrap"
    >
      <!-- <Marquee text="very long infinite text" /> -->
      VERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG
      INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE
      TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY
      LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE
      TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY
      LONG INFINITE TEXTVERY LONG INFINITE TEXTVERY LONG INFINITE TEXT
    </div>
  </div>
</main>

<style>
  :global(*) {
    overscroll-behavior-y: none;
    overscroll-behavior-x: none;
  }

  img {
    user-drag: none;
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

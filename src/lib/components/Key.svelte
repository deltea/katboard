<script lang="ts">
	import hotkeys from "hotkeys-js";
  import { onMount } from "svelte";
  import { Howl } from "howler";

  let { position, label, keymap }: {
    position: number,
    label: string,
    keymap: string
  } = $props();
  let isDown = $state(false);

  const sound = new Howl({
    src: ["/meowsynth.wav"],
    volume: 0.5,
    preload: true,
  });

  function playSound() {
    sound.play();
  }

  onMount(() => {
    hotkeys(keymap, { keyup: true }, (event) => {
      event.preventDefault();
      if (event.type === "keydown") {
        console.log("keydown");
        if (!isDown) {
          isDown = true;
          playSound();
        }
      } else if (event.type === "keyup") {
        console.log("keyup");
        isDown = false;
      }
    });
  });
</script>

{#if position % 1 !== 0}
  <button
    onmousedown={playSound}
    style:left={`${position * 68}px`}
    class="absolute -translate-y-8 top-0 h-2/3 w-16 rounded-2xl outline-none shadow-base bg-dark z-10 active:shadow-none active:-translate-y-[27px] duration-100 flex flex-col justify-end cursor-pointer {isDown ? "shadow-none -translate-y-[27px]" : ""}"
  >
    <span class="pb-2 text-fg font-bold">{label}</span>
  </button>
{:else}
  <button
    onmousedown={playSound}
    class="h-full rounded-2xl outline-none shadow-muted w-16 active:shadow-none active:translate-y-[5px] duration-100 flex flex-col justify-end bg-fg cursor-pointer {isDown ? "shadow-none translate-y-[5px]" : ""}"
  >
    <span class="pb-2 text-dark font-bold">{label}</span>
  </button>
{/if}

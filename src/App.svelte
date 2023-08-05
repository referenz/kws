<script lang="ts">
  import { Temporal } from "@js-temporal/polyfill";
  import Month from "./components/Month.svelte";
  import { onMount } from "svelte";

  const now = Temporal.Now.plainDateISO();

  const min = -12;
  const max = 12;

  const dates = [];

  for (let i = min; i <= max; i++) {
    const currDate = now.add({ months: i });
    dates.push(currDate);
  }

  onMount(() => {
    const button = document.querySelector('button');
    button.addEventListener(('click'), () => {
      const today = document.querySelector('.currentDay');
      today?.scrollIntoView({block: 'center', behavior: 'smooth'});
    })
  })


</script>

<main>
  {#each dates as date}
    <Month year={date.year} month={date.month} />
  {/each}
</main>

<button type="button">Zum heutigen Datum</button>

<style>
  button {
    position: fixed;
    top: 1em;
    left: 1em;

    display: block;
    background-color: rgb(144, 238, 144);
    height: 2em;

    border: 1px solid black;
    border-radius:15px;
  }

  button:hover {
      background-color:rgb(0, 255, 0);
      transition: 0.3s;
  }

button:active {
    background-color: red;
}
</style>


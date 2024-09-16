<script lang="ts">
  import { Temporal } from "@js-temporal/polyfill";
  import Month from "./components/Month.svelte";
  import { onMount } from "svelte";

  const now = Temporal.Now.plainDateISO();

  const min = -12;
  const max = 12;

  const dates : Temporal.PlainDate[] = [];
  for (let i = min; i <= max; i++) {
    const currDate = now.add({ months: i });
    dates.push(currDate);
  }

  onMount(() => {
    const today = document.querySelector(".currentDay");
    today?.scrollIntoView({ block: "center", behavior: "smooth" });

    const button = document.querySelector("button");
    button?.addEventListener("click", () => {
      today?.scrollIntoView({ block: "center", behavior: "smooth" });
    });
  });
</script>

<h1>Kalenderwochen-Anzeige (KWs)</h1>

<div>
  <nav>
    <button type="button">Zum heutigen Datum</button>
  </nav>

  <main>
    {#each dates as date}
      <Month year={date.year} month={date.month} />
    {/each}
  </main>
</div>

<footer><a href="https://github.com/referenz/kws">Quellcode auf GitHub</a></footer>

<style>
  h1 {
    text-align: center;
  }

  nav {
    text-align: center;
  }

  button {
    background-color: rgb(144, 238, 144);
    padding: 0.5em 1em;

    border: 1px solid black;
    border-radius: 15px;
  }

  button:hover {
    background-color: rgb(0, 255, 0);
    transition: 0.3s;
  }

  button:active {
    background-color: red;
  }

  @media (min-width: 768px) {
    div {
      display: grid;
      grid-template-columns: 1fr 2fr 1fr;
    }

    main {
      grid-column: 2;
      overflow: scroll;
    }

    nav {
      grid-column: 1;
      max-height: 100vh;

      position: sticky;
      top: 1em;
    }
  }

  @media (max-width: 768px) {
    main {
      margin: 0 .5em;
    }

    nav {
      position: fixed;
      bottom: 1.5em;
      left: 50%;
      transform: translateX(-50%);
    }

    button {
      background-color: rgba(144 238 144 / 0.67);
    }
  }
</style>

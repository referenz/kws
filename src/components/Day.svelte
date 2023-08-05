<script lang="ts">
  import { Temporal } from "@js-temporal/polyfill";
  import { onMount } from "svelte";

  export let date: Temporal.PlainDate;
  export let hasCurrentDay: boolean;

  const currentDay = hasCurrentDay && date.day === Temporal.Now.plainDateISO().day;
</script>

<td class:currentDay>
  {date.toLocaleString("de-DE", { day: "numeric" })}
</td>

<style>
  td.currentDay {
    color: red;
  }
</style>

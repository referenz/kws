<script lang="ts">
  import { Temporal } from "@js-temporal/polyfill";
  import Day from "./Day.svelte";
  export let first: Temporal.PlainDate;
  export let last: Temporal.PlainDate;

  const weekDays = ["Mo", "Di", "Mi", "Do", "Fr", "Sa", "So"];

  const checkbox = new Map(weekDays.map((_, i) => [i + 1, undefined]));

  let current = first;
  while (!current.subtract({ days: 1 }).equals(last)) {
    checkbox.set(current.dayOfWeek, current);
    current = current.add({ days: 1 });
  }

  const today = Temporal.Now.plainDateISO();
  const currentWeek = today.weekOfYear === first.weekOfYear && today.year === current.year;
</script>

<tr class:currentWeek>
  <th>{first.weekOfYear}</th>
  {#each checkbox as [_, date]}
    {#if !date}
      <td class="empty-date" />
    {:else}
      <Day {date} hasCurrentDay={currentWeek} />
    {/if}
  {/each}
</tr>

<style>
  tr.currentWeek {
    background-color: yellow;
  }
</style>

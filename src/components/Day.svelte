<script lang="ts">
  import { Temporal } from "@js-temporal/polyfill";

  export let date: Temporal.PlainDate;
  export let isCurrentWeek: boolean;

  const currentDay = isCurrentWeek && date.day === Temporal.Now.plainDateISO().day;
</script>

<td class:currentDay>
  {date.toLocaleString("de-DE", { day: "numeric" })}
</td>

<style>
  td {
    text-align: right;
  }
  td.currentDay {
    color: red;
  }
</style>

<script lang="ts">
  import { Temporal } from "@js-temporal/polyfill";
  import Week from "./Week.svelte";

  interface Props {
    year: number;
    month: number;
  }

  let { year, month }: Props = $props();

  const weekDays = ["Mo", "Di", "Mi", "Do", "Fr", "Sa", "So"];

  function getFirstOfMonth(): Temporal.PlainDate {
    return new Temporal.PlainDate(year, month, 1);
  }

  function getLastOfMonth(): Temporal.PlainDate {
    return new Temporal.PlainDate(year, month, 1).add({ months: 1 }).subtract({ days: 1 });
  }

  function getMonthName(): string {
    const date = getFirstOfMonth();
    return date.toLocaleString("default", { month: "long" });
  }

  function getNextSunday(dateString: Temporal.PlainDate) {
    const date = Temporal.PlainDate.from(dateString);
    const dayOfWeek = date.dayOfWeek;
    const daysUntilNextSunday = (7 - dayOfWeek) % 7;
    const nextSunday = date.add({ days: daysUntilNextSunday });
    return nextSunday;
  }

  function getPreviousMonday(dateString: Temporal.PlainDate): Temporal.PlainDate {
    const date = Temporal.PlainDate.from(dateString);
    const dayOfWeek = date.dayOfWeek;
    const daysUntilPreviousMonday = (dayOfWeek - 1 + 7) % 7; // Der Montag ist Wochentag 1
    const previousMonday = date.subtract({ days: daysUntilPreviousMonday });
    return previousMonday;
  }

  function getEpochSeconds(date: Temporal.PlainDate) {
    const instant = Temporal.Instant.from(date.toZonedDateTime({ timeZone: "Europe/Berlin" }).toString());
    return instant.epochSeconds;
  }

  const weeks: [Temporal.PlainDate, Temporal.PlainDate][] = [];
  weeks.push([getFirstOfMonth(), getNextSunday(getFirstOfMonth())]);

  let current = getFirstOfMonth();
  do {
    current = current.add({ weeks: 1 });
    weeks.push([getPreviousMonday(current), getNextSunday(current)]);
  } while (getEpochSeconds(getNextSunday(current).add({ weeks: 1 })) < getEpochSeconds(getLastOfMonth()));

  weeks.push([getPreviousMonday(getLastOfMonth()), getLastOfMonth()]);
</script>

<table>
  <caption>{getMonthName()} {year}</caption>
  <thead>
    <tr>
      <th>KW</th>
      {#each weekDays as weekDay}
        <th>{weekDay}</th>
      {/each}
    </tr>
  </thead>
  <tbody>
    {#each weeks as [first, last]}
      <Week {first} {last} />
    {/each}
  </tbody>
</table>

<style>
  table {
    margin: 2em auto;
  }
</style>

<script>
  import { DateTime, Interval, Duration } from "luxon";
  import Button from "@smui/button";
  import DayPicker from "./DayPicker.svelte";
  import TimePicker from "./TimePicker.svelte";

  export let timeline;
  export let action;

  let monthModificator = 0;
  let now = DateTime.local().plus({ month: monthModificator });
  let firstDayOfFirstWeek = now.startOf("month").startOf("week");
  let lastDayOfLasttWeek = now.endOf("month").endOf("week");
  let monthToDisplay = Interval.fromDateTimes(
    firstDayOfFirstWeek,
    lastDayOfLasttWeek
  );
  let monthMatrix = [];
  let dayMatrix = [];
  for (let v = 0; v < monthToDisplay.length("day"); v++) {
    monthMatrix = [...monthMatrix, firstDayOfFirstWeek.plus({ day: v })];
  }
  $: for (let v = 0; v < 24; v = v + 1) {
    for (let i = 0; i < 60; i = i + 15) {
      dayMatrix = [
        ...dayMatrix,
        {
          hour: v,
          minute: i,
          working: false,
        },
      ];
    }
  }

  function monthModificatorController(value) {
    monthModificator = monthModificator + value;
    now = DateTime.local().plus({ month: monthModificator });

    firstDayOfFirstWeek = now.startOf("month").startOf("week");
    lastDayOfLasttWeek = now.endOf("month").endOf("week");
    monthToDisplay = Interval.fromDateTimes(
      firstDayOfFirstWeek,
      lastDayOfLasttWeek
    );
    monthMatrix = [];

    for (let v = 0; v < monthToDisplay.length("day"); v++) {
      monthMatrix = [...monthMatrix, firstDayOfFirstWeek.plus({ day: v })];
    }
  }
</script>

<main>
  <Button on:click={() => monthModificatorController(-1)}>Previous month</Button
  >
  <h2>{now.monthLong}</h2>
  <Button on:click={() => monthModificatorController(1)}>Next Month</Button>

  <div class="calendar">
    <div class="weekday">Monday</div>
    <div class="weekday">Tuesday</div>
    <div class="weekday">Wednesday</div>
    <div class="weekday">Thursday</div>
    <div class="weekday">Friday</div>
    <div class="weekday">Saturday</div>
    <div class="weekday">Sunday</div>
    {#each monthMatrix as day, i (day.ts)}
      <DayPicker on:pick-a-day {day} {timeline} {action} />
    {/each}
    <div class="timeMatrix">
      {#each dayMatrix as day}
        <TimePicker on:pick-a-time {day} {timeline} {action} />
      {/each}
    </div>
  </div>
</main>

<style>
  h2 {
    width: 200px;
  }
  .calendar {
    max-width: 540px;
    display: flex;
    flex-wrap: wrap;
    margin: 0 auto;
  }
  .weekday {
    width: 70px;
    font-size: smaller;
    float: left;
    text-align: center;
  }
  main {
    text-align: center;
    padding: 1em;
    max-width: 90%;
    margin: 0 auto;
  }
  .timeMatrix {
    margin: auto auto;
    width: 400px;
  }

  @media (min-width: 640px) {
    main {
      display: flex;
      flex-wrap: wrap;
    }
  }
</style>

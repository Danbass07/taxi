<script>
  import { DateTime, Interval, Duration } from "luxon";
  //import Button from "@smui/button";
  import { createEventDispatcher } from "svelte";

  export let day;
  export let timeline;
  export let action;
  let active = true;
  let buttonClass = "notActiveDay";
  let disableButton = false;

  if (action === "Selecting Time") {
    disableButton = true;
  }
  const dispatch = createEventDispatcher();

  $: active = timeline.workDays.filter(
    (date) =>
      date[0].month === day.toObject().month &&
      date[0].day === day.toObject().day
  );

  $: if (active.length !== 0) {
    buttonClass = "activeDay";
  } else {
    buttonClass = "notActiveDay";
  }
  function dayPicker(day) {
    dispatch("pick-a-day", day);
  }
</script>

<button
  class={buttonClass + " " + "weekday"}
  variant="outlined"
  disabled={disableButton}
  on:click={() => dayPicker(day)}
  >{day.day}
</button>

<style>
  .weekday {
    width: 70px;
    font-size: smaller;
    float: left;
    text-align: center;
  }
  .calendar {
    max-width: 540px;
    display: flex;
    flex-wrap: wrap;
    margin: 0 auto;
  }
  button {
    width: 100px;
    color: aliceblue;
  }
  .activeDay {
    background-color: blue;
  }
  .notActiveDay {
    background-color: darkgoldenrod;
  }
</style>

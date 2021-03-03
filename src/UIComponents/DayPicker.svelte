<script>
  import { DateTime, Interval, Duration } from "luxon";
  import Button from "@smui/button";
  import { createEventDispatcher } from "svelte";

  export let day;
  export let timeline;

  let colour = "secondary";

  const dispatch = createEventDispatcher();

  function dayPicker() {
    dispatch("pick-a-day", day);
  }

  $: timeline.workDays.forEach((date) => {
    let dayObject = day.toObject();

    if (date[0].month === dayObject.month && date[0].day === dayObject.day) {
      colour = "primary";
    }
  });
</script>

<div class="weekday">
  <Button variant="outlined" color={colour} on:click={dayPicker}
    >{day.day}</Button
  >
</div>

<style>
  .weekday {
    width: 70px;
    font-size: smaller;
    float: left;
    text-align: center;
  }
</style>

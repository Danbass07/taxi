<script>
  import { onMount } from "svelte";
  import { createEventDispatcher } from "svelte";

  export let day;
  export let action;
  export let timeline;
  const dispatch = createEventDispatcher();

  function timePicker(hour, minute) {
    dispatch("pick-a-time", [hour, minute]);
  }
</script>

<button
  class="timeSlot"
  on:click={() => {
    timePicker(day.hour, day.minute);
  }}
  >{day.hour === 0 ? "00" : day.hour} {day.minute === 0 ? "00" : day.minute}
</button>

<style>
  .timeSlot {
    width: 100px;
  }
</style>

<script>
  import Calendar from "../UIComponents/Calendar.svelte";
  import TimePicker from "../UIComponents/TimePicker.svelte";
  import { DateTime, Interval, Duration } from "luxon";

  let timeline = {
    id: 1,
    user_id: 1,
    workDays: [
      [
        {
          year: 2021,
          month: 2,
          day: 1,
          hour: 0,
          minute: 0,
          second: 0,
          millisecond: 0,
        },

        [null, null, null],
      ],
      [
        {
          year: 2021,
          month: 2,
          day: 2,
          hour: 0,
          minute: 0,
          second: 0,
          millisecond: 0,
        },

        [null, null, null],
      ],
    ],
  };
  let selectedDay;
  let action = "Selecting Day";
  function actionController(event) {
    if (event.target.name === "Selecting Day") {
      action = "Selecting Time";
    } else {
      action = "Selecting Day";
    }
  }

  function dayPickerController(event) {
    if (action === "Selecting Day") {
      let newDay = [{ ...event.detail.toObject() }, [null, null, null]];
      let addDay = true;
      timeline.workDays.map((workDay, index) => {
        if (
          workDay[0].year === newDay[0].year &&
          workDay[0].month === newDay[0].month &&
          workDay[0].day === newDay[0].day
        ) {
          addDay = false;
          timeline.workDays.splice(index, 1);
          timeline.workDays = [...timeline.workDays];
        }
      });
      if (addDay) {
        timeline.workDays = [...timeline.workDays, newDay];
      }
      console.log(timeline);
      // console.log(timeline);
    } else {
      console.log(event.detail);
    }
  }
  function timePickerController(event) {
    if (action === "Selecting Time") {
      console.log(event.detail);
    }
  }
</script>

<main>
  <button on:click={actionController} name={action}>{action}</button>
  <Calendar
    on:pick-a-day={dayPickerController}
    on:pick-a-time={timePickerController}
    {timeline}
    {action}
  />
</main>

<style>
</style>

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

      let data = timeline.workDays.filter(
        (workDay) => JSON.stringify(workDay[0]) === JSON.stringify(newDay[0])
      );

      if (data.length === 0) {
        timeline.workDays = [...timeline.workDays, newDay];
      } else {
        timeline.workDays = [
          ...timeline.workDays.filter(
            (workDay) =>
              JSON.stringify(workDay[0]) !== JSON.stringify(newDay[0])
          ),
        ];
      }
    }
  }
  function timePickerController(event) {
    if (action === "Selecting Time") {
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

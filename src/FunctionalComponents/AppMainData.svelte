<script>
  import DatePicker from "../UIComponents/DatePicker.svelte";
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

  function dayPickerController(event) {
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
        //timeline.workDays = timeline.workDays;
      }
    });
    if (addDay) {
      timeline.workDays = [...timeline.workDays, newDay];
    }
  }
</script>

<main>
  <DatePicker on:pick-a-day={dayPickerController} {timeline} />
</main>

<style>
</style>

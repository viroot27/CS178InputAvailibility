<script>
  import Radio from './Radio.svelte'
    export let rows = 60;
    export let cols = 7;
    export let title = 'Find Some Time';
    let radioValue;
    export let timeSlots = [
      { day: 'Monday', start: '9:00', end: '23:59' },
      { day: 'Tuesday', start: '9:00', end: '23:59' },
      { day: 'Wednesday', start: '9:00', end: '23:59' },
      { day: 'Thursday', start: '9:00', end: '23:59' },
      { day: 'Friday', start: '9:00', end: '23:59' },
      { day: 'Saturday', start: '9:00', end: '23:59' },
      { day: 'Sunday', start: '9:00', end: '23:59' }
    ];
    export let availability = Array.from({ length: rows }, () =>
      Array.from({ length: cols }, () => [false, radioValue])
    );
    //If radio value is selected do the radio value color, if not do white
    function toggleAvailability(row, col) {
      availability[row][col][0] = !availability[row][col][0];
      if (availability[row][col][0]){
        (availability[row][col][1]) = radioValue
      }else{
        availability[row][col][1] = "white"
      }
      availability = [...availability];
    }
  
    function incrementTime(time, minutes) {
      let [hours, mins] = time.split(':').map(Number);
      mins += minutes;
      hours += Math.floor(mins / 60);
      mins %= 60;
      return `${String(hours).padStart(2, '0')}:${String(mins).padStart(2, '0')}`;
    }

	
	const options = [{
		value: 'green',
		label: 'In Person',
	}, {
		value: 'blue',
		label: 'In Zooom only',
	}]

  </script>
  
  <h1>{title}</h1>
  <Radio {options} fontSize={16} legend='Select availability in' bind:userSelected={radioValue}/>
  <table>
    <thead>
      <tr>
        <th></th>
        {#each timeSlots as { day }}
        <th>{day}</th>
        {/each}
      </tr>
    </thead>
    <tbody>
      {#each Array(rows) as _, row}
      <tr>
        <td>{incrementTime(timeSlots[0].start, row * 15)}</td>
        {#each Array(cols) as _, col}
        <td>
          <button
            on:click={() => toggleAvailability(row, col)}
            style="background-color:{availability[row][col][1]}"
          ></button>
        </td>
        {/each}
      </tr>
      {/each}
      <tr>
        <td></td>
        {#each timeSlots as { day, start }}
        <td>{start} - {incrementTime(start, 60 * rows)}</td>
        {/each}
      </tr>
    </tbody>
  </table>
  
  <style>
    
    table {
      border-collapse: collapse;
    }
    td {
      border: 1px solid rgb(75, 75, 75);
      padding: 5px;
      text-align: center;
    }
    button {
      width: 100%;
      height: 100%;
      border: none;
      background-color: white;
    }

  </style>
  
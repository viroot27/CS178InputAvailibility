<script>
  import Radio from './Radio.svelte'

  // // loclal storage
  // import NameInput from './NameInput.svelte';
  
  // let name = localStorage.getItem('name');

  // Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAuth } from "Firebase/auth";
import { collection, doc, getFirestore } from "Firebase/firestore/lite";

import 'firebase/firestore';
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

  // Your web app's Firebase configuration
  const firebaseConfig = {
  apiKey: "AIzaSyBT2Ur07b6fkr8p5wp887fJ39vb5djycSY",
  authDomain: "inputavailability.firebaseapp.com",
  projectId: "inputavailability",
  storageBucket: "inputavailability.appspot.com",
  messagingSenderId: "1080505983154",
  appId: "1:1080505983154:web:1bf5c0effd14d1a3eb29ab",
  measurementId: "G-5N2YW6CR34"
};


// Initialize Firebase
const app = initializeApp(firebaseConfig);
let db = getFirestore(app);
//  intialize username
let name = "";

//  username function
function submitForm() {
  console.log("Nico")
  const db=getDatabase();
  set(ref(db,'users/' + name),{
    });
  }

  


   
  
    export let rows = 21;
    export let cols = 7;
    export let title = 'Find Some Time';
    let radioValue;
    export let timeSlots = [
      { day: 'Monday', start: '9:00', end: '19:00' },
      { day: 'Tuesday', start: '9:00', end: '19:00' },
      { day: 'Wednesday', start: '9:00', end: '19:00' },
      { day: 'Thursday', start: '9:00', end: '19:00' },
      { day: 'Friday', start: '9:00', end: '19:00' },
      { day: 'Saturday', start: '9:00', end: '19:00' },
      { day: 'Sunday', start: '9:00', end: '19:00' }
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
 
  <!--local storage stuff-->  
  <!-- {#if name}
  <p>Hello {name}!</p>
  {:else}
  <NameInput />
  {/if} -->

  <form on:submit|preventDefault={submitForm} style="white-space:nowrap">
    <label for="name-input">Enter your name:</label>
    <input type="text" id="name-input" bind:value={name}>
    <button type="submit" style="width:auto">Submit</button>
  </form>

  <Radio {options} fontSize={16} legend='Location:' bind:userSelected={radioValue}/>
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
        <td>{incrementTime(timeSlots[0].start, row * 30)}</td>
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
        <!-- <td></td>
        {#each timeSlots as { day, start }}
        <td>{start} - {incrementTime(start, 60 * rows)}</td>
        {/each} -->
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

    th{
      width:100px;
    }

  </style>
  
<!-- https://www.w3schools.com/js/js_syntax.asp was a good resource for javascript sytnax
ChatGPT was useful in helping debug the code efficiently -->

<script>
  //Radio-based location preferences concept
  import Radio from './Radio.svelte'


  // Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAuth } from "Firebase/auth";
import { collection, doc, setDoc, getFirestore, updateDoc } from "Firebase/firestore/lite";

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
let isSubmitted = false

//  username function with the help of chat GPT
//simplified ui concept: b) enter name by enter key concept
  function submitForm() {
   setDoc(doc(db, 'user/'+name), {
    start_time : new Date(),
  })
  
  .then(() => {
    console.log("succesful")
    document.getElementById("name-input").value = "";
    isSubmitted = true
  })
  .catch((error) => {
    console.log("unsucess")
  }

  )
}
  ////simplified ui concept: b) enter name by enter key concept
  function submitEnter(event) {
    if (event.key === "Enter") {
      submitForm();
    }
  }

   
  //simplified ui concept: (a)simplified-time concept, and click-in availability concept
    export let rows = 21;
    export let cols = 7;
    export let title = 'MeetMe';
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

    //click-in availability concept and color-based location selection concept
    export let availability = Array.from({ length: rows }, () =>
      Array.from({ length: cols }, () => [false, radioValue])
    );
    // Radio-based location preferences concept and color-based location selection concept. 
    //If radio value is selected do the radio value color, if not do white
    //simplified ui concept: alert concept
    function toggleAvailability(row, col) {
      if (name != ""){
      availability[row][col][0] = !availability[row][col][0];
      if (availability[row][col][0]){
        (availability[row][col][1]) = radioValue
      }else{
        availability[row][col][1] = "white"
      }
      availability = [...availability];
    }else{
      alert("Please enter your name first")
    }
  }
    
    //lick-in availability concept, and simplified ui concept (a)simplified-time concept
    function incrementTime(time, minutes) {
      let [hours, mins] = time.split(':').map(Number);
      mins += minutes;
      hours += Math.floor(mins / 60);
      mins %= 60;
      return `${String(hours).padStart(2, '0')}:${String(mins).padStart(2, '0')}`;
    }

	//Radio-based location preferences concept: color-based location selection concept
	const options = [{
		value: 'green',
		label: 'Anywhere In Person (Shows in Green)',
	}, {
		value: 'blue',
		label: 'On Zoom only (Blue)',
  }, {
		value: 'purple',
		label: 'SEC only (Purple)',
	}, {
    value: 'crimson',
    label: 'Harvard Square only (Crimson)',
  }, {
    value: 'pink',
    label: 'Quad only (Pink)',
  }, {
    value: 'orange',
    label: 'Maxwell Dworkin only (Orange)',
  }  
]

  //finished function with help from ChatGPT for endtime per user
  function finish(){
    if (name != ""){
      const ref = doc(db, "user", name)

      updateDoc(ref, {
        "end_time": new Date()
      })

      isSubmitted = false

      availability = Array.from({length: rows}, () =>
        Array.from({length: cols }, () => [false,"white"])

      );


    }
  }

  </script> 
  <h1>{title}</h1>
   <!--simplified ui concept: b) enter name by enter key concept</form> -->
  <form on:submit|preventDefault={submitForm} on:keydown={submitEnter} style="white-space:nowrap">
    <label for="name-input">Name:</label>
    <input type="text" id="name-input" bind:value={name}>
    <button type="submit" class="smallButton">Begin</button>
  </form>
  {#if isSubmitted}
  <p> Hello {name} :)</p>
  {/if}
  
   <!--Radio-based location preferences concept: color-based location selection concept </form> -->
  <Radio {options} fontSize={16} legend='Location Preference for Time Slots:' bind:userSelected={radioValue}/>
  <!-- <form on:submit|preventDefault={submitForm} class="smallForm" style="white-space:nowrap">
    <label for="location" >Suggest Location:</label>
    <input type="text" id="location" >
    <button type="submit" class="smallButton">Submit</button>
  </form> -->
  <button style="width:auto" class="smallButton" on:click={finish}>Finish</button>
  <table>
    <thead>
      <tr>
        <!-- 3) simplified ui concept (a)simplified-time concept  </form> -->
        <th></th>
        {#each timeSlots as { day }}
        <th>{day}</th>
        {/each}
      </tr>
    </thead>
    <tbody>
      {#each Array(rows) as _, row}
      <tr>
        <!-- 3) simplified ui concept (a)simplified-time concept  </form> -->
        <td>{incrementTime(timeSlots[0].start, row * 30)}</td>
        {#each Array(cols) as _, col}
        <td>
         <!--  click-in availability concept and color-based location selection concept </form> -->
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
      text-align: center
    }
    button {
      width: 110px;
      height: 30px;
      border: none;
      background-color: white;
    }
  /* Click in availability: Maximize click in concept*/
    .smallButton {
      width: 80px;
      height: 40px;
      border: none;
      background-color: white;
      text-align: left;

    }

    th{
      width:100px;
    }

  </style>
  
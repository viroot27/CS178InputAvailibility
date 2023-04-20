<!-- Login  -->

<script>
    let name = '';
    
    const handleSubmit = () => {
      localStorage.setItem('name', name);
    }
  </script>
  
  <label>
    Name:
    <input type="text" bind:value={name} />
  </label>
  <!-- SignIn() -->
  <button on:click={handleSubmit}>
    Submit
  </button>
  
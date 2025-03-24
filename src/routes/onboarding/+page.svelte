<script lang="ts">
  import { fade } from 'svelte/transition';
  import { goto } from "$app/navigation";
  import axios from 'axios';

  // Total steps updated to 4
  let step = 1;
  const totalSteps = 4;
  
  let formData = {
    firstName: '',
    lastName: '',
    interests: [] as string[],
    writingStyle: '',
    tone: '',
    structure: '',
    influences: '',
    postingDays: [] as string[],
    preferredStart: '', // Time in "HH:MM" format
    preferredEnd: ''    // Time in "HH:MM" format
  };

  let customInterest = '';
  const predefinedInterests = ['Entrepreneurship', 'Meditation', 'Tech', 'AI', 'LLMs', 'Writing', 'Reading', 'Gaming'];
  const daysOfWeek = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];

  async function goToPreview() {
    console.log('go to preview');
    try {
      const response = await axios.post(
        'http://127.0.0.1:8000/login',
        { formData },
        { withCredentials: true }
      );
      console.log('Redirecting to:', response.request.responseURL);
      window.location.href = response.request.responseURL;
    } catch (error) {
      console.error('Login error:', error?.response?.data || error?.message || "error");
    }
  }

  function nextStep() {
    // You can add further validation before progressing if needed.
    step += 1;
  }

  function addInterest(interest: string) {
    if (!formData.interests.includes(interest)) {
      formData.interests = [...formData.interests, interest];
    }
    customInterest = '';
  }

  function removeInterest(interest: string) {
    formData.interests = formData.interests.filter(i => i !== interest);
  }

  // Toggle posting day selection (max two selections)
  function toggleDay(day: string) {
    if (formData.postingDays.includes(day)) {
      formData.postingDays = formData.postingDays.filter(d => d !== day);
    } else {
      if (formData.postingDays.length < 2) {
        formData.postingDays = [...formData.postingDays, day];
      }
    }
  }

  // Validate that the preferred end time does not exceed start time + 1 hour.
  function validateTime() {
    if (formData.preferredStart && formData.preferredEnd) {
      const [startHour, startMin] = formData.preferredStart.split(':').map(Number);
      const [endHour, endMin] = formData.preferredEnd.split(':').map(Number);
      const startTime = startHour * 60 + startMin;
      const endTime = endHour * 60 + endMin;
      if (endTime > startTime + 60) {
        alert("End time cannot be greater than start time plus one hour.");
        formData.preferredEnd = '';
      }
    }
  }
</script>

<div class="container">
  <!-- Progress Bar -->
  <div class="progress-bar">
    <div class="progress" style="width: {(step / totalSteps) * 100}%"></div>
  </div>

  {#if step === 1}
    <div class="form-step" transition:fade>
      <h2>Tell Us About Yourself</h2>
      <div class="input-group">
        <label for="firstName">First Name</label>
        <input id="firstName" type="text" bind:value={formData.firstName} placeholder="Your first name" />
      </div>
      <div class="input-group">
        <label for="lastName">Last Name</label>
        <input id="lastName" type="text" bind:value={formData.lastName} placeholder="Your last name" />
      </div>
      <div class="input-group">
        <label for="interests">Interests & Hobbies</label>
        <div class="interests-tags">
          {#each formData.interests as interest}
            <span class="tag">
              {interest}
              <button on:click={() => removeInterest(interest)}>x</button>
            </span>
          {/each}
        </div>
        <input type="text" bind:value={customInterest} placeholder="Add custom interest..." on:keydown={(e) => e.key === 'Enter' && customInterest && addInterest(customInterest)} />
        <div class="predefined-interests">
          {#each predefinedInterests as interest}
            <button class="interest-btn" on:click={() => addInterest(interest)}>{interest}</button>
          {/each}
        </div>
      </div>
      <button class="next-btn" on:click={nextStep}>Next</button>
    </div>

  {:else if step === 2}
    <div class="form-step" transition:fade>
      <h2>Your Writing Style</h2>
      <div class="input-group">
        <label for="writingStyle">Overall Writing Style</label>
        <select id="writingStyle" bind:value={formData.writingStyle}>
          <option value="">Select one...</option>
          <option value="descriptive">Descriptive</option>
          <option value="concise">Concise</option>
          <option value="analytical">Analytical</option>
          <option value="narrative">Narrative</option>
          <option value="creative">Creative</option>
        </select>
      </div>
      <div class="input-group">
        <label for="tone">Tone</label>
        <select id="tone" bind:value={formData.tone}>
          <option value="">Select one...</option>
          <option value="formal">Formal</option>
          <option value="informal">Informal</option>
          <option value="humorous">Humorous</option>
          <option value="serious">Serious</option>
        </select>
      </div>
      <div class="input-group">
        <label for="structure">Structure</label>
        <select id="structure" bind:value={formData.structure}>
          <option value="">Select one...</option>
          <option value="detailed">Detailed outlines</option>
          <option value="freeform">Freeform / Stream-of-consciousness</option>
        </select>
      </div>
      <div class="input-group">
        <label for="influences">Writing Influences</label>
        <input id="influences" type="text" bind:value={formData.influences} placeholder="E.g., favorite authors or styles" />
      </div>
      <button class="next-btn" on:click={nextStep}>Next</button>
    </div>

  {:else if step === 3}
    <div class="form-step" transition:fade>
      <h2>What Days Do You Want to Post?</h2>
      <p>Select up to two days:</p>
      <div class="days-tags">
        {#each daysOfWeek as day}
          <button 
            class:active={formData.postingDays.includes(day)}
            on:click={() => toggleDay(day)}>
            {day}
          </button>
        {/each}
      </div>
      <div class="input-group">
        <label for="preferredStart">Preferred Start Time</label>
        <input id="preferredStart" type="time" bind:value={formData.preferredStart} on:change={validateTime} />
      </div>
      <div class="input-group">
        <label for="preferredEnd">Preferred End Time (max 1 hour after start)</label>
        <input id="preferredEnd" type="time" bind:value={formData.preferredEnd} on:change={validateTime} />
      </div>
      <button class="next-btn" on:click={nextStep}>Next</button>
    </div>

  {:else if step === 4}
    <div class="form-step" transition:fade>
      <h2>Almost Done!</h2>
      <p>Great! You're almost done. Please sign up with LinkedIn to complete your registration.</p>
      <button class="linkedin-btn" on:click={goToPreview}>
        Sign Up with LinkedIn
      </button>
    </div>
  {/if}
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap');

  * {
    font-family: 'Poppins', sans-serif;
  }

    /* Basic styling to emulate a LinkedIn-ish theme */
    .container {
    max-width: 700px;
    margin: 2rem auto;
    padding: 1rem;
    font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
    color: #333;
  }
  h2 {
    color: #0073b1;
  }
  .input-group {
    margin-bottom: 1rem;
  }
  label {
    display: block;
    margin-bottom: 0.25rem;
  }
  input[type="text"],
  input[type="time"],
  select {
    width: 100%;
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  .interests-tags, .days-tags {
    margin-bottom: 0.5rem;
  }
  .tag,
  .days-tags button {
    display: inline-block;
    margin: 0.25rem;
    padding: 0.25rem 0.5rem;
    background-color: #e1ecf4;
    color: #0073b1;
    border: none;
    border-radius: 3px;
    cursor: pointer;
  }
  .days-tags button.active {
    background-color: #0073b1;
    color: white;
  }
  .interest-btn {
    margin: 0.25rem;
    padding: 0.25rem 0.5rem;
    background-color: #f3f6f8;
    border: 1px solid #ccc;
    border-radius: 3px;
    cursor: pointer;
  }
  .next-btn, .linkedin-btn {
    background-color: #0073b1;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 3px;
    cursor: pointer;
  }
  .progress-bar {
    background-color: #e1ecf4;
    border-radius: 4px;
    overflow: hidden;
    margin-bottom: 1rem;
  }
  .progress {
    background-color: #0073b1;
    height: 8px;
    transition: width 0.3s ease;
  }

  .container {
    max-width: 600px;
    margin: 2rem auto;
    background: #fff; /* White background */
    padding: 2rem;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    border: 1px solid #e6e9ec; /* Light border similar to LinkedIn */
  }
  h2 {
    color: #0077B5; /* LinkedIn Blue */
    margin-bottom: 1.5rem;
    text-align: center;
  }
  .form-step {
    display: flex;
    flex-direction: column;
  }
  .input-group {
    display: flex;
    flex-direction: column;
    margin-bottom: 1rem;
  }
  label {
    margin-bottom: 0.5rem;
    font-weight: 600;
    color: #333; /* Darker gray for labels */
  }
  input,
  select {
    padding: 0.75rem;
    font-size: 1rem;
    border: 1px solid #e6e9ec; /* Light border similar to LinkedIn */
    border-radius: 5px;
    transition: border-color 0.2s ease;
  }
  input:focus,
  select:focus {
    border-color: #0077B5; /* LinkedIn Blue on focus */
    outline: none;
  }
  .next-btn,
  .linkedin-btn {
    width: 100%;
    padding: 0.75rem;
    font-size: 1rem;
    border: none;
    border-radius: 5px;
    background: #0077B5; /* LinkedIn Blue */
    color: #fff;
    cursor: pointer;
    transition: background 0.3s ease;
    margin-top: 1rem;
  }
  .next-btn:hover,
  .linkedin-btn:hover {
    background: #005f99; /* Darker shade of LinkedIn Blue on hover */
  }
  p {
    text-align: center;
    margin-bottom: 1.5rem;
    color: #666;
  }

  .interests-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 0.5rem;
  }

  .tag {
    background-color: #e6e9ec; /* Light gray tag background */
    padding: 0.25rem 0.5rem;
    border-radius: 15px;
    font-size: 0.8rem;
    display: flex;
    align-items: center;
  }

  .tag button {
    background: none;
    border: none;
    color: #555;
    cursor: pointer;
    font-size: 0.8rem;
    margin-left: 0.25rem;
  }

  .predefined-interests {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-top: 0.5rem;
  }

  .interest-btn {
    background-color: #f0f2f5; /* Very light gray for interest buttons */
    padding: 0.25rem 0.5rem;
    border-radius: 15px;
    border: none;
    cursor: pointer;
    font-size: 0.8rem;
    color: #333;
  }
    .interest-btn:hover{
        background-color: #e6e9ec;
    }
</style>
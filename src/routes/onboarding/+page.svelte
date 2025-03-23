<script lang="ts">
  import { fade } from 'svelte/transition';

  let step = 1;
  let formData = {
    firstName: '',
    lastName: '',
    interests: [], // Change interests to an array for tags
    writingStyle: '',
    tone: '',
    structure: '',
    influences: ''
  };

  let customInterest = '';
  const predefinedInterests = ['Entrepreneurship', 'Meditation', 'Tech', 'AI', 'LLMs', 'Writing', 'Reading', 'Gaming'];

  function nextStep() {
    step += 1;
  }

  function addInterest(interest: string) {
    if (!formData.interests.includes(interest)) {
      formData.interests = [...formData.interests, interest];
    }
    customInterest = ''; // Reset custom interest input
  }

  function removeInterest(interest: string) {
    formData.interests = formData.interests.filter(i => i !== interest);
  }
</script>

<div class="container">
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
      <h2>Almost Done!</h2>
      <p>Great! You're almost done. Please sign up with LinkedIn to complete your registration.</p>
      <button class="linkedin-btn" on:click={() => {/* Insert LinkedIn signup logic here */}}>
        Sign Up with LinkedIn
      </button>
    </div>
  {/if}
</div>

<style>
  /* ... (your existing styles remain the same) */
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap');

  * {
    font-family: 'Poppins', sans-serif;
  }

  .container {
    max-width: 500px;
    margin: 2rem auto;
    background: #fff;
    padding: 2rem;
    border-radius: 10px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }
  h2 {
    color: #333;
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
    color: #555;
  }
  input,
  select {
    padding: 0.75rem;
    font-size: 1rem;
    border: 1px solid #ddd;
    border-radius: 5px;
    transition: border-color 0.2s ease;
  }
  input:focus,
  select:focus {
    border-color: #0077ff;
    outline: none;
  }
  .next-btn,
  .linkedin-btn {
    width: 100%;
    padding: 0.75rem;
    font-size: 1rem;
    border: none;
    border-radius: 5px;
    background: #0077ff;
    color: #fff;
    cursor: pointer;
    transition: background 0.3s ease;
    margin-top: 1rem;
  }
  .next-btn:hover,
  .linkedin-btn:hover {
    background: #005fcc;
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
    background-color: #e0e0e0;
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
    background-color: #f0f0f0;
    padding: 0.25rem 0.5rem;
    border-radius: 15px;
    border: none;
    cursor: pointer;
    font-size: 0.8rem;
  }
</style>
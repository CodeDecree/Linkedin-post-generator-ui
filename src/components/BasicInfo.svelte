<script>
  import { createEventDispatcher } from 'svelte';

  export let firstName = '';
  export let lastName = '';
  export let email = '';
  export let interests = []; // Array to hold selected interests

  const dispatch = createEventDispatcher();

  function handleNext() {
      if (isFormValid()) {
          dispatch('next', { interests }); // Pass interests in the event
      } else {
          showErrors = true;
      }
  }

  function updateField(field, value) {
      dispatch('update', { field, value });
  }

  let showErrors = false;
  let customInterest = '';
  const predefinedInterests = ['entrepreneurship', 'meditation', 'tech', 'AI', 'LLMs', 'programming', 'reading', 'writing'];

  function isFormValid() {
      return firstName.trim() !== '' && lastName.trim() !== '' && email.trim() !== '';
  }

  function addInterest(interest) {
      if (interest.trim() && !interests.includes(interest.trim())) {
          interests = [...interests, interest.trim()];
          customInterest = ''; // Clear custom input after adding
      }
  }

  function removeInterest(index) {
      interests = interests.filter((_, i) => i !== index);
  }

</script>

<div class="basic-info">
  <h2>Welcome! Let's get started</h2>
  <p>First, tell us a bit about yourself.</p>

  <div class="form-group">
      <label class="form-label" for="firstName">First Name</label>
      <input
          type="text"
          id="firstName"
          class="form-input"
          bind:value={firstName}
          on:input={() => updateField('firstName', firstName)}
          class:error={showErrors && firstName.trim() === ''}
      />
      {#if showErrors && firstName.trim() === ''}
          <p class="error-text">First name is required</p>
      {/if}
  </div>

  <div class="form-group">
      <label class="form-label" for="lastName">Last Name</label>
      <input
          type="text"
          id="lastName"
          class="form-input"
          bind:value={lastName}
          on:input={() => updateField('lastName', lastName)}
          class:error={showErrors && lastName.trim() === ''}
      />
      {#if showErrors && lastName.trim() === ''}
          <p class="error-text">Last name is required</p>
      {/if}
  </div>

  <div class="form-group">
      <label class="form-label" for="email">Email</label>
      <input
          type="email"
          id="email"
          class="form-input"
          bind:value={email}
          on:input={() => updateField('email', email)}
          class:error={showErrors && email.trim() === ''}
      />
      {#if showErrors && email.trim() === ''}
          <p class="error-text">Email is required</p>
      {/if}
  </div>

  <div class="form-group">
      <label class="form-label" for="interests">Interests and Hobbies</label>
      <div class="interests-container">
          <div class="interests-input-container">
              <input
                  type="text"
                  id="interests"
                  class="form-input"
                  bind:value={customInterest}
                  placeholder="Enter custom interest"
                  on:keydown={(e) => { if (e.key === 'Enter') addInterest(customInterest); }}
              />
              <button class="add-interest-btn" on:click={() => addInterest(customInterest)}>Add</button>
          </div>
          <div class="selected-interests">
              {#each interests as interest, index}
                  <span class="interest-tag">
                      {interest}
                      <button class="remove-interest-btn" on:click={() => removeInterest(index)}>x</button>
                  </span>
              {/each}
          </div>
          <div class="predefined-interests">
              {#each predefinedInterests as interest}
                  <button class="predefined-interest-btn" on:click={() => addInterest(interest)}>{interest}</button>
              {/each}
          </div>
      </div>
  </div>

  <div class="btn-container">
      <div></div>
      <button class="btn btn-primary" on:click={handleNext}>Next</button>
  </div>
</div>

<style>
  h2 {
      color: #212529;
      margin-bottom: 0.5rem;
  }

  p {
      color: #6c757d;
      margin-bottom: 2rem;
  }

  .error {
      border-color: #dc3545 !important;
  }

  .error-text {
      color: #dc3545;
      font-size: 0.875rem;
      margin-top: 0.25rem;
      margin-bottom: 0;
  }

  .interests-container {
      display: flex;
      flex-direction: column;
      gap: 0.5rem;
  }

  .interests-input-container {
      display: flex;
      gap: 0.5rem;
  }

  .selected-interests {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
  }

  .interest-tag {
      background-color: #e9ecef;
      padding: 0.25rem 0.5rem;
      border-radius: 0.25rem;
      display: flex;
      align-items: center;
      gap: 0.25rem;
  }

  .remove-interest-btn {
      background: none;
      border: none;
      cursor: pointer;
      color: #6c757d;
      font-size: 0.8rem;
  }

  .predefined-interests {
      display: flex;
      flex-wrap: wrap;
      gap: 0.5rem;
  }

  .predefined-interest-btn {
      background-color: #f8f9fa;
      border: 1px solid #dee2e6;
      padding: 0.25rem 0.5rem;
      border-radius: 0.25rem;
      cursor: pointer;
  }

  .add-interest-btn {
      background-color: #007bff;
      color: white;
      border: none;
      padding: 0.25rem 0.5rem;
      border-radius: 0.25rem;
      cursor: pointer;
  }
</style>
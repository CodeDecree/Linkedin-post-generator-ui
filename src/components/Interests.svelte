<script>
    import { createEventDispatcher } from 'svelte';
    
    export let selectedInterests = [];
    
    const dispatch = createEventDispatcher();
    
    const interestCategories = [
      { id: 'reading', label: 'Reading' },
      { id: 'writing', label: 'Writing' },
      { id: 'sports', label: 'Sports' },
      { id: 'arts', label: 'Creative Arts' },
      { id: 'tech', label: 'Technology' },
      { id: 'travel', label: 'Travel' },
      { id: 'cooking', label: 'Cooking' },
      { id: 'music', label: 'Music' },
      { id: 'movies', label: 'Movies & TV' },
      { id: 'gaming', label: 'Gaming' },
      { id: 'outdoors', label: 'Outdoor Activities' },
      { id: 'health', label: 'Health & Fitness' }
    ];
    
    function toggleInterest(id) {
      if (selectedInterests.includes(id)) {
        selectedInterests = selectedInterests.filter(i => i !== id);
      } else {
        selectedInterests = [...selectedInterests, id];
      }
      dispatch('update', { field: 'interests', value: selectedInterests });
    }
    
    function handleNext() {
      dispatch('next');
    }
    
    function handlePrev() {
      dispatch('prev');
    }
  </script>
  
  <div class="interests">
    <h2>Your Interests & Hobbies</h2>
    <p>Select all that apply to you.</p>
    
    <div class="interests-grid">
      {#each interestCategories as interest}
        <div 
          class="interest-item {selectedInterests.includes(interest.id) ? 'selected' : ''}" 
          on:click={() => toggleInterest(interest.id)}
        >
          {interest.label}
        </div>
      {/each}
    </div>
    
    <div class="btn-container">
      <button class="btn btn-secondary" on:click={handlePrev}>Back</button>
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
    
    .interests-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1rem;
      margin-bottom: 2rem;
    }
    
    .interest-item {
      padding: 1rem;
      border: 1px solid #dee2e6;
      border-radius: 6px;
      text-align: center;
      cursor: pointer;
      transition: all 0.2s ease;
    }
    
    .interest-item:hover {
      border-color: #adb5bd;
    }
    
    .interest-item.selected {
      background-color: #e8f0fe;
      border-color: #5469d4;
      color: #5469d4;
      font-weight: 500;
    }
    
    @media (max-width: 576px) {
      .interests-grid {
        grid-template-columns: repeat(2, 1fr);
      }
    }
  </style>
  
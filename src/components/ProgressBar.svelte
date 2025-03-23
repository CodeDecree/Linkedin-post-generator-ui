<script>
    export let currentStep;
    export let totalSteps;
  </script>
  
  <div class="progress-container">
    <div class="steps">
      {#each Array(totalSteps) as _, i}
        <div class="step {i + 1 <= currentStep ? 'active' : ''}">
          <div class="step-number">{i + 1}</div>
          <div class="step-line" class:hidden={i === totalSteps - 1}></div>
        </div>
      {/each}
    </div>
  </div>
  
  <style>
    .progress-container {
      padding: 1.5rem;
      background-color: #5469d4;
    }
    
    .steps {
      display: flex;
      justify-content: space-between;
      align-items: center;
      position: relative;
    }
    
    .step {
      display: flex;
      align-items: center;
      z-index: 1;
    }
    
    .step-number {
      width: 32px;
      height: 32px;
      border-radius: 50%;
      background-color: rgba(255, 255, 255, 0.2);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      font-weight: bold;
      transition: all 0.3s ease;
    }
    
    .step.active .step-number {
      background-color: white;
      color: #5469d4;
    }
    
    .step-line {
      width: 100%;
      height: 2px;
      background-color: rgba(255, 255, 255, 0.2);
      margin: 0 0.5rem;
    }
    
    .hidden {
      visibility: hidden;
    }
  </style>
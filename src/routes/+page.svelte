<script>
  import { goto } from "$app/navigation";

  function goToOnboarding() {
    goto("/onboarding");
  }
</script>

<svelte:head>
  <title>Automated LinkedIn Posting Agent</title>
  <meta
    name="description"
    content="Automate your LinkedIn posts with personalized content based on your interests, writing style, and schedule."
  />
  <link
    href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap"
    rel="stylesheet"
  />
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css"
    integrity="sha512-9usAa10IRO0HhonpyAIVpjrylPvoDwiPUiKdWk5t3PyolY1cOd4DSE0Ga+ri4AuTroPR5aQvXU9xC6qOPnzFeg=="
    crossorigin="anonymous"
    referrerpolicy="no-referrer"
  />
</svelte:head>

<div class="landing-container">
  <!-- Left Side: Boasting Headings and Bullet Points -->
  <div class="left-content">
    <h1 class="typewriter" style="--delay: 0.5s;">
      Automated LinkedIn Posting Agent
    </h1>
    <p class="typewriter" style="--delay: 1s;">
      Revolutionize your LinkedIn presence with automation.
    </p>
    <ul>
      <li class="typewriter" style="--delay: 1.5s;">
        Tailored content based on your interests and hobbies
      </li>
      <li class="typewriter" style="--delay: 2s;">
        Customizable writing style and mood
      </li>
      <li class="typewriter" style="--delay: 2.5s;">
        Scheduled posts at your preferred times
      </li>
      <li class="typewriter" style="--delay: 3s;">
        Easy integration with LinkedIn
      </li>
    </ul>
  </div>

  <!-- Right Side: Sign In / Sign Up Button -->
  <div class="right-content">
    <button class="linkedin-button fade-in" style="--delay: 3.5s;" on:click={goToOnboarding}>
      Get Started!
    </button>
  </div>
</div>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    font-family: "Poppins", sans-serif;
    background: #f5f5f5;
  }

  .landing-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 100vh;
    padding: 0 50px;
    box-sizing: border-box;
  }

  .left-content {
    flex: 1;
    width: 50vw;
    padding-right: 20px;
  }

  .left-content h1 {
    font-size: 3rem;
    margin-bottom: 20px;
    margin-right: 20px;
  }

  .left-content p {
    font-size: 1.5rem;
    margin-bottom: 20px;
    padding-right: 20px;
  }

  .left-content ul {
    list-style: none;
    padding: 0;
  }

  .left-content li {
    font-size: 1.2rem;
    margin-bottom: 10px;
    position: relative;
    padding-left: 20px;
  }

  .left-content li:before {
    content: "•";
    position: absolute;
    left: 0;
    color: #0077b5;
  }

  .right-content {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 50vw;
    height: 100%;
    background-color: #f0f0f0;
  }

  .linkedin-button {
    background: #0077b5;
    color: #fff;
    border: none;
    padding: 15px 25px;
    font-size: 1.2rem;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s ease;
    display: flex;
    align-items: center;
  }

  .linkedin-button i {
    margin-right: 10px;
  }

  .linkedin-button:hover {
    background: #005983;
  }

  /* Typewriter Animation */
  .typewriter {
    display: inline-block;
    overflow: hidden;
    white-space: nowrap;
    border-right: 0.15em solid #0077b5;
    width: 0;
    opacity: 0;
    animation:
      typing 1s steps(30, end) var(--delay) forwards,
      blink-caret 0.75s step-end calc(var(--delay) + 1s) infinite,
      fadeIn 0.5s var(--delay) forwards,
      removeCaret 0.5s calc(var(--delay) + 4s) forwards;
  }

  /* Fade in for button (and any element with .fade-in) */
  .fade-in {
    opacity: 0;
    animation: fadeIn 1s var(--delay) forwards;
  }

  @keyframes typing {
    from {
      width: 0;
      opacity: 1;
    }
    to {
      width: 100%;
      opacity: 1;
    }
  }

  @keyframes blink-caret {
    from,
    to {
      border-color: transparent;
    }
    50% {
      border-color: #0077b5;
    }
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }

  /* Remove the caret at the end of the animation */
  @keyframes removeCaret {
    from {
      border-right-width: 0.15em;
    }
    to {
      border-right-width: 0;
    }
  }
</style>

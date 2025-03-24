<script>
    let message = '';
    const chatMessages = [
      { daysToGo: 3, schedule: 'Schedule for coming monday.', topic: 'Gen AI' },
      { daysToGo: 7, schedule: 'Follow up on project X.', topic: 'Team Meeting' },
      { daysToGo: 10, schedule: 'Prepare presentation for client Y.', topic: 'Client Presentation' },
      // Add more tile data as needed
    ];
  </script>
  
  <!-- Import Poppins font -->
  <svelte:head>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
  </svelte:head>
  
  <style>
    /* Global styles */
    :global(body) {
      margin: 0;
      font-family: 'Poppins', sans-serif;
      font-size: 16px; /* slightly larger base font size */
      background-color: #f3f6f8;
      color: #333;
    }
    
    .container {
      display: flex;
      height: 100vh; /* Full viewport height */
      background-color: #ffffff;
    }
  
    .left-half {
      flex: 2; /* Twice as wide as right half */
      padding: 20px;
      border-right: 1px solid #eee;
      display: flex;
      flex-direction: column;
    }
  
    .right-half {
      flex: 1;
      padding: 20px;
      overflow-y: auto; /* Enable scrolling if tiles overflow */
      background-color: #f3f6f8;
    }
  
    h2 {
      margin-bottom: 15px;
      color: #0077B5; /* LinkedIn blue */
      font-size: 1.75rem;
    }
  
    .preview-text {
      flex-grow: 1; /* Take up available space */
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 5px;
      margin-bottom: 20px;
      background-color: #ffffff;
      white-space: pre-wrap; /* Preserve line breaks */
      font-size: 1.1rem;
    }
  
    .chat-input-area {
      display: flex;
      align-items: center;
      gap: 10px;
    }
  
    textarea {
      flex-grow: 1;
      padding: 15px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 1.1rem;
      resize: none; /* Prevent manual resizing */
    }
  
    textarea::placeholder {
      color: #999;
    }
  
    .file-attach-button {
      padding: 12px 18px;
      border: 1px solid #ccc;
      border-radius: 5px;
      background-color: #0077B5; /* LinkedIn blue */
      color: #fff;
      cursor: pointer;
      font-size: 1.1rem;
      transition: background-color 0.3s ease;
    }
  
    .file-attach-button:hover {
      background-color: #005f8d;
    }
  
    .tile {
      background-color: #dbe9f4; /* Pastel blue background */
      border: 1px solid #b7d4e6;
      border-radius: 8px;
      padding: 20px;
      margin-bottom: 15px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
      font-size: 1.1rem;
    }
  
    .tile h3 {
      margin-top: 0;
      margin-bottom: 5px;
      color: #0077B5;
      font-size: 1.4rem;
    }
  
    .tile p {
      margin-bottom: 8px;
      color: #555;
    }
  
    .tile .topic {
      color: #0077B5;
      font-weight: 600;
      font-size: 1.1rem;
    }
  </style>
  
  <div class="container">
    <div class="left-half">
      <h2>Preview</h2>
      <div class="preview-text">
        This is some bunch of random text for now. It can be anything you want to display or preview.
        Feel free to replace this with actual content. This area will likely show a preview of something based on the right side.
        Maybe it's a generated schedule, a document, or some other information.
      </div>
      <div class="chat-input-area">
        <textarea placeholder="Some changes? Or need something Fresh." bind:value={message}></textarea>
        <button class="file-attach-button">Attach File</button>
      </div>
    </div>
    <div class="right-half">
      {#each chatMessages as tileData}
        <div class="tile">
          <h3>{tileData.daysToGo} days to go</h3>
          <p>{tileData.schedule}</p>
          <p class="topic">Topic: {tileData.topic}</p>
        </div>
      {/each}
    </div>
  </div>
  
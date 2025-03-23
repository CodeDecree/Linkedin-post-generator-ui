<script>
    import { createEventDispatcher } from 'svelte';
    
    export let writingStyle = {
      communication: '',
      tone: '',
      sentenceLength: '',
      structure: '',
      examples: ''
    };
    
    const dispatch = createEventDispatcher();
    
    const questions = [
      {
        id: 'communication',
        question: 'When communicating professionally, do you prefer:',
        options: [
          { value: 'brief', label: 'Brief,
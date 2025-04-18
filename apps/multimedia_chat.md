---
layout: page
title: Multimedia chat
---

Sample applications making use of a multimodal (vision) model
(i.e. Mistral small): chat (traditional), translation, rewriting tool,
optical character recognition.

<div class="responsive-container">
  <script
    type="module"
    src="https://gradio.s3-us-west-2.amazonaws.com/5.22.0/gradio.js"
  ></script>
  <gradio-app src="https://didier-vision-language-mistral-small.hf.space"></gradio-app>
</div>

<style>
  .responsive-container {
    width: 100%;
    max-width: 1200px; /* Optional: set a maximum width */
    margin: 0 auto;
  }
  
  gradio-app {
    width: 100% !important;
    height: auto !important;
    min-height: 500px; /* Set a minimum height */
  }
</style>

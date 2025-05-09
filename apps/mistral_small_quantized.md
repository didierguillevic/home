---
layout: page
title: Mistral Small (4 bit weights)
---

Vision language model (Mistral Small 3.1) using 4 bit weight quantization.

<div class="responsive-container">
    <script
        type="module"
        src="https://gradio.s3-us-west-2.amazonaws.com/5.29.0/gradio.js"
    ></script>
    <gradio-app src="https://didier-mistral-small-autoround.hf.space"></gradio-app>
</div>

<style>
  .responsive-container {
    width: 100%;
    max-width: 1800px; /* Optional: set a maximum width */
    margin: 0 auto;
  }
  
  gradio-app {
    width: 100% !important;
    height: auto !important;
    min-height: 500px; /* Set a minimum height */
  }
</style>
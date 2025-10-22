---
layout: page
title: 
---

<div class="responsive-container">
    <script
	    type="module"
	    src="https://gradio.s3-us-west-2.amazonaws.com/5.49.1/gradio.js">
    </script>
    <gradio-app src="https://didier-learning-google-agent-development-kit.hf.space"></gradio-app>
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
---
layout: page
title: Upload
permalink: /upload/
nav: true
nav_order: 5
---

<h1>🕒 Current Time</h1>
<p id="time">Loading time...</p>

<h2>📤 Upload an Image</h2>
<form id="upload-form" enctype="multipart/form-data">
  <input type="file" name="file" id="file-input" accept="image/*">
  <button type="submit">Upload</button>
</form>
<p id="upload-status">Waiting for upload feature...</p>

<script>
fetch("https://arshakrz-api-zohoor.hf.space/")
  .then(res => res.json())
  .then(data => {
    document.getElementById("time").textContent = "⏰ Time: " + data.time;
  })
  .catch(() => {
    document.getElementById("time").textContent = "❌ Failed to load time.";
  });
</script>

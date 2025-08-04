---
layout: page
title: upload
permalink: /upload/
nav: true
nav_order: 5
---

<h1>🕒 Current Time</h1>
<p id="time">Loading time...</p>

<h2>📤 Upload an Image</h2>
<form id="upload-form" enctype="multipart/form-data">
  <input type="file" name="file" id="file-input" accept="image/*" required>
  <button type="submit">Upload</button>
</form>
<p id="upload-status">Waiting for upload...</p>

<script>
const apiBase = "https://arshakrz-simple-api-arshak.hf.space";  // ← Update if renamed

// Load current time
fetch(`${apiBase}/`)
  .then(res => res.json())
  .then(data => {
    document.getElementById("time").textContent = "⏰ Time: " + data.time;
  })
  .catch(() => {
    document.getElementById("time").textContent = "❌ Failed to load time.";
  });

// Upload image
document.getElementById("upload-form").addEventListener("submit", async (e) => {
  e.preventDefault();
  const fileInput = document.getElementById("file-input");
  const formData = new FormData();
  formData.append("file", fileInput.files[0]);

  document.getElementById("upload-status").textContent = "⏳ Uploading...";

  try {
    const res = await fetch(`${apiBase}/upload`, {
      method: "POST",
      body: formData
    });
    const data = await res.json();
    document.getElementById("upload-status").textContent = "✅ " + data.message;
  } catch {
    document.getElementById("upload-status").textContent = "❌ Upload failed.";
  }
});
</script>

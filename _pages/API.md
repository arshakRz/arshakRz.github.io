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
  <input type="file" name="file" id="file-input" accept="image/*" required>
  <button type="submit">Upload</button>
</form>

<p id="upload-status">Waiting for upload...</p>

<h2>🖼️ Processed Image</h2>
<img id="output-image" style="max-width: 100%; border: 1px solid #ccc; display: none;">

<script>
// === Config ===
const apiBase = "https://arshakrz-simple-api-arshak.hf.space";  // Update if renamed

// === Load current time from API ===
fetch(`${apiBase}/`)
  .then(res => res.json())
  .then(data => {
    document.getElementById("time").textContent = "⏰ Time: " + data.time;
  })
  .catch(() => {
    document.getElementById("time").textContent = "❌ Failed to load time.";
  });

// === Handle image upload ===
document.getElementById("upload-form").addEventListener("submit", async (e) => {
  e.preventDefault();
  const fileInput = document.getElementById("file-input");
  const formData = new FormData();
  formData.append("file", fileInput.files[0]);

  document.getElementById("upload-status").textContent = "⏳ Uploading...";
  document.getElementById("output-image").style.display = "none";

  try {
    const res = await fetch(`${apiBase}/upload`, {
      method: "POST",
      body: formData
    });

    if (!res.ok) throw new Error("Image processing failed");

    const blob = await res.blob();
    const imageURL = URL.createObjectURL(blob);

    document.getElementById("upload-status").textContent = "✅ Edge detection complete";
    const img = document.getElementById("output-image");
    img.src = imageURL;
    img.style.display = "block";
  } catch (err) {
    document.getElementById("upload-status").textContent = "❌ Upload failed.";
    console.error(err);
  }
});
</script>

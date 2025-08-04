---
layout: page
title: Fourier Contour Animation
permalink: /fourier/
nav: true
nav_order: 6
---

<style>
  body {
    font-family: Arial, sans-serif;
    margin: 2rem;
  }
  h1, h2 {
    margin-bottom: 0.5rem;
  }
  #images {
    display: flex;
    gap: 2rem;
    margin-top: 1rem;
  }
  #images img, #images video {
    max-width: 300px;
    border: 1px solid #ccc;
  }
</style>

<h1>🕒 Current Time</h1>
<p id="time">🕒 Loading time...</p>

<h2>📤 Upload an Image</h2>
<form id="upload-form" enctype="multipart/form-data">
  <input type="file" name="file" id="file-input" accept="image/*" required>
  <button type="submit">Upload</button>
</form>
<p id="upload-status">Waiting for upload...</p>

<div id="images">
  <div>
    <h3>Original Image</h3>
    <img id="original-img" src="" alt="Original will show here" />
  </div>
  <div>
    <h3>Processed Animation</h3>
    <video id="result-video" controls autoplay loop muted></video>
  </div>
</div>

<script>
  const apiBase = "https://arshakrz-simple-api-arshak.hf.space";

  // Optional time fetch from API
  fetch(`${apiBase}/`)
    .then(res => res.json())
    .then(data => {
      document.getElementById("time").textContent = "⏰ Time: " + data.time;
    })
    .catch(() => {
      document.getElementById("time").textContent = "❌ Failed to load time.";
    });

  document.getElementById("upload-form").addEventListener("submit", async function (e) {
    e.preventDefault();

    const fileInput = document.getElementById("file-input");
    const file = fileInput.files[0];
    const formData = new FormData();
    formData.append("file", file);

    // STEP 1: Show uploaded image immediately
    const reader = new FileReader();
    reader.onload = function (event) {
      document.getElementById("original-img").src = event.target.result;
    };
    reader.readAsDataURL(file);

    // STEP 2: Update status to uploading
    document.getElementById("upload-status").textContent = "📤 Uploading...";

    try {
      // STEP 3: Start fetch and immediately update to processing
      const fetchPromise = fetch(`${apiBase}/upload`, {
        method: "POST",
        body: formData
      });

      document.getElementById("upload-status").textContent = "⚙️ Processing...";

      // STEP 4: Await fetch result
      const response = await fetchPromise;
      const data = await response.json();

      if (!response.ok) throw new Error("Upload failed");

      const videoEl = document.getElementById("result-video");
      if (data.video_base64) {
        videoEl.src = data.video_base64;
        videoEl.load();
        document.getElementById("upload-status").textContent = "✅ Done!";
      } else {
        throw new Error("No video in response");
      }
    } catch (err) {
      console.error(err);
      document.getElementById("upload-status").textContent = "❌ Upload failed.";
    }
  });
</script>

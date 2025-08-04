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
  h2 { text-align:center; margin-bottom:1rem; }
  #images {
    display: flex;
    gap: 2rem;
    justify-content: center;
    margin-top: 1rem;
  }
  #images img, #images video {
    max-width: 300px;
    border: 1px solid #ccc;
    background:#eee;
    object-fit: contain;
  }
</style>

<h2>📤 Upload an image with a <em>clear, detectable edge</em> for best results</h2>

<form id="upload-form" enctype="multipart/form-data" style="text-align:center">
  <input type="file" name="file" id="file-input" accept="image/*" required>
  <button type="submit">Upload</button>
</form>
<p id="upload-status" style="text-align:center">Waiting for upload…</p>

<div id="images">
  <div>
    <h3 style="text-align:center">Original</h3>
    <!-- default placeholder -->
    <img id="original-img" src="/assets/img/pi.jpg" alt="Original image">
  </div>
  <div>
    <h3 style="text-align:center">Animation</h3>
    <!-- default placeholder -->
    <video id="result-video" src="/assets/video/pi.mp4" controls autoplay loop muted></video>
  </div>
</div>

<script>
  const apiBase = "https://arshakrz-simple-api-arshak.hf.space";

  document
    .getElementById("upload-form")
    .addEventListener("submit", async function (e) {
      e.preventDefault();

      const fileInput = document.getElementById("file-input");
      const file = fileInput.files[0];
      const formData = new FormData();
      formData.append("file", file);

      /* show selected image immediately */
      const reader = new FileReader();
      reader.onload = (ev) =>
        (document.getElementById("original-img").src = ev.target.result);
      reader.readAsDataURL(file);

      document.getElementById("upload-status").textContent = "📤 Uploading…";

      try {
        const uploading = fetch(`${apiBase}/upload`, {
          method: "POST",
          body: formData,
        });

        document.getElementById("upload-status").textContent = "⚙️ Processing…";

        const resp = await uploading;
        const data = await resp.json();

        if (!resp.ok || !data.video_base64)
          throw new Error(data.error || "Unexpected response");

        const vid = document.getElementById("result-video");
        vid.src = data.video_base64;
        vid.load();

        document.getElementById("upload-status").textContent = "✅ Done!";
      } catch (err) {
        console.error(err);
        document.getElementById("upload-status").textContent = "❌ Upload failed.";
      }
    });
</script>

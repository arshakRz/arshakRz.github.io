<h2>📤 upload an Image</h2>
<form id="upload-form" enctype="multipart/form-data">
  <input type="file" name="file" id="file-input" accept="image/*" required>
  <button type="submit">Upload</button>
</form>

<p id="upload-status">Waiting for upload...</p>

<div style="display: flex; flex-wrap: wrap; gap: 20px; margin-top: 20px;">
  <div>
    <h3 style="text-align: center;">Original</h3>
    <img id="original-image"
         style="width: 250px; height: auto; border: 1px solid #ccc; display: none;">
  </div>
  <div>
    <h3 style="text-align: center;">Edge Detected</h3>
    <img id="output-image"
         style="width: 250px; height: auto; border: 1px solid #ccc; display: none;">
  </div>
</div>

<script>
const apiBase = "https://arshakrz-simple-api-arshak.hf.space";

// Load time
fetch(`${apiBase}/`)
  .then(res => res.json())
  .then(data => {
    document.getElementById("time").textContent = "⏰ Time: " + data.time;
  })
  .catch(() => {
    document.getElementById("time").textContent = "❌ Failed to load time.";
  });

// Handle upload
document.getElementById("upload-form").addEventListener("submit", async (e) => {
  e.preventDefault();
  const fileInput = document.getElementById("file-input");
  const file = fileInput.files[0];

  const formData = new FormData();
  formData.append("file", file);

  // Show original preview
  const originalURL = URL.createObjectURL(file);
  const originalImage = document.getElementById("original-image");
  originalImage.src = originalURL;
  originalImage.style.display = "block";

  document.getElementById("upload-status").textContent = "⏳ Uploading...";
  document.getElementById("output-image").style.display = "none";

  try {
    const res = await fetch(`${apiBase}/upload`, {
      method: "POST",
      body: formData
    });

    if (!res.ok) throw new Error("Processing failed");

    const blob = await res.blob();
    const outputURL = URL.createObjectURL(blob);

    const outputImage = document.getElementById("output-image");
    outputImage.src = outputURL;
    outputImage.style.display = "block";

    document.getElementById("upload-status").textContent = "✅ Edge detection complete";
  } catch (err) {
    document.getElementById("upload-status").textContent = "❌ Upload failed.";
    console.error(err);
  }
});
</script>

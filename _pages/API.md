---
layout: page
title: API
permalink: /API/
nav: true
nav_order: 10
---

<h1>🕌 My API</h1>
<p id="date">Loading date...</p>
<p id="zohoor">Checking status...</p>

<script>
  fetch("https://arshakrz-api-zohoor.hf.space/")
    .then(response => response.json())
    .then(data => {
      document.getElementById("date").textContent = "📅 Date: " + data.date;
    })
    .catch(error => {
      document.getElementById("zohoor").textContent = "⚠️ Failed to load data.";
      console.error("Error:", error);
    });
</script>

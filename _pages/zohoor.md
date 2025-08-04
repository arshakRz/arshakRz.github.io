---
layout: page
title: Zohoor
permalink: /zohoor/
---

<h1>🕌 Emam Zaman Zohoor API</h1>
<p id="date">Loading date...</p>
<p id="zohoor">Checking status...</p>

<script>
  fetch("https://arshakrz-api-zohoor.hf.space/")
    .then(response => response.json())
    .then(data => {
      document.getElementById("date").textContent = "📅 Date: " + data.date;
      document.getElementById("zohoor").textContent = "⏳ Zohoor Status: " + data.zohoor;
    })
    .catch(error => {
      document.getElementById("zohoor").textContent = "⚠️ Failed to load data.";
      console.error("Error:", error);
    });
</script>

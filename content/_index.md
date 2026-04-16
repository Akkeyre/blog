---
title: "Akkeyre"
---

3rd year undergrad at IIT-BHU.

This is where I keep my notes and personal ramblings.

<p>
  <img id="comic" class="comic" data-count="2x">
</p>

<script>
  const img = document.getElementById("comic");
  const n = parseInt(img.dataset.count, 3);

  const idx = Math.floor(Math.random() * n) + 1;
  img.src = `/images/comic${idx}.png`;
</script>

---
layout: page
title: A Special Question for Sara
permalink: /meow/
nav: true
nav_order: 7
---

<style>
  body {
    font-family: Arial, sans-serif;
    margin: 2rem;
  }
  
  .proposal-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 2rem;
    text-align: center;
  }

  #proposal-img {
    max-width: 400px;
    width: 90%;
    border-radius: 15px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.2);
    margin-bottom: 2.5rem;
  }

  .button-container {
    display: flex;
    gap: 2rem;
    align-items: center;
    justify-content: center;
  }

  .btn {
    padding: 15px 40px;
    font-size: 1.5rem;
    border: none;
    border-radius: 30px;
    cursor: pointer;
    font-weight: bold;
    transition: transform 0.2s ease;
  }

  #yes-btn {
    background-color: #ff4d6d;
    color: white;
    box-shadow: 0 4px 15px rgba(255, 77, 109, 0.4);
  }

  #yes-btn:hover {
    transform: scale(1.1);
  }

  #no-btn {
    background-color: #e9ecef;
    color: #495057;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    z-index: 1000; /* Keeps the button above other elements when it moves */
  }
</style>

<div class="proposal-container">
  <img id="proposal-img" src="/assets/img/lead1_fe2b51.webp" alt="Will you meow me?">

  <div class="button-container">
    <button id="yes-btn" class="btn">Yes</button>
    <button id="no-btn" class="btn">No</button>
  </div>
</div>

<script>
  const yesBtn = document.getElementById('yes-btn');
  const noBtn = document.getElementById('no-btn');

  // 1. Pop up when she clicks 'Yes'
  yesBtn.addEventListener('click', () => {
    // You can customize this message!
    alert("Yay! ❤️ I can't wait!");
  });

  // 2. Make the 'No' button move away on hover or tap
  const moveNoButton = () => {
    // Switch to fixed positioning on the first interaction so it can travel the whole screen
    noBtn.style.position = 'fixed';

    // Calculate maximum bounds so the button doesn't go off-screen
    const maxX = window.innerWidth - noBtn.offsetWidth;
    const maxY = window.innerHeight - noBtn.offsetHeight;

    // Generate random coordinates
    const randomX = Math.floor(Math.random() * maxX);
    const randomY = Math.floor(Math.random() * maxY);

    // Apply the new random coordinates
    noBtn.style.left = randomX + 'px';
    noBtn.style.top = randomY + 'px';
  };

  // Triggers for computers (mouse movement)
  noBtn.addEventListener('mouseover', moveNoButton);
  
  // Triggers for phones (touching the screen)
  noBtn.addEventListener('touchstart', (e) => {
    e.preventDefault(); // Prevents a click from registering before the button moves
    moveNoButton();
  });
</script>

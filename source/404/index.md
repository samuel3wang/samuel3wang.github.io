---
title: 404
date: 2023-09-24 17:29:14
comments: false
permalink: /404.html
---

<!-- markdownlint-disable MD039 MD033 -->

## This is an error page

Welcome to 404 Page.

It will back to Home Page about <span id="timeout">5</span> seconds.

You can also Click **[HERE](https://samuel3wang.github.io/)** to back to Home Page immediately.

<script>
let countTime = 5;

function count() {
  
  document.getElementById('timeout').textContent = countTime;
  countTime -= 1;
  if(countTime === 0){
    location.href = 'https://samuel3wang.github.io';
  }
  setTimeout(() => {
    count();
  }, 1000);
}

count();
</script>
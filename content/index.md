---
title: Home
---

<!-- 动态打字效果 -->
<div style="margin-top: -2rem; margin-bottom: -5rem;">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&pause=600&width=420&lines=Hello!+Privet!;Bonjour!+こんにちは!&size=27" alt="Typing SVG" style="height: 5.2rem; width: auto; vertical-align: middle;" />
</div>

# Welcome

这是一个基于 Obsidian 和 Quartz 构建的[个人知识库](https://anthony-hcy.github.io/Study/)。
<!-- 每日一言（Hitokoto） -->
<div id="hitokoto" style="margin: 0.5rem 0 -2rem 0; color: var(--darkgray);">
  加载中...
</div>
<script>
  fetch('https://v1.hitokoto.cn/?c=a&c=b&c=d&c=h&c=j')
    .then(response => response.json())
    .then(data => {
      const container = document.getElementById('hitokoto');
      const sentence = data.hitokoto;
      const from = data.from ? `—— ${data.from}` : '';
      
      if (from) {
        container.innerHTML = `
          <div style="font-size: 1.2rem; margin-bottom: 0.1rem;">“${sentence}”</div>
          <div style="text-align: right; font-size: 0.95rem; color: var(--darkgray); padding-right: 6rem;">${from}</div>
        `;
      } else {
        container.innerHTML = `<div style="font-size: 1.2rem;">“${sentence}”</div>`;
      }
    })
    .catch(() => {
      document.getElementById('hitokoto').innerHTML = '今日一言加载失败，请稍后刷新。';
    });
</script>

# Contents

- [[Python/01 Python3 教程|Python 教程]]
- [[ROS2/ROS2 零基础入门到进阶完整学习指南|ROS2 学习指南]]
- [[Markdown/Markdown-1|Markdown 语法]]
<div style="display: flex; gap: 1rem; flex-wrap: wrap; margin-top: 0.5rem;">
  <img src="https://github.com/Anthony-hcy/Anthony-hcy/blob/main/images/image-1.png?raw=true" alt="左图" style="flex: 1; min-width: 200px; border-radius: 8px;" />
  <img src="https://github.com/Anthony-hcy/Anthony-hcy/blob/main/images/image-3.jpg?raw=true" alt="右图" style="flex: 1; min-width: 200px; border-radius: 8px;" />
</div>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Anthony-hcy/Anthony-hcy/output/github-contribution-grid-snake-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Anthony-hcy/Anthony-hcy/output/github-contribution-grid-snake.svg">
  <img alt="github contribution grid snake animation" src="https://raw.githubusercontent.com/Anthony-hcy/Anthony-hcy/output/github-contribution-grid-snake.svg">
</picture>
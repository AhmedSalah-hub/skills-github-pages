---
layout: default
title: Home
---

<div class="hero-container">
  <span class="hero-pill">🚀 Software Engineer & .NET Developer</span>
  <h1 class="hero-title">Building Scalable Backend Systems & Clean Architectures.</h1>
  <p class="hero-subtitle">
    Hi, I'm <strong>Ahmed Salah</strong>. Welcome to my engineering blog. I build robust data-driven applications with <strong>C#</strong>, <strong>.NET 9</strong>, <strong>Entity Framework Core</strong>, and <strong>SQL Server</strong>. Here I share technical guides, architecture patterns, and open-source projects.
  </p>
  <div class="hero-actions">
    <a href="{{ '/projects' | relative_url }}" class="btn-primary">Explore Projects →</a>
    <a href="https://github.com/AhmedSalah-hub" class="btn-secondary" target="_blank" rel="noopener">GitHub Profile ↗</a>
    <a href="{{ '/about' | relative_url }}" class="btn-secondary">About Me</a>
  </div>
</div>

<hr style="border: 0; border-top: 1px solid var(--border-color); margin: 2.5rem 0;" />

## 🌟 Featured Projects

<div class="projects-grid">
  <div class="project-card">
    <div>
      <h3><a href="https://github.com/AhmedSalah-hub/bikestore" target="_blank" rel="noopener">BikeStore Platform</a></h3>
      <p>Enterprise-grade e-commerce & inventory data layer implementing advanced ORM relationships, repository patterns, and high-performance querying.</p>
      <div class="project-tags">
        <span class="tech-tag">.NET 9</span>
        <span class="tech-tag">EF Core 9</span>
        <span class="tech-tag">SQL Server</span>
        <span class="tech-tag">LINQ</span>
      </div>
    </div>
    <a href="https://github.com/AhmedSalah-hub/bikestore" class="project-link" target="_blank" rel="noopener">View Repository →</a>
  </div>

  <div class="project-card">
    <div>
      <h3><a href="https://github.com/AhmedSalah-hub/ExamManagementSystem" target="_blank" rel="noopener">Exam Management System</a></h3>
      <p>Dynamic OOP examination engine featuring question banks, automated answer evaluation, difficulty tiers, and instant grading telemetry.</p>
      <div class="project-tags">
        <span class="tech-tag">C#</span>
        <span class="tech-tag">OOP</span>
        <span class="tech-tag">Console Engine</span>
      </div>
    </div>
    <a href="https://github.com/AhmedSalah-hub/ExamManagementSystem" class="project-link" target="_blank" rel="noopener">View Repository →</a>
  </div>

  <div class="project-card">
    <div>
      <h3><a href="https://github.com/AhmedSalah-hub/Student_Management_System" target="_blank" rel="noopener">Student Administration System</a></h3>
      <p>Modular academic records management system designed with strict object-oriented modeling and structured data persistence.</p>
      <div class="project-tags">
        <span class="tech-tag">C#</span>
        <span class="tech-tag">Data Structures</span>
        <span class="tech-tag">Management</span>
      </div>
    </div>
    <a href="https://github.com/AhmedSalah-hub/Student_Management_System" class="project-link" target="_blank" rel="noopener">View Repository →</a>
  </div>
</div>

<hr style="border: 0; border-top: 1px solid var(--border-color); margin: 3rem 0 2rem;" />

## 📝 Recent Articles

<ul class="post-list">
  {% for post in site.posts %}
    <li class="post-item">
      <div class="post-meta">{{ post.date | date: "%b %d, %Y" }}</div>
      <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <p class="post-excerpt">{{ post.excerpt | strip_html | truncatewords: 28 }}</p>
    </li>
  {% endfor %}
</ul>

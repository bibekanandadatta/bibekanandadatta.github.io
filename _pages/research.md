---
layout: page
title: Research
permalink: /research/
description: Projects that I worked on or working on now
nav: true
nav_order: 2
display_categories: [Current, Past]
horizontal: false
---

My current research focuses primarily on the multiphysics modeling of stimuli-responsive hydrogels. I use continuum scale models and coupled finite element method to understand the chemo-mechanical response of hydrogels subjected to temperature, pH, and ionic solution like environmental stimuli. Most of my research involves developing multi-physics frameworks and constitutive models, and eventually implementing them in finite element programs to perform simulations. Besides developing a fundamental understanding of a wide class of hydrogel materials, I also focus on exploiting such stimuli-responsive behaviors to develop devices that can be used in soft sensing and actuation for biomedical applications. 

I will update the pages for individual projects soon (I promise)!

<!-- pages/projects.md -->
<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="grid">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>

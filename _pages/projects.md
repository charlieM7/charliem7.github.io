---
layout: page
title: Projects
permalink: /projects/
description: 
nav: true
nav_order: 3
---

<!-- pages/projects.md -->
<div class="projects">
  {% assign all_projects = site.projects | sort: "importance" %}
  
  {% comment %} Separate projects with years (from year field or date) and without {% endcomment %}
  {% assign projects_with_year = '' | split: ',' %}
  {% assign projects_without_year = '' | split: ',' %}
  
  {% for project in all_projects %}
    {% if project.year and project.year != '' %}
      {% assign projects_with_year = projects_with_year | push: project %}
    {% elsif project.date %}
      {% assign project_year = project.date | date: "%Y" %}
      {% if project_year and project_year != '' %}
        {% assign projects_with_year = projects_with_year | push: project %}
      {% else %}
        {% assign projects_without_year = projects_without_year | push: project %}
      {% endif %}
    {% else %}
      {% assign projects_without_year = projects_without_year | push: project %}
    {% endif %}
  {% endfor %}
  
  {% if projects_with_year.size > 0 %}
    {% comment %} Group by year {% endcomment %}
    {% assign year_map = '' | split: ',' %}
    {% assign unique_years = '' | split: ',' %}
    
    {% for project in projects_with_year %}
      {% if project.year and project.year != '' %}
        {% assign current_year = project.year %}
      {% else %}
        {% assign current_year = project.date | date: "%Y" %}
      {% endif %}
      
      {% unless unique_years contains current_year %}
        {% assign unique_years = unique_years | push: current_year %}
      {% endunless %}
    {% endfor %}
    
    {% assign sorted_years = unique_years | sort | reverse %}
    
    {% for year in sorted_years %}
      <h2 class="project-year">{{ year }}</h2>
      <div class="project-list">
        {% for project in projects_with_year %}
          {% if project.year and project.year != '' %}
            {% assign project_year = project.year %}
          {% else %}
            {% assign project_year = project.date | date: "%Y" %}
          {% endif %}
          {% if project_year == year %}
            {% include projects_card.liquid %}
          {% endif %}
        {% endfor %}
      </div>
    {% endfor %}
  {% endif %}
  
  {% if projects_without_year.size > 0 %}
    <div class="project-list">
      {% for project in projects_without_year %}
        {% include projects_card.liquid %}
      {% endfor %}
    </div>
  {% endif %}
</div>

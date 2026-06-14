---
layout: page
title: "Newsletter - Academic Year 2025-2026"
permalink: /posts/2025-2026/
# Specify the starting year of the academic cycle you want to display
academic_start_year: 2025 
---

<div id="top" style="scroll-margin-top: 200px;"></div>

{% comment %} 
  1. CALCULATE EXPLICIT BOUNDARIES BASED ON FRONT MATTER
  If academic_start_year is 2025, it sets June 1, 2025 to March 31, 2026.
{% endcomment %}
{% if page.academic_start_year %}
  {% assign start_yr = page.academic_start_year | plus: 0 %}
  {% assign end_yr = start_yr | plus: 1 %}

  {% assign academic_start_date = start_yr | append: "-06-01" %}
  {% assign academic_end_date = end_yr | append: "-03-31" %}
{% else %}
  {% comment %} Fallback: If no year is specified, show all posts {% endcomment %}
  {% assign academic_start_date = "1970-01-01" %}
  {% assign academic_end_date = "2099-12-31" %}
{% endif %}

<div class="newsletter-container">
  <aside class="toc-sidebar">
    <nav class="toc-card">
      <h2 class="toc-title">Editions</h2>
      <ul class="toc-list">
        {% assign grouped_posts = site.posts | group_by: "category" %}
        {% for group in grouped_posts %}
          {% comment %} Verify if the category has posts in this specific window {% endcomment %}
          {% assign has_current_posts = false %}
          {% for item in group.items %}
            {% assign item_date = item.date | date: "%Y-%m-%d" %}
            {% if item_date >= academic_start_date and item_date <= academic_end_date %}
              {% assign has_current_posts = true %}
              {% break %}
            {% endif %}
          {% endfor %}

          {% if has_current_posts %}
            {% assign category_id = group.name | slugify | default: "general-updates" %}
            <li><a href="#{{ category_id }}">{{ group.name | default: "General Updates" }}</a></li>
          {% endif %}
        {% endfor %}
      </ul>
    </nav>
  </aside>

  <div class="posts-list">
    {% assign total_displayed_posts = 0 %}
    
    {% for group in grouped_posts %}
      {% comment %} Filter items down to the selected June-March window {% endcomment %}
      {% assign academic_items = "" | split: "" %}
      {% for item in group.items %}
        {% assign item_date = item.date | date: "%Y-%m-%d" %}
        {% if item_date >= academic_start_date and item_date <= academic_end_date %}
          {% assign item_array = item | Array %}
          {% assign academic_items = academic_items | concat: item_array %}
        {% endif %}
      {% endfor %}

      {% if academic_items.size > 0 %}
        {% assign category_id = group.name | slugify | default: "general-updates" %}
        {% assign total_displayed_posts = total_displayed_posts | plus: academic_items.size %}
        
        <section class="term-section" id="{{ category_id }}">
          <h2 class="category-heading">
            {{ group.name | default: "General Updates" }}
          </h2>

          {% comment %} Pin handling logic applied to filtered list {% endcomment %}
          {% assign pinned_posts = academic_items | where: "pinned", true %}
          {% assign normal_posts = academic_items | where_exp: "item", "item.pinned != true" %}
          {% assign sorted_posts = pinned_posts | concat: normal_posts %}

          {% for post in sorted_posts %}
            <article class="post-preview">
              <a href="{{ post.url | relative_url }}" style="text-decoration: none;">
                <h3 class="post-title">{{ post.title }}</h3>
                {% if post.subtitle %}
                  <h4 class="post-subtitle">
                    {{ post.subtitle }}
                  </h4>
                {% endif %}
              </a>

              <p class="post-meta">
                Posted on {{ post.date | date: site.date_format }}
              </p>

              <div class="post-entry-container">
                {% if post.image %}
                  <div class="post-image">
                    <a href="{{ post.url | relative_url }}">
                      <img src="{{ post.image | relative_url }}" alt="{{ post.title }}">
                    </a>
                  </div>
                {% endif %}
                
                <div class="post-entry">
                  {{ post.excerpt | strip_html | truncatewords: 30 }}
                  <a href="{{ post.url | relative_url }}" class="post-read-more">[Read&nbsp;More]</a>
                </div>
              </div>
            </article>
          {% endfor %}
          
          <div class="back-to-top">
             <a href="#top">↑ Back to top</a>
          </div>
        </section>
      {% endif %}
    {% endfor %}

    {% if total_displayed_posts == 0 %}
      <p>No newsletters found for the specified academic year.</p>
    {% endif %}
  </div>
</div>

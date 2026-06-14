---
layout: page
title: "Newsletter - Academic Year 2025-2026"
permalink: /posts/2025-2026/
academic_start_year: 2025 
---

<div id="top" style="scroll-margin-top: 200px;"></div>

{% comment %} 
  1. CALCULATE EXPLICIT BOUNDARIES BASED ON FRONT MATTER
{% endcomment %}
{% if page.academic_start_year %}
  {% assign start_yr = page.academic_start_year | plus: 0 %}
  {% assign end_yr = start_yr | plus: 1 %}

  {% assign academic_start_date = start_yr | append: "-06-01" %}
  {% assign academic_end_date = end_yr | append: "-03-31" %}
{% else %}
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
          
          {% comment %} Check if category contains posts within the target year range {% endcomment %}
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
      
      {% comment %} 
        Instead of creating arrays via 'concat', we map group items directly using 
        Liquid's native filters to split pinned vs unpinned posts.
      {% endcomment %}
      {% assign group_pinned = group.items | where: "pinned", true %}
      {% assign group_normal = group.items | where_exp: "item", "item.pinned != true" %}
      
      {% comment %} 
        Count how many posts in this group actually fit the target academic window
      {% endcomment %}
      {% assign current_group_count = 0 %}
      
      {% capture group_output %}
        {% comment %} First loop: Pinned Posts {% endcomment %}
        {% for post in group_pinned %}
          {% assign post_date = post.date | date: "%Y-%m-%d" %}
          {% if post_date >= academic_start_date and post_date <= academic_end_date %}
            {% assign current_group_count = current_group_count | plus: 1 %}
            {% include post_preview_template.html %}
          {% endif %}
        {% endfor %}

        {% comment %} Second loop: Normal Posts {% endcomment %}
        {% for post in group_normal %}
          {% assign post_date = post.date | date: "%Y-%m-%d" %}
          {% if post_date >= academic_start_date and post_date <= academic_end_date %}
            {% assign current_group_count = current_group_count | plus: 1 %}
            {% include post_preview_template.html %}
          {% endif %}
        {% endfor %}
      {% endcapture %}

      {% comment %} Only display the section if posts were found in the timeframe {% endcomment %}
      {% if current_group_count > 0 %}
        {% assign total_displayed_posts = total_displayed_posts | plus: current_group_count %}
        {% assign category_id = group.name | slugify | default: "general-updates" %}
        
        <section class="term-section" id="{{ category_id }}">
          <h2 class="category-heading">
            {{ group.name | default: "General Updates" }}
          </h2>

          {{ group_output }}
          
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

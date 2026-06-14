---
layout: page
title: "Newsletter"
permalink: /posts25/
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
        1. Filter out the pinned posts
        2. Filter out the unpinned posts (remaining ones)
        3. Concatenate them so Pinned is silently first without visual differentiation
      {% endcomment %}
      {% assign pinned_posts = group.items | where: "pinned", true %}
      {% assign normal_posts = group.items | where_exp: "item", "item.pinned != true" %}
      {% assign sorted_posts = pinned_posts | concat: normal_posts %}
      
      {% comment %} 
        Generate post preview outputs while ensuring they fall into the academic window
      {% endcomment %}
      {% assign current_group_count = 0 %}
      
      {% capture group_output %}
        {% for post in sorted_posts %}
          {% assign post_date = post.date | date: "%Y-%m-%d" %}
          {% if post_date >= academic_start_date and post_date <= academic_end_date %}
            {% assign current_group_count = current_group_count | plus: 1 %}
            
            <article class="post-preview">
              <a href="{{ post.url | relative_url }}" style="text-decoration: none;">
                <h3 class="post-title">
                  {{ post.title }}
                </h3>
                {% if post.subtitle %}
                  <h4 class="post-subtitle">
                    {{ post.subtitle }}
                  </h4>
                {% endif %}
              </a>

              <p class="post-meta">
                Posted on {{ post.date | date: site.date_format | default: "%B %d, %Y" }}
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
          {% endif %}
        {% endfor %}
      {% endcapture %}

      {% comment %} Only display the section if valid posts were found in the timeframe {% endcomment %}
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
      <p>No newsletters found for the specified academic year. Check back soon!</p>
    {% endif %}
  </div>
</div>

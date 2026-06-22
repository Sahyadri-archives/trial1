---
layout: page
title: "Newsletter"
permalink: /posts/
---

{% assign year_blocks = "2025-26" | split: "|" %}
{% assign default_active_year = "2025-26" %}

<div id="top" style="scroll-margin-top: 200px;"></div>

<div class="tabs-container">
  <div class="academic-tabs">
    {% for current_year in year_blocks %}
      {% if current_year == "archive" %}
        {% assign tab_id = "ay-archive" %}
        {% assign tab_label = "Archive" %}
      {% else %}
        {% assign start_yr = current_year | plus: 0 %}
        {% assign short_end_yr = start_yr | plus: 1 | slice: 2, 2 %}
        {% assign tab_id = "ay-" | append: start_yr | append: "-" | append: short_end_yr %}
        {% assign tab_label = start_yr | append: "–" | append: short_end_yr %}
      {% endif %}
      
      <button 
        class="tab-link {% if current_year == default_active_year %}active{% endif %}" 
        onclick="switchAcademicYear(event, '{{ tab_id }}')">
        {{ tab_label }}
      </button>
    {% endfor %}
  </div>
</div>

{% assign grouped_posts = site.posts | group_by: "category" %}

{% for current_year in year_blocks %}
  {% if current_year == "archive" %}
    {% assign academic_start_date = "1970-01-01" %}
    {% assign academic_end_date = "2024-05-31" %}
    {% assign panel_id = "ay-archive" %}
  {% else %}
    {% assign start_yr = current_year | plus: 0 %}
    {% assign end_yr = start_yr | plus: 1 %}
    {% assign short_end_yr = end_yr | slice: 2, 2 %}
    
    {% assign academic_start_date = start_yr | append: "-06-01" %}
    {% assign academic_end_date = end_yr | append: "-03-31" %}
    {% assign panel_id = "ay-" | append: start_yr | append: "-" | append: short_end_yr %}
  {% endif %}

  {% assign is_active = false %}
  {% if current_year == default_active_year %}
    {% assign is_active = true %}
  {% endif %}

  <div id="{{ panel_id }}" class="academic-panel {% if is_active %}active{% endif %}">
    <div class="newsletter-container">
      
      <aside class="toc-sidebar">
        <nav class="toc-card">
          <h2 class="toc-title">Editions</h2>
          <ul class="toc-list">
            {% for group in grouped_posts %}
              {% assign has_current_posts = false %}
              {% for item in group.items %}
                {% assign item_date = item.date | date: "%Y-%m-%d" %}
                {% if item_date >= academic_start_date and item_date <= academic_end_date %}
                  {% assign has_current_posts = true %}
                  {% break %}
                {% endif %}
              {% endfor %}

              {% if has_current_posts %}
                {% assign category_id = panel_id | append: "-" | append: group.name | slugify | default: "general-updates" %}
                <li><a href="#{{ category_id }}">{{ group.name | default: "General Updates" }}</a></li>
              {% endif %}
            {% endfor %}
          </ul>
        </nav>
      </aside>

      <div class="posts-list">
        {% assign total_displayed_posts = 0 %}
        
        {% for group in grouped_posts %}
          {% assign pinned_posts = group.items | where: "pinned", true %}
          {% assign normal_posts = group.items | where_exp: "item", "item.pinned != true" %}
          {% assign sorted_posts = pinned_posts | concat: normal_posts %}
          
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
                  </a>

                  {% if post.subtitle %}
                    {% comment %} Extract main author's first name slug {% endcomment %}
                    {% assign author1_parts = post.subtitle | split: ' ' %}
                    {% assign author1_slug = author1_parts[0] | downcase %}
                    
                    <h4 class="post-subtitle" style="margin-top: 0.5rem; font-weight: inherit;">
                      By <a href="{{ '/profiles/' | relative_url }}{{ author1_slug }}">{{ post.subtitle }}</a>{% if post.subtitle2 %}{% comment %} Extract co-author's first name slug {% endcomment %}{% assign author2_parts = post.subtitle2 | split: ' ' %}{% assign author2_slug = author2_parts[0] | downcase %} and <a href="{{ '/profiles/' | relative_url }}{{ author2_slug }}">{{ post.subtitle2 }}</a>{% endif %}
                    </h4>
                  {% endif %}

                  <p class="post-meta">
                    Posted on {{ post.date | date: site.date_format | default: "%B %d, %Y" }}
                  </p>

                  <div class="post-entry-container">
                    {% if post.image_id %}
                      <div class="post-image">
                        <a href="{{ post.url | relative_url }}">
                          <img src="https://lh3.googleusercontent.com/d/{{ post.image_id }}" alt="{{ post.title }}">
                        </a>
                      </div>
                    {% endif %}
                    
                    <div class="post-entry">
                      {{ post.excerpt | strip_html | truncatewords: 30 }}
                      <a href="{{ post.url | relative_url }}" class="post-read-more">Read More</a>
                    </div>
                  </div>
                </article>
              {% endif %}
            {% endfor %}
          {% endcapture %}

          {% if current_group_count > 0 %}
            {% assign total_displayed_posts = total_displayed_posts | plus: current_group_count %}
            {% assign category_id = panel_id | append: "-" | append: group.name | slugify | default: "general-updates" %}
            
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
          <p class="no-posts-msg">No newsletters found for this academic period.</p>
        {% endif %}
      </div>
    </div>
  </div>
{% endfor %}

<style>
  .tabs-container {
    width: 100%;
    margin: 2rem 0 3rem 0;
    border-bottom: 1px solid #e2e8f0;
  }

  .academic-tabs {
    display: flex;
    justify-content: center;
    gap: 2.5rem;
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1rem;
  }

  .tab-link {
    background: none;
    border: none;
    outline: none;
    padding: 0.75rem 0;
    font-size: 1.15rem;
    font-weight: 500;
    font-family: inherit;
    color: #718096;
    cursor: pointer;
    transition: all 0.2s ease-in-out;
    border-bottom: 3px solid transparent;
  }

  .tab-link:hover, .tab-link.active {
    color: #5f745f; 
    border-bottom: 3px solid #5f745f;
  }

  .academic-panel {
    display: none;
  }
  
  .academic-panel.active {
    display: block;
    animation: fadeIn 0.4s ease;
  }

  .post-subtitle a {
    color: #5f745f;
    text-decoration: none;
    font-weight: 600;
  }

  .post-subtitle a:hover {
    text-decoration: underline;
  }

  .post-read-more {
    display: inline-block;
    margin-left: 0.5rem;
    color: #5f745f;
    font-weight: 600;
    text-decoration: none;
    transition: color 0.2s ease;
  }

  .post-read-more:hover {
    color: #3b4a3b;
    text-decoration: underline;
  }

  .no-posts-msg {
    color: #718096;
    font-style: italic;
    padding: 2rem 0;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(4px); }
    to { opacity: 1; transform: translateY(0); }
  }

  @media (max-width: 767px) {
    .academic-tabs {
      gap: 1rem;
      justify-content: flex-start;
      overflow-x: auto;
      white-space: nowrap;
      padding-bottom: 0.5rem;
      -webkit-overflow-scrolling: touch;
    }
    
    .tab-link {
      font-size: 1rem;
      padding: 0.5rem 0.25rem;
    }

    .tabs-container {
      margin: 1rem 0 2rem 0;
    }
  }
</style>

<script>
  function switchAcademicYear(evt, panelId) {
    const panels = document.getElementsByClassName("academic-panel");
    for (let i = 0; i < panels.length; i++) {
      panels[i].classList.remove("active");
    }

    const tabs = document.getElementsByClassName("tab-link");
    for (let i = 0; i < tabs.length; i++) {
      tabs[i].classList.remove("active");
    }

    document.getElementById(panelId).classList.add("active");
    evt.currentTarget.classList.add("active");
  }
</script>

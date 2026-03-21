---
layout: page
title: Ninad
subtitle: Click thumbnail to download ninad
---
<style>
  .pdf-grid-container {
    display: flex;
    flex-wrap: wrap;
    gap: 30px;
    justify-content: center;
    padding: 20px 0;
  }

  .ninad-card {
    display: flex;
    flex-direction: column;
    width: 220px;
    background-color: white; /* Card background is still white */
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s ease;
    overflow: hidden; 
  }

  .ninad-card:hover {
    transform: translateY(-8px);
  }

  /* Updated caption styling */
  .ninad-caption-banner {
    /* Changed to very slight greenish-white and moved caption down */
    background-color: #f7fff7; 
    color: #333; /* Use a darker text for contrast on light background */
    padding: 12px 5px;
    text-align: center;
    font-weight: 600;
    font-size: 1rem;
    margin: 0;
    /* Important: Changed order to 1 to move below image */
    order: 1; 
    /* Add space between picture and caption */
    margin-top: 15px; 
    border-top: 1px solid #e0e0e0; /* Optional: subtle line separator */
  }

  /* Ensures thumbnails are uniform and removes order style */
  .ninad-card img {
    width: 100%;
    height: 300px; 
    object-fit: cover;
    display: block;
  }
</style>

<div class="pdf-grid-container">
  
  <figure class="ninad-card">
    {% include pdf.html thumbnail_path="/assets/Ninads/25-26.png" 
    pdf_path="https://drive.google.com/uc?export=download&id=1BOHZ1iYSJ9j9mpoCR4AyvWRJUxMwauNh" only_thumbnail=true %} 
<figcaption class="ninad-caption-banner">Ninad 2023-24</figcaption>
  </figure>

  <figure class="ninad-card">
    {% include pdf.html thumbnail_path="/assets/Ninads/23-24.png" 
    pdf_path="https://drive.google.com/uc?export=download&id=161g-gIkPTlV3mbLBJH5srVOQKo626C26" only_thumbnail=true %} <figcaption class="ninad-caption-banner">Ninad 2023-24</figcaption>
  </figure>

  <figure class="ninad-card">
    {% include pdf.html thumbnail_path="/assets/Ninads/20-21.png" 
    pdf_path="https://drive.google.com/uc?export=download&id=1PHivXhIcHCvNP4rXcgtujfAU9xH9wEFF" only_thumbnail=true %}
    <figcaption class="ninad-caption-banner">Ninad 2020-21</figcaption>
  </figure>

  <figure class="ninad-card">
    {% include pdf.html thumbnail_path="/assets/Ninads/19-20.png" 
    pdf_path="https://drive.google.com/uc?export=download&id=19S9ppPF3vfPfE5EKnAzJYvylu442QgLg" only_thumbnail=true %}
    <figcaption class="ninad-caption-banner">Ninad 2019-20</figcaption>
  </figure>

  <figure class="ninad-card">
  {% include pdf.html 
     thumbnail_path="/assets/Ninads/17.png" 
     pdf_path="https://drive.google.com/uc?export=download&id=1w8JTz5IaMSysoqtxqyiFksKrQ9it64VJ" 
     only_thumbnail=true %} 
  <figcaption class="ninad-caption-banner">Ninad 2017</figcaption>
</figure>

</div>

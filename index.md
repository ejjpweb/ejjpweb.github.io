---
layout: default
title: Home
---




# Welcome


This is my professional website where you can gather information regarding my current research and teaching tasks.

I am an [ARAID](https://www.araid.es) researcher at [University of Zaragoza](https://www.unizar.es)'s [Departamento de Ingeniería Química y Tecnologías del Medio Ambiente](http://iqtma.unizar.es/), [Instituto de Nanociencia y Materiales de Aragón](http://inma.unizar-csic.es) (INMA) CSIC-Universidad de Zaragoza and a member of the [Nanostructured Films & Particles](https://nfp.unizar.es) Research Group leading the [Nanomaterials for Solar Energy Harvesting](https://nfp.unizar.es/research/#nanomaterials-for-solar-energy-harvesting) research line.
<br>




<div class="related">
  <h3>Recent Posts</h3>
  <ul class="related-posts">
    {% for post in site.posts limit:3 %}
      <li>
        <h4>
          <a href="{{ post.url }}">
            {{ post.title }}
            <small>{{ post.date | date_to_string }}</small>
          </a>
        </h4>
      </li>
    {% endfor %}
  </ul>
</div>







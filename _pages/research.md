---
layout: archive
title: "Investigaciones"
permalink: /research/
author_profile: true
header:
  og_image: "research/ecdf.png"
---

Mi investigación se centra en la aplicación de métodos cuantitativos y econométricos para analizar problemas de economía pública, mercado laboral, educación y salud. A través del uso de microdatos, técnicas de evaluación de impacto y análisis estadístico, busco generar evidencia que contribuya al diseño y evaluación de políticas públicas basadas en datos.

Una de mis líneas de investigación analiza las **diferencias salariales entre trabajadores del sector público y del sector privado**, con el objetivo de comprender los factores asociados a las brechas de ingresos y sus implicancias para el mercado laboral peruano. Asimismo, estudio el **impacto del proceso de licenciamiento universitario sobre la calidad de la educación superior**, evaluando cómo las políticas regulatorias pueden influir en las oportunidades educativas y laborales de los estudiantes.

Otra línea de investigación se enfoca en la economía de la salud, particularmente en la **estimación del impacto económico de las enfermedades no transmisibles en el Perú**. Este trabajo cuantifica la carga económica asociada a estas enfermedades y proporciona evidencia para apoyar la formulación de políticas de prevención y asignación eficiente de recursos en el sistema de salud.

<nbsp>

{% include base_path %}

{% assign ordered_pages = site.research | sort:"order_number" %}

{% for post in ordered_pages %}
  {% include archive-single.html type="grid" %}
{% endfor %}

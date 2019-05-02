---
title: Documentación de Flutter
short-title: Docs
description: La landing page para la documentación de Flutter.
---

{% for card in site.data.docs_cards -%}
  {% capture index0Modulo3 %}{{ forloop.index0 | modulo:3 }}{% endcapture %}
  {% capture indexModulo3 %}{{ forloop.index | modulo:3 }}{% endcapture %}
  {% if index0Modulo3 == '0' %}
  <div class="card-deck mb-4">
  {% endif %}
    <a class="card" href="{{card.url}}">
      <div class="card-body">
        <header class="card-title">{{card.name}}</header>
        <p class="card-text">{{card.description}}</p>
      </div>
    </a>
  {% if indexModulo3 == '0' %}
  </div>
  {% endif %}
{% endfor -%}

## Que hay de nuevo en este sitio

**26 de Febrero, 2019 **

Flutter released [version
1.2](https://developers.googleblog.com/2019/02/launching-flutter-12-at-mobile-world.html)
en Mobile World Congress (MWC) en Barcelona. Para más información, mira las 
[release notes](https://github.com/flutter/flutter/wiki/Release-Notes---Flutter-1.2.1)
o [descarga el release](/docs/development/tools/sdk/archive).

Para una lista de nuevos documentos, mira [que hay de nuevo](/docs/whats-new-archive).

## ¿Nuevo en Flutter?

Cuando ya has pasado por [Empezar](/docs/get-started/install),
incluyendo [Escribe tu Primera App Flutter,](/docs/get-started/codelab)
aquí hay algunos nuevos pasos.

### Docs

¿Vienes de otra plataforma? Revisa:
[Android](/docs/get-started/flutter-for/android-devs)
[iOS](/docs/get-started/flutter-for/ios-devs)
[Web](/docs/get-started/flutter-for/web-devs)
[React Native](/docs/get-started/flutter-for/react-native-devs)
[Xamarin.Forms](/docs/get-started/flutter-for/xamarin-forms-devs)

[Construir layouts en Flutter](/docs/development/ui/layout)
: Aprende como crear layouts en Flutter, donde todo es un widget.

[Añade interactividad a tu app Flutter](/docs/development/ui/interactive)
: Aprende como añadir un widget stateful a tu app.

[Un tour por el framework de widgets de Flutter](/docs/development/ui/widgets-intro)
: Aprende más sobre el framework estilo-reactivo de Flutter.

[FAQ](/docs/resources/faq)
: Obtén las respuestas a las preguntas más frecuentes.

### Videos

¡También tenemos algunos videos útiles en nuestro [Canal 
de Youtube de Flutter]({{site.social.youtube}})! En 
particular, revisa la serie Flutter in Focus, 
y apende sobre otras series en 
en nuestra página de [videos](/docs/resources/videos).

<iframe style="max-width: 100%" width="560" height="315" src="https://www.youtube.com/embed/wgTBLj7rMPM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
Flutter in Focus: Learn Flutter features in 10 minutes or less.<br>
[Flutter in Focus playlist](https://www.youtube.com/playlist?list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2)

En Flutter, ¡"todo es un widget"! Si quieres enteder mejor dos tipos de widgets, Stateless y Stateful, mira los siguientes videos,
parte de la serie [Flutter in
Focus](https://www.youtube.com/playlist?list=PLjxrf2q8roU2HdJQDjJzOeO6J3FoFLWr2).

<iframe style="max-width: 100%" width="560" height="315" src="https://www.youtube.com/embed/wE7khGHVkYY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> <iframe width="560" height="315" src="https://www.youtube.com/embed/AqCMFXEmf3w" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## ¿Buscas mejorar tus conocimientos?

Si aprendes mejor mirando a ingenieros escribir código, comenter equivocaciones, y corregirlas,
revisa la serie de videos 
[Boring Flutter Show](https://www.youtube.com/watch?v=vqPG1tU6-c0&list=PLjxrf2q8roU28W3pXbISJbVA5REsA41Sx&index=3&t=9s)

<iframe style="max-width: 100%" width="560" height="315" src="https://www.youtube.com/embed/vqPG1tU6-c0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Boring Flutter Show playlist](https://www.youtube.com/watch?v=vqPG1tU6-c0&list=PLjxrf2q8roU28W3pXbISJbVA5REsA41Sx&index=3&t=9s)

Quizas también encuentres estos documentos útiles:

* [Usando paquetes](/docs/development/packages-and-plugins/using-packages)
* [Añadir assets e imágenes](/docs/development/ui/assets-and-images)
* [Navegación y rutas](/docs/development/ui/navigation)
* [Manejo de estados](/docs/development/data-and-backend/state-mgmt/intro)
* [Animaciones](/docs/development/ui/animations)

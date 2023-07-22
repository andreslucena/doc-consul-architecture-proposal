# Propuesta de hoja de ruta, y preguntas para la reflexión colectiva

Se propone la siguiente hoja de ruta para trabajar en un desarrollo conjunto a futuro:

En el corto plazo, aplicar la propuesta "**1. Directorios de personalización**". Esta solución puede servir como punto de partida para la mayoría de los forks basados en upstream, pero como ya hemos visto, tiene el inconveniente de que una vez se diverge en términos de necesidades funcionales es un modelo que deja de ser válido por la propia arquitectura por defecto de Ruby on Rails. Esta solución es la que tiene el menor impacto en los tiempos de desarrollo actuales. Se recomiendan las siguientes lecturas para su aplicación:

1. Rails Internationalization (I18n) API - Ruby on Rails Guides - 2. Setup the Rails Application for Internationalization - Optional: Custom I18n Configuration Setup: [http://guides.rubyonrails.org/i18n.html#optional-custom-i18n-configuration-setup](http://guides.rubyonrails.org/i18n.html#optional-custom-i18n-configuration-setup)
2. The Asset Pipeline - Ruby on Rails Guides - 2. How to Use the Asset Pipeline - Asset Organization: [http://guides.rubyonrails.org/asset\_pipeline.html#asset-organization](http://guides.rubyonrails.org/asset\_pipeline.html#asset-organization)
3. Autoloading and Reloading Constants - Ruby on Rails Guides - 5. autoload\_paths: [http://guides.rubyonrails.org/autoloading\_and\_reloading\_constants.html#autoload-paths](http://guides.rubyonrails.org/autoloading\_and\_reloading\_constants.html#autoload-paths)

Para iniciar la discusión sobre esta funcionalidad se ha realizado un Pull Request  relacionado con este Issue, donde aparte del código se agrega la documentación necesaria para su uso .

En el medio plazo, por parte del Ajuntament de Barcelona se comenzará una nueva versión basándose en la propuesta "**2. Modularización (engines)**" ya que es la única solución que cumple con las necesidades expuestas anteriormente sin suponer un enlentecimiento grande en los tiempos de desarrollo. Una vez se tenga una versión funcional de este modelo se realizará una reunión con el equipo de desarrollo del Ayuntamiento de Madrid para poder intercambiar impresiones sobre esta experiencia y continuar con la discusión y planificación de una hoja de ruta a seguir. Se recomiendan las siguientes lecturas:

1. Component-Based Rails Applications - Stephan Hagemann, Ryan Platte, Benjamin Smith and Enrico Teotti - 2016 - [https://leanpub.com/cbra](https://leanpub.com/cbra)
2. Modular Rails - Thibault Denizet - 2015 [http://modular-rails.samurails.com/](http://modular-rails.samurails.com/)

Algunas preguntas que queremos plantear para la reflexión colectiva, son:

¿En qué medida es importante favorecer que el desarrollo de Cónsul sea distribuido para la construcción y fortalecimiento de la red de ciudades del cambio?

¿En qué medida está favoreciendo o dificultando la arquitectura monolítica de Ruby Rails la adopción de Consul por parte de otros ayuntamientos?

¿En qué medida es esperable que la reutilización de código escrito distribuidamente compense el mayor esfuerzo para escribirlo y documentarlo?

# Análisis de alternativas para potenciar la reutilización y el desarrollo colaborativo de Consul

Idealmente, el modelo de desarrollo de Consul se caracterizaría por su:

* Facilidad de reutilización: los ayuntamientos u organizaciones deben poder re-utilizar el código de la manera más sencilla posible.
* Facilidad de contribución: facilidad de contribuir al código desde adaptaciones o funciones que se puedan desarrollar en otro municipio o por otra organización.
* Facilidad de escalar con nuevas funcionalidades: es la capacidad del código de ir creciendo de manera sostenible en sofisticación y complejidad.

A continuación se presentan tres posibles soluciones para potenciar la reutilización, la contribución y la escalabilidad de Consul, junto con sus ventajas e inconvenientes, ordenados de menor a mayor esfuerzo requerido para su implementación y el impacto negativo en la velocidad de desarrollo ("1. Directorios de personalización", “2. Modularización (engines)” y “3. Microservicios”).

Las soluciones se evaluarán en función de necesidades concretas extraídas de la experiencia de decidim.barcelona, basándonos en el documento realizado por su programador principal Josep Jaume Reyes y agregando las relativas a la facilidad de contribución y escalabilidad que se consideran importantes de cara a siguientes fases:

* Adaptación de diseño (logos Ajuntament de Barcelona, campaña, interfaz etc.)
* Incorporación de nuevas funcionalidades para adaptarse a diferentes procesos participativos: Categorización PAM/PAD, fases posteriores, normativas.
* Traducciones, páginas estáticas.
* Compartir desarrollos entre ciudades que quieren desplegar procesos participativos similares (compartir desarrollos de forma no centralizada).
* Actualizar el código de Consul una vez realizadas las modificaciones de adaptación, para que puedan ser reutilizados fácilmente.

## Alternativa 1: Directorios de personalización

Se trata de la solución propuesta por el Ayuntamiento de Madrid en la issue número 953 publicada en Github el 29 de febrero de 2015 con el título "Custom folder structure":

![Figura 5: Ticket nº 953 - Custom folder structure - GitHub de Consul](.gitbook/assets/image\_5.png)

**Figura 5**: Ticket nº 953 - Custom folder structure - GitHub de Consul

Se traduce a continuación:

_Con el fin de mejorar la colaboración entre los_ forks _y_ upstream_, debe haber un lugar para poner el código personalizado y un lugar para el código genérico. Hay una serie de posibilidades para resolver este problema, tales como_ gems _y_ engines_. Si bien creemos que estas son buenas opciones, nos gustaría mantener las cosas lo más simple posible y empezar con una estructura de carpetas personalizada._

_Algunas de las carpetas personalizadas podrían ser:_

* _MVC_
* _Assets_
* _Configuración_

### Argumentos a favor

1. Facilidad de uso: al ser una aplicación Vanilla Rails es fácilmente asumible por programadores que se hayan acercado por primera vez al framework Ruby on Rails. Con una arquitectura compleja, aparte de aprender el framework, los desarrolladores novatos también deberían aprender las especificidades de este desarrollo antes de poder comenzar a realizar cambios. Una parte importante de los desarrolladores que se quieren incorporar al flujo de desarrollo del Ayuntamiento de Madrid no cuentan con los conocimientos del lenguaje (Ruby) ni del framework (Ruby on Rails): programadores del Ayuntamiento, estudiantes de la Universidad , etc.
2. Simplicidad de cara al desarrollo: Mantendría la estructura monolítica actual, y por tanto los menores tiempos de desarrollo.

### Argumentos en contra

1. Impide compartir los distintos módulos de cada proceso diferenciado. Se puede dat el caso de que se quiera incorporar el código realizado por alguno de los forks a upstream, por ejemplo con las Citas presenciales que se han hecho en decidim.barcelona, el portal de Participación del Ajuntament de Barcelona. Por la propia naturaleza de las aplicaciones Ruby on Rails monolíticas esto es complejo y debe realizarse de forma manual. Lo mismo puede decirse de otros módulos realizados para el proceso de Pla Municipal.
2. Se genera un cuello de botella en la introducción y aceptación de funcionalidades nuevas por parte del equipo de desarrollo de Consul.

## Alternativa 2: Modularización (engines)

Se trata de la solución planteada al problema de la reutilización de módulos en distintas aplicaciones Ruby On Rails. Se toma la definición de las guías oficiales de Ruby on Rails.

_Los engines pueden ser considerados aplicaciones en miniatura que proporcionan funcionalidad a sus aplicaciones anfitrión. (...) Por lo tanto, los engines y las aplicaciones se pueden considerar casi la misma cosa, sólo con diferencias sutiles, como se verá a lo largo de esta guía. Engines y aplicaciones también comparten una estructura común._

### Argumentos a favor

1. Reduce el tiempo de ejecución de tests. Esto es importante tanto para la integración continua como para el desarrollo en local.
2. Es más fácil encontrar un bug. Si un test se rompe dentro de un engine se sabe que el fallo debe estar dentro o en sus dependencias. En aplicaciones monolíticas el fallo se puede encontrar en cualquier parte.
3. Es más fácil quitar componentes que no se estén utilizando.
4. Es más fácil entender el histórico del desarrollo de un módulo. Aunque se pueda hacer un git log en todo el proyecto o ficheros específicos de una funcionalidad
5. Las migraciones se organizan mejor al estar prefijadas con el nombre del engine.
6. Permite customizar mejor las instalaciones. En una aplicación monolítica es posible deshabilitar funcionalidades, pero el código sigue estando disponible.
7. Es más fácil entender y manejar las dependencias. En un Gemfile global de una aplicación monolítica es complejo de ver en qué parte del código se está utilizando esa dependencia. Cuando está contenida en una engine es obvio ver que parte es la que lo utiliza.
8. Provee una forma alternativa de refactorizar una funcionalidad siempre y cuando se mantenga la misma API.
9. Permite un desarrollo en paralelo organizado. Uno o más desarrolladores pueden estar trabajando en una funcionalidad específica (por ejemplo, Debates) mientras otra parte puede trabajar en otra (por ejemplo, Presupuestos participativos). Esto es más complejo en arquitecturas monolíticas.
10. Permite evitar un cuello de botella en la introducción y aceptación de funcionalidades nuevas por parte del equipo de desarrollo de Consul.

### Argumentos en contra

1. Requiere una inversión inicial costosa de tiempo y recursos para el cambio a esta arquitectura, en comparación a otras alternativas.
2. Enlentece la escritura de código. Al empezar con este modelo hay que tener en cuenta en qué componente pertenece cada funcionalidad que se quiera agregar.
3. Aumenta la curva de aprendizaje de gente nueva al proyecto.
4. Requiere mayores esfuerzos en documentación.

## Alternativa 3: Microservicios

Se trata de la solución que aplican en distintos proyectos de software libre, como las herramientas propuestas por el proyecto europeo D-CENT y el software de votaciones electrónicas Agora Voting. Se toma la definición del artículo de Martin Fowler.

_(...) el estilo arquitectónico de microservicio es un enfoque para el desarrollo de una única aplicación como un conjunto de pequeños servicios, cada uno ejecutándose en su propio proceso y comunicándose con mecanismos livianos, a menudo una API HTTP. (...) Hay un mínimo de gestión centralizada de estos servicios, que pueden estar escritas en lenguajes de programación diferentes y utilizan diferentes tecnologías de almacenamiento de datos._&#x20;

### Argumentos a favor&#x20;

1. Cumple con la mayoría de puntos a favor que se encuentran en la arquitectura "2. Modularización (engines)".
2. Su principal ventaja reside en que esta arquitectura permite que cada componente diferenciado esté escrito en un lenguaje de programación diferente, por lo que diferentes equipos de desarrollo pueden contribuir sin tener la limitación de que todos tengan que saber Ruby on Rails.

### Argumentos en contra

1. De todas las arquitecturas propuestas es la que complejiza enlentece más el desarrollo.
2. Aumenta la curva de aprendizaje de gente nueva al proyecto.
3. Requiere mayores esfuerzos en documentación.
4. Puede agregar latencias en la red por las distintas conexiones que tiene que realizarse para cada petición.
5. Dificulta tanto el desarrollo como realizar pruebas de integración de todos los servicios y despliegue de los mismos.

## Comparativa de las tres alternativas

A continuación se analizan los principales problemas encontrados y definidios anteriormente por el equipo de desarrollo de decidim.barcelona (ver inicio de la presente sección) y como se resuelve cada uno de los problemas, utilizando tres tipos de código para definir el grado de cumplimiento:

1. NC: No cumple
2. P: Cumple parcialmente
3. C: Cumple

| Alternativa                       | Diseño | Funcional | Traducciones | Compartir | Actualizar | Rapidez |
| --------------------------------- | ------ | --------- | ------------ | --------- | ---------- | ------- |
| 0. Situación actual               | NC     | NC        | NC           | NC        | NC         | C       |
| 1. Directorios de personalización | C      | P         | C            | NC        | C          | C       |
| 2. Modularización (engines)       | C      | C         | C            | C         | C          | P       |
| 3. Microservicios                 | C      | C         | C            | C         | C          | NC      |

Se puede ver que aunque la solución más simple de aplicar sea la propuesta de "**1. Directorios de personalización**", lo cierto es que hay una necesidad que consideramos fundamental que no cumple: la posibilidad de compartir fácilmente los avances de forma paralela. Por ejemplo, se da el caso de instituciones que quieren a su vez utilizar los Presupuestos Participativos desarrollados por el Ayuntamiento de Madrid y el proceso del Plan Municipal (PAM) realizado por el Ajuntament de Barcelona.

Con respecto a solución de "**3. Microservicios**" se puede ver que es difícil contar con los recursos para reescribir todas las múltiples funcionalidades ya ofrecidas por las aplicaciones en su desarrollo actual. Requiere una reescritura completa del código actual ya que es un cambio de paradigma importante: se tiene que llevar todo el desarrollo a tecnologías como Docker de cara a facilitar su instalación, programar APIs HTTP que ofrezcan los datos (para consumir, crear, modificar, gestión de permisos de usuario, etc) y documentar cada una de estas APIs. El único beneficio que se obtendría es el de poder facilitar el desarrollo de otros programadores no familiarizados con Ruby on Rails, ya que se puede utilizar cualquier lenguaje, pero el balance de coste contra ganancia es negativo.

Por último , la solución "**2. Modularización (engines)**" permite cumplir casi todas las necesidades, siendo el único criterio que no cumple el de rapidez de escritura de código, al salirse del estándar de Ruby on Rails “vanilla”. Sin embargo, los desarrolladores que estén aprendiendo Ruby on Rails lo harían partiendo de este modelo con múltiples ejemplos de cómo deben realizarse los componentes, por lo que la curva de aprendizaje tampoco sería tan pronunciada.

El problema a solucionar no es sencillo, por lo que la solución debe aplicarse en múltiples iteraciones que permitan a su vez cumplir con las necesidades funcionales de cada proceso.

&#x20;Véase Josep Jaume Rey, Consul y decidim.barcelona - [https://gist.github.com/josepjaume/c820c84a36d93163694c](https://gist.github.com/josepjaume/c820c84a36d93163694c) : Ver definición en Anexo (Glosario) : Ver definición en Anexo (Glosario) : Ver definición en Anexo (Glosario) : Ver definición en Anexo (Glosario) : Ver definición en Anexo (Glosario) : Véase [http://wikis.fdi.ucm.es/ELP/Trabajo:C%C3%B3mo\_colaborar\_con\_el\_ayuntamiento](http://wikis.fdi.ucm.es/ELP/Trabajo:C%C3%B3mo\_colaborar\_con\_el\_ayuntamiento) ) : Véase [http://guides.rubyonrails.org/engines.html#what-are-engines-questionmark](http://guides.rubyonrails.org/engines.html#what-are-engines-questionmark) ) : Texto original: Engines can be considered miniature applications that provide functionality to their host applications. (...) Therefore, engines and applications can be thought of almost the same thing, just with subtle differences, as you'll see throughout this guide. Engines and applications also share a common structure. : Basado en [http://ruby-on-rails.wikidot.com/modular-component-based-rails-architecture](http://ruby-on-rails.wikidot.com/modular-component-based-rails-architecture) ) : Véase [https://github.com/d-cent/](https://github.com/d-cent/) ) : Véase [https://github.com/agoravoting](https://github.com/agoravoting) ) : Véase [http://martinfowler.com/articles/microservices.html](http://martinfowler.com/articles/microservices.html) ) : Texto original: (...) the microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API. (...) There is a bare minimum of centralized management of these services, which may be written in different programming languages and use different data storage technologies. : Basado en [http://ruby-on-rails.wikidot.com/modular-component-based-rails-architecture](http://ruby-on-rails.wikidot.com/modular-component-based-rails-architecture) )

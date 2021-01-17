---
title: Mostrar texto con colores en consola
layout: post
read_time: 2 min.
locale: es
---
Mostrar texto con diferentes colores en una herramienta de consola ayuda a entender los mensajes que se muestran, asi que veamos como podemos hacer uso de esto.

{:.mt-5 .mb-3}
### Colorize

Afortunadamente en la asombrosa comunidad de Ruby ya existe una gema que nos provee este proceso, así que solo tenemos que utilizarla.
Como siempre la buena practica de instalar gemas con bundler así que agrega `gem 'colorize'` en tu Gemfile. Luego en tu código ruby la llamas como cualquier gema `require 'colorize'` y ahora todos los objetos string tendrán nuevos métodos para colorizar en la consola, veamos como usarlos

{% highlight ruby linenos %}
require 'colorize'
puts 'este texto es rojo'.red
puts 'este texto es azul'.blue
puts 'este texto es verde'.green
{% endhighlight %}
Creo que el texto es bastante explicativo, básicamente los nuevos métodos tienen el nombre del color que deseamos utilizar. Si ejecutamos el código anterior veremos algo como esto

![colorizing](/assets/images/posts/colorize/prove.png){:.img-fluid}

La gema incluso nos permite mostrarlo con fondo de colores y muchas otras opciones que podes revisar en su [repositorio](https://github.com/fazibear/colorize){:target='_blank'}

{:.mt-5 .mb-3}
### Conclusión
Como muchas veces, la comunidad fuerte de Ruby nos provee herramientas para hacer nuestro desarrollo mas sencillo; con esto podemos tener mejores mensajes de error, advertencia, éxito e incluso tener una aventura de texto.

Espero que haya sido de ayuda y comienza a utilizar colores!!

---
title: Como funciona Monkey Patching
layout: post
read_time: 4 min.
locale: es
---
¿Alguna vez has deseado que alguna clase estándar de Ruby o de alguna gema tenga ya un método por defecto que podas usar directamente? Pues Ruby tiene una forma de poder agregar métodos a clases ya existentes y esto se llama Monkey Patching.

{:.mt-5 .mb-3}
### Monkey Patching

En Ruby todas las clases pueden ser reabiertas para agregar o incluso sobrescribir métodos, sin embargo, hay que tener mucho cuidado al hacer esto ya que podrías modificar clases sin darte cuenta, esto podría suceder cuando distintas clases tienen métodos con el mismo nombre. Es por eso que es buena practica encapsular nuestro monkey matching en módulos correspondientes.    
Veamos como podemos agregar un método a la clase `String`

{% highlight ruby linenos %}
# Abrimos la clase String del core de Ruby (por eso los cuatro puntos)
class ::String
  # Comprueba si la cadena contiene algún numero
  def include_numbers?
    (0..9).each do |d|
      return true if include? d.to_s
    end
    false
  end
end
{% endhighlight %}

Ahora con ejecutar este código mediante un `require`, todas nuestras strings tendrán por defecto nuestro método

{% highlight ruby linenos %}
require_relative 'string_extensions'

# retornara false
'abc'.include_numbers?

# retornara true
'8nn2'.include_numbers?
{% endhighlight %}

{:.mt-5 .mb-3}
### Conclusión

Con monkey patching, nuestro código se vuelve mas legible (y elegante) ademas que promueve la reusabilidad de código, como dije antes, hay que tener mucho cuidado organizándolo para no tener comportamientos inesperados.

Espero que haya sido de ayuda y comienza a hacer monkey patching!!

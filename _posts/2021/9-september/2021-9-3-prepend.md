---
title: Como utilizar prepend
layout: post
read_time: 5 min.
locale: es
---

Una manera distinta de herencia.

{:.mt-5 .mb-3}
### prepend
Seguramente estas familiarizando con la forma mas común de sobrescribir un método en el cual la clase madre define un método y este método puede ser sobrescrito por las clases derivadas. Sin embargo, en Ruby existe una manera de incluir una colección de métodos en tu clase y evitar que sean sobrescritos.  
Y eso es lo que veremos en este post, la palabra reservada `prepend` funciona para incluir los métodos de un modulo y que estos (como dice la palabra) se antepongan sobre los métodos de la clase, veamos el ejemplo

{% highlight ruby linenos %}
module Mother
  def do_something
    p 'mother did something'
  end
end

class Daughter
  include Mother
  def do_something
    p 'daughter did something'
  end
end

class Son
  prepend Mother

  def do_something
    p 'son did something'
  end
end


d = Daughter.new
d.do_something

s = Son.new
s.do_something
{% endhighlight %}

Si ejecutas el código de ejemplo veras que la clase `Son` no ejecuta su propio método sino que ejecuta el método del modulo `Mother` ya que a diferencia de la clase `Daughter`, esta está utilizando `prepend`.

Ahora te preguntaras, ¿que uso se podría dar a esta funcionalidad?; pues uno de los mas comunes es cuando necesitamos que un método se ejecute previo a otro método (ej. validación), para eso necesitamos hacer una ligera modificación al método original del modulo `Mother`.

{% highlight ruby linenos %}
module Mother
  def do_something
    p 'mother did something'
    super
  end
end
{% endhighlight %}

Con la palabra reservada `super` nos aseguramos que se llame al método `do_something` de cualquier clase que incluya (o en este caso, anteponga) el modulo, asi el código de las clases sera mas limpio y transparente, si ejecutas la clase `Son` veras que la salida sera
{% highlight shell %}
mother did something
son did something
{% endhighlight %}

{:.mt-5 .mb-3}
### Conclusión
Si bien el caso de uso expuesto puede cubrirse con herencias regulares, `prepend` nos ayuda a tener código *mágico* más limpio y menos redundante.

Espero que haya sido de ayuda y utiliza prepend!!

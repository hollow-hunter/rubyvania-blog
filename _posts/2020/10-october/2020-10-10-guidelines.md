---
title: Estilos y buenas practicas.
layout: post
read_time: 5 min.
locale: es
---
Como cualquier otro lenguaje de programación, Ruby tiene sus propias buenas practicas y estilo de codificación.

### Convenciones
Ya te habras dado cuenta viendo codigo de proyectos open source, asi tambien en sitios como este que el codigo Ruby sigue ciertas convenciones como en los nombres de variables, estructura del codigo y otras cosas. Veremos un poco de las mas comunes/conocidas.    
Primero veamos los nombres
{% highlight ruby linenos %}
# Los nombre de las clases siempre son en PascalCase
class VideoGame
  # Métodos y variables son snake_case
  def select_character
    current_character = nil
  end
  # Si el método retorna un valor booleano, se debe declarar con signo de interrogación al final
  def exist?(a)
    false
  end
end
{% endhighlight %}
Por favor tomar en cuenta que en snake_case **todo** tiene que estar en minúscula, es decir, esto `select_Character` es incorrecto. Si venís de Java seguramente estas acostumbrado a tener métodos que inician con `get` y `set`, hacer esto tambien es incorrecto.

Ahora continuamos con la llamada de metodos, quizas ya sepas que Ruby soporta la ejecucion de metodos con parametros sin la necesidad de encerrarlos entre parentesis, pero hay casos en que es mas legible (e incluso necesario) usarlos.
{% highlight ruby linenos %}
# Con parentesis
puts('hello')
# Sin parentesis
puts 'hello'
# Para métodos anidados, es mas legible usar paréntesis
puts my_method(1, 2)
# A partir de Ruby 2.7, los métodos que reciban un hash ya no es necesario encerrarlo en hash
# así que esto
my_method_hash { a: 1, b: 2, c: 3 }
# se simplifica a esto
my_method_hash a: 1, b: 2, c: 3
{% endhighlight %}

Ahora veamos un poco de estructura de código; la indentacion en Ruby es de 2 espacios, así que ajusta bien tu editor de texto para que un TAB no agregue 4 espacios en tus proyectos Ruby.    
Utiliza `if` para condicionantes verdaderas y `unless` para condiciones falsas, por ejemplo
{% highlight ruby linenos %}
# Cuando sea verdadero se vera en pantalla 'active'
puts 'active' if a.active?
# Cuando sea falso, se vera en pantalla 'not active'
puts 'not active' unless a.active?
{% endhighlight %}
Ya que estamos viéndolo, cuando solo tengas una linea dentro de tu bloque `if` es mejor usar este formato que acabamos de ver. En donde la parte de la izquierda solo se ejecuta si la parte de la derecha cumple la condición.    
Ahora en lo contrario, si tenes una sección de código que debe ejecutarse **solamente si** se cumple cierta condición entonces en lugar de encerrar todas las lineas en ese bloque `if` se debe utilizar una cláusula de guardia (guard clause)
{% highlight ruby linenos %}
# En lugar de hacer esto
def process(a)
  if a.active?
    # Mucho
    # Código
    # Aqui
  end
end

# Se debe hacer esto
def process(a)
  return unless a.active?

  # Mucho
  # Código
  # Aqui
end
{% endhighlight %}

Cuando uses `case` no es necesario indentar las condicionantes, solo el código que se procesa
{% highlight ruby linenos %}
# Esto es incorrecto
case a
  when a.nil?
    a = A.new
  when a.active?
    a.process
end
# Esto es correcto
case a
when a.nil?
  a = A.new
when a.active?
  a.process
end
{% endhighlight %}

### Conclusión

Esto es una introducción muy pequeña y obviamente no cubre todo el código que se te podría ocurrir para tu aplicación. Todo esto esta muy bien documentado en el sitio web mantenido por la misma comunidad de Ruby [https://rubystyle.guide](https://rubystyle.guide) te recomiendo que le des un vistazo. También existen herramientas que automáticamente aplican estos formatos desde tu editor de texto, el mas popular siendo [Rubocop](https://github.com/rubocop-hq/rubocop).

Espero que haya sido de ayuda y comienza a aplicar el estilo de código Ruby!!

---
title: Diferencia entre has_one y belongs_to
layout: post
read_time: 2 min.
locale: es
---

Algo facil de confundir a cualquier iniciante en Rails, veamos cuales son las diferencias entre estos dos métodos y cuando se deberian usar.

{:.mt-5 .mb-3}
### belongs_to
Este definitivamente es el más comun entre los dos, usamos `belongs_to` en una relación de uno-a-muchos en conjunto con `has_many` y se lo asignamos al lado de la relacion que es *uno*, veamos los siguientes modelos

{% highlight ruby linenos %}
class Book < ApplicationRecord
  belongs_to :author
end

class Author < ApplicationRecord
  has_many :books
end
{% endhighlight %}

Estos modelos nos indican que un autor tiene muchos libros y si lo traducimos a tablas en base de datos, entonces la tabla de `Books` deberia tener una llave foranea apuntando a la tabla `Authors`.

{:.mt-5 .mb-3}
### has_one
Este es menos común pero puede ahorrarnos queries, se utiliza `has_one` en una relacion de uno-a-uno en conjunto con `belongs_to` y se lo asignamos al lado que esta siendo referenciado, veamos el ejemplo

{% highlight ruby linenos %}
class Student < ApplicationRecord
  has_one :desk
end

class Desk < ApplicationRecord
  belongs_to :student
end
{% endhighlight %}

Estos modelos nos indican que un estudiante tiene un solo escritorio y si lo traducimos a tablas en base de datos, entonces la tabla de `Desks` deberia tener una llave foranea apuntando a la tabla `Students`.

{:.mt-5 .mb-3}
### Conclusión
La mayor confusion que puede ocurrir es como saber quien tiene la llave foranea, y la respuesta directa es solo `belongs_to` requiere una llave foranea, de todos modos, es bueno saber las diferencias y en que casos podemos utilizar cada helper.

Espero que haya sido de ayuda y utiliza el que corresponda!!

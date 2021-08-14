---
title: Clases y Objetos
layout: post
read_time: 4 min.
locale: es
---

En este post veremos como declarar nuestras propias clases en Ruby además de aprender el funcionamiento y la estructura interna de una clase Ruby

### Class

Para declarar una clase en Ruby se debe utilizar la palabra reservada `class` seguida del nombre de la clase, por convención el nombre debe ser declarado en PascalCase y el archivo de la clase debe ser el mismo nombre pero snake_case, veamos el inicio de la clase que usaremos para este post `blog.rb` 
{% highlight ruby linenos %}
class Blog
end
{% endhighlight %}

##### Constructor

Para declarar una variable de nuestra clase solo tenemos que usar el método `new` de la siguiente manera `b = Blog.new`. Esto busca el método `initialize`, por defecto ya existe el constructor pero tambien podemos sobrescribirlo y si queremos podemos recibir uno o varios parámetros, en este caso recibiremos uno y le pondremos un valor por defecto.
{% highlight ruby linenos %}
class Blog
  def initialize(name = 'Rubyvania')
    @name = name
  end
end
{% endhighlight %}

Aqui estamos diciéndole a la clase que cuando un nuevo objeto sea creado tambien inicialice una variable `@name` con el texto del parámetro name que por defecto será `'Rubyvania'`, las variables que inician con `@` son variables de instancia por defecto privadas, si necesitamos acceso externo a estas variables requerimos declarar accesores (valga la redundancia).

##### Accessor

Si venis de otros lenguajes de programacion, tal vez estes familiarizado con los conceptos de getters y setters pero si no sabés lo que son, básicamente son métodos públicos que se encargan de leer (get) y escribir (set) valores en variables que pertenecen a la clase. Estos se llaman **accessors** en Ruby y los mas comunes son
- `attr_reader` se utiliza para declarar variables públicas de solo lectura
- `attr_writer` se utiliza para declarar variables públicas de solo escritura
- `attr_accessor` se utiliza para declarar variables públicas tanto de lectura como escritura

Para declarar variables se deben declarar en forma de símbolos Ruby (un símbolo Ruby, es básicamente una constante porque siempre apunta al mismo espacio de memoria e inician con `:`), veamos el codigo

{% highlight ruby linenos %}
class Blog
  attr_accessor :name
  attr_reader :creation_date
  attr_writer :visitors

  def initialize(name = 'Rubyvania')
    @name = name
    @creation_date = Time.now
  end
end
{% endhighlight %}

Si te diste cuenta, al declarar un accessor tambien se habilita como variable de instancia. Sin importar que tipo de accesor sea, la variable de instancia puede ser manipulada como queramos dentro de la clase.

Ahora necesitamos hacer uso de nuestra clase, para eso crearemos un archivo main como lo requiere VS Code y usaremos el método `require_relative` para cargar nuestra clase y poder usarla este método busca el archivo que le indiquemos de manera relativa donde esta siendo ejecutado el codigo, dentro de nuestro archivo main tendremos algo como esto

{% highlight ruby linenos %}
require_relative 'blog'

b = Blog.new
puts b.name
b.name = 'Rubyvania Blog'
puts b.name
puts b.creation_date
b.visitors = 28

{% endhighlight %}

##### Metodos

Para crear métodos se utiliza la palabra reservada `def` (igual que el constructor), por convención los métodos deben estar en snake_case, algo a tomar en cuenta en Ruby es que los métodos tienen un `return` implícito, es decir, que lo sea que se ejecute en la ultima linea del método, eso es lo que se retorna como resultado.
Para declarar **métodos estáticos**, debemos nombrarlos con el prefijo `self`.
Para declarar **métodos privados**, se debe utilizar la palabra reservada `private` y declarar métodos debajo de la linea donde se encuentre esta palabra.

Veamos estos conceptos en nuestro ejemplo

{% highlight ruby linenos %}
class Blog
  attr_accessor :name
  attr_reader :creation_date
  attr_writer :visitors

  def initialize(name = 'Rubyvania')
    @name = name
    @creation_date = Time.now
  end

  # Un metodo publico, no es necesario return para retornar el resultado del método zero?
  def empty
    @visitors.zero?
  end

  # Un metodo estatico
  def self.create
    Blog.new
  end

  private

  # Un metodo privado
  def clear
    @visitors = 0
  end
end
{% endhighlight %}

##### Herencia

Como cualquier lenguaje orientado a objetos, en Ruby existe el concepto de herencia y se realiza utilizando el operador `<`, asi que si queremos crear una clase mas especifica de nuestra clase Blog, podemos declararla de la con la siguiente manera
{% highlight ruby linenos %}
require_relative 'blog'
class ProgrammingBlog < Blog
end
{% endhighlight %}

##### Operadores

Los operadores en Ruby funcionan como cualquier otro método, obviamente tenemos limitantes para no usar símbolos reservados, pero los mas comunes (+, -, <, *, etc.) están disponibles. Esta es la forma de declararlos
{% highlight ruby linenos %}
class Account
  attr_reader :balance
  def +(amount)
    @balance += amount
  end
end

# y ahora podemos hacer lo siguiente

a = Account.new
a + 300
{% endhighlight %}

### Conclusión

Al ser lenguaje "enteramente orientado objetos", comprender como funcionan las clases Ruby es esencial, podemos apreciar que la dinamicidad de Ruby se mantiene al crear variables de instancias sin tener que declararlas aparte, además que no tienen un tipo definido hasta que se le asigna un valor (como cualquier variable), asi mismo al ser un lenguaje dinámico no son necesarios los conceptos de clases abstractas e interfaces (lo cual hace mas sencillo la creación de mocks y stubs en pruebas unitarias).

Espero que haya sido de ayuda y comienza a crear clases!

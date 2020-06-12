---
title: Como hacer tus propias gemas
layout: post
read_time: 5 min.
locale: es
---

Veremos como hacer tus propias librerías y publicarlas para la comunidad en Rubygems.

### Rubygems

Antes de comenzar, necesitaremos una cuenta en [Rubygems](https://rubygems.org/){:target="____blank"} asi que asegurate de crear una.

Muchas veces tendemos a diseñar clases que sean reutilizables dentro de un proyecto y con un poco de esfuerzo adicional, se pueden utilizar en otros proyectos, y a esto le llamamos gemas. Algo que ya vimos como utilizar en un post anterior pero ahora crearemos nuestra propia gema (algo muy básico, no te preocupes).

Siguiendo la guia de [Rubygems](https://guides.rubygems.org/make-your-own-gem/){:target="____blank"} haremos una simple gema que diga "Holita mundo!" asi que en la raíz de nuestro entorno de desarrollo crearemos un archivo `holita.gemspec` con el siguiente contenido (si realmente vas a publicar esta gema de ejemplo, tendras que cambiar el nombre ya que yo lo tengo utilizado)

{% highlight ruby linenos %}
Gem::Specification.new do |s|
  s.name        = 'holita'
  s.version     = '0.0.0'
  s.date        = '2020-06-11'
  s.summary     = 'Holita!'
  s.description = 'Gema para post de rubyvania'
  s.authors     = ['Tu nombre']
  s.email       = 'Tu correo electrónico'
  s.files       = ['lib/holita.rb']
  s.homepage    =
    'https://rubygems.org/gems/holita'
  s.license = 'MIT'
end
{% endhighlight %}

Continuando con las convenciones, dentro de una carpeta `lib` crearemos un archivo `holita.rb` (como lo indicamos en nuestra gemspec) con el siguiente contenido
{% highlight ruby linenos %}
class Holita
  def self.hi
    puts 'Holita mundo!'
  end
end
{% endhighlight %}

Luego tenemos que generar el archivo gem, y para eso tenemos que ejecutar el siguiente comando `gem build holita.gemspec` esto generara un archivo `holita-0.0.0.gem` en la misma ruta.

Para publicar necesitas generar unas credenciales en una ruta especial de la utilidad **gem** asi que ingresa a la siguiente dirección [https://rubygems.org/api/v1/api_key.yaml](https://rubygems.org/api/v1/api_key.yaml){:target="____blank"}, te pedirá tu cuenta de Rubygems y descargara un archivo, toma ese archivo y llevalo a la ruta `C:\Users\tu_usuario_windows\.gem` con el nombre `credentials`.

Luego vuelve a la ruta de nuestra gema recien creada y ejecuta el siguiente comando `gem push holita-0.0.0.gem` y dentro de unos minutos podras ver tu gema publicada en Rubygems (puedes ver la que yo hice [aqui](https://rubygems.org/gems/holita){:target="____blank"}), podemos probar que todo funciona ejecutando `gem install 'holita'` o usándola como vimos en nuestro post de [bundler](/2020/01/30/bundler.html).

En la pagina de Rubygems se puede revisar guias mas extensas sobre como realizar pruebas, convenciones y otras cosas.

### Conclusión

Uno de los grandes fuertes de Ruby es su comunidad y las gemas es una excelente forma de compartir clases reutilizables, vemos que cualquiera puede comenzar sin muchos requisitos.

Espero que haya sido de ayuda y comienza a desarrollar gemas!!

---
title: Crear ejecutable de gema.
layout: post
read_time: 2 min.
locale: es
---
Así como el ejecutable de `rails` y otros mas, nosotros también podemos realizar nuestros propios ejecutables ruby a través de una gema que compilemos.

Para continuar ya deberías saber como compilar y publicar una en RubyGems, si aun no lo sabes podes revisar [mi post](/2020/06/12/gem) donde voy paso por paso.

### Gema

Primero haremos una modificación a nuestra gema anterior para recibir un nuevo parámetro en el método principal `hi`

{% highlight ruby linenos %}
def self.hi(name)
  name = 'mundo' if name.nil? || name.empty?
  puts "Holita #{name}!"
end
{% endhighlight %}

Hacemos esto para poder pasarle el parámetro que recibiremos en la terminal.

##### bin

Ahora en la raíz del proyecto, crea una carpeta llamada `bin` y dentro un archivo de texto llamado igual que tu gema (en nuestro caso es `holita`), dentro de este archivo tendremos el siguiente contenido
{% highlight ruby linenos %}
#!/usr/bin/env ruby

require 'holita'
puts Holita.hi(ARGV[0])
{% endhighlight %}

Vemos que le pasaremos al método el primer argumento que recibamos en la terminal. Para probar que realmente funciona antes de publicar, podemos ejecutar el comando `ruby -Ilib ./bin/holita nelson` y si vemos la respuesta **Holita nelson!** entonces esta todo bien.

### Publicación

La publicación se hace de la misma manera que ya conocemos pero aun falta que agreguemos el archivo ejecutable en nuestro `gemspec` así que debería quedar de la siguiente manera
{% highlight ruby linenos %}
Gem::Specification.new do |s|
  s.name        = 'holita'
  s.version     = '1.0.1'
  s.executables << 'holita'
  s.date        = '2020-12-06'
  s.summary     = 'Holita!'
  s.description = 'Gema para post de rubyvania'
  s.authors     = ['Tu nombre']
  s.email       = 'Tu correo electronico'
  s.files       = ['lib/holita.rb']
  s.homepage    = 'https://rubygems.org/gems/holita'
  s.license = 'MIT'
end
{% endhighlight %}

Una vez realizada la publicación, solo tenés que instalarla en tu computadora como cualquier otra gema (`gem install holita`) y desde cualquier ubicación podrás llamar a tu ejecutable `holita`

### Conclusión

Realizar una herramienta de comandos no es tan difícil como suena, obviamente no es como un ejecutable completamente independiente pero sigue siendo una excelente opción para poder entregar una funcionalidad empaquetada.

Espero que haya sido de ayuda y comienza crear herramientas de comandos!!

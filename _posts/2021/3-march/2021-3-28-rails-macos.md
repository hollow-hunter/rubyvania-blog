---
title: Usar Rails en MacOS
layout: post
read_time: 5 min.
locale: es
---

Me di cuenta que el post sobre como usar Rails esta especifico para Windows, asi que veamos como tener Rails en MacOS.

Asumo que ya tenés [instalado Ruby en tu Mac](/2020/08/20/macos) así que iremos directo a la instalación de Rails.

{:.mt-5 .mb-3}
### Rails

Instalaremos Rails de la manera común ya que al final de todo Rails es solo una gema mas para Ruby, comencemos abriendo una ventana de comandos y ejecutando `gem install rails`, puede tomar un poco de tiempo ya que es un framework completo.
Una vez finalizado sin mensajes de error, podes ejecutar `rails -v` para ver la version de Rails que acabás de instalar, al momento de edición de este post, Rails esta en la version 6.1.3.1    
Si instalaste Ruby siguiendo mi anterior post, tendrás que ejecutar `rbenv rehash` para que **rbenv** reconozca el comando de Rails.

A partir de Rails 6 se utiliza webpack para servir archivos javascript, y los paquetes son instalados con **yarn** por lo tanto antes de crear una aplicación de prueba necesitamos instalarlo en conjunto con **node** así que vayamos en orden, primero instalaremos node mediante brew, ejecuta `brew install node` para instalar la version mas reciente. Cuando finalice podes verificar la version con `node -v` y `npm -v` para el gestor de paquetes node.    
En cuanto a **yarn**, Rails aun no soporta Yarn 2.0 (provisto como paquete node) así que también sera instalado mediante brew con el comando `brew install yarn`, verifica la instalación ejecutando `yarn --version` una vez finalice.

Con todo lo mencionado instalado, como siguiente prueba que todo esta correcto, podemos crear una nueva aplicación Rails, para eso ve a una carpeta donde tengas o vayas a tener tus proyectos y ejecuta el comando `rails new` seguido del nombre de tu aplicación, por ejemplo: `rails new demo_app`, esto creara una nueva carpeta llamada **demo_app** con todas los archivos necesarios para una aplicación Rails.

Una vez finalizado, ejecutamos el comando `rails s` dentro de la carpeta recién creada, luego navegamos desde Safari a [localhost:3000](http://localhost:3000){:target="_blank"} y veremos la pantalla de bienvenida de Rails

{:.mt-5 .mb-3}
##### PostgreSQL

Si bien la base de datos por defecto (SQLite) es suficiente para pruebas, para el desarrollo es preferible utilizar un motor como PostgreSQL así que adivina como lo instalaremos... usando brew. Ejecutemos el siguiente comando `brew install postgres`, toma en cuenta que esto no instala una gestor con interfaz gráfica, para verificar la instalación correcta verifica la version con el comando `psql --version` y para iniciar el servicio se utiliza como servicio de brew ejecutando `brew services start postgres` para iniciarlo y `brew services stop postgres` para detenerlo.    
Por defecto se crea un usuario sin contraseña con tu usuario Mac actual, si no estas seguro cual es podes ejecutar el comando `psql` para iniciar el shell Postgre y dentro ejecuta el comando `\du` para ver la lista de usuarios, veras algo como esto

![welcome](/assets/images/posts/rails-macos/postgre.png){:.img-fluid}

Entonces en tu archivo `config/database.yml` solo específicas el usuario

{% highlight yaml linenos %}
adapter: postgresql
username: nelsonuribe
password:
{% endhighlight %}

A partir de eso ya podes seguir casi cualquier tutorial o post sobre Rails, independientemente de en que sistema operativo haya sido realizado.

{:.mt-5 .mb-3}
### Conclusión

Homebrew es realmente una utilidad impresionante, pudimos instalar todas nuestras herramientas necesarias usandolo e incluso se encarga de configuraciones como usuarios y servicios sin causarnos dolores de cabeza. Es probable que ya te hayas dado cuenta (si has visto mis anteriores posts) que he dejado de usar Windows hace mucho y puedo asegurarte que no lo extraño ni un poco pero eso ya queda fuera de tema.

Espero que haya sido de ayuda y empieza a usar Rails en tu Mac!!

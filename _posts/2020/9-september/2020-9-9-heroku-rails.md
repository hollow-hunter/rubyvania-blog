---
title: Como publicar Rails en heroku
layout: post
read_time: 10 min.
locale: es
---

Es momento de publicar en heroku la aplicacion Rails que hemos estado desarrollando en el blog.

Podes ver post por post a continuación
- [Haciendo una aplicación en Rails](/2020/03/28/stock-app)
- [Haciendo una aplicación en Rails - Parte 2](/2020/04/15/stock-app-2)
- [Haciendo una aplicación en Rails - Parte 3](/2020/05/17/stock-app-3)
- [Haciendo una aplicación en Rails - Parte 4](/2020/06/21/stock-app-4)

### Heroku

Asumo que ya tenes una cuenta de heroku asi que iremos directamente a crear una aplicación ya sabemos como hacer esto ya que creamos una cuando [publicamos nuestros bots](/2020/07/25/heroku).

Una vez tengas la aplicación disponibles, agregaremos un add-on el cual es nuestra base de datos en PostgreSQL llamada Heroku Postgres aquí, y no te preocupes hay una versión gratis con sus limitaciones obviamente.

![add on](/assets/images/posts/heroku-rails/add-on-installing.png){:.img-fluid}

Luego veras el add-on ya agregado a tu aplicación y por ultimo toca publicar el código de la aplicación y usaremos el mismo método de antes con el [heroku cli](https://devcenter.heroku.com/articles/heroku-command-line){:target="____blank"}. Asegurate de tener la gema `pg` en el Gemfile.

Al igual que nuestros bots, aqui tambien necesitamos un Procfile, asi que crea uno en la raiz del proyecto con el siguiente contenido

{% highlight ruby linenos %}
web: bundle exec puma -t 5:5 -p ${PORT:-3000} -e ${RACK_ENV:-development}
{% endhighlight %}

Como ves, heroku recomienda el uso del servidor `puma` asi que si no tenes la gema en tu Gemfile, agrégala.

No tenes que preocuparte por cadenas de conexión ni nada de eso, ya que heroku establece una variable de entorno llamada `DATABASE_URL` que Rails detecta automáticamente para conectarse a la base de datos ignorando cualquier configuración del archivo `database.yml`.

Cuando termine la publicación ve al dashboard de heroku y en la parte superior derecha busca la opción que dice **More**, se desplegará una lista de opciones y selecciona **Run console** ya que necesitamos aplicar las migraciones de Rails de la misma manera que lo hacemos localmente `rails db:migrate` (también se puede hacer usando el heroku cli pero esto me parece mas cómodo)

Con las migraciones aplicadas correctamente simplemente navega a las rutas que ya conoces de nuestra aplicación; `/categories`, `/products`, etc. esto porque aun no hemos establecido una pagina inicial. Deberías poder registrar datos sin problemas.

![production](/assets/images/posts/heroku-rails/app_running.png){:.img-fluid}

### Conclusión

Obviamente nuestra aplicación aun es muy sencilla, no tenemos autenticación, estilos, estructura de paginas y un montón de cosas mas que iremos agregando en post futuros, pero cumplimos nuestro objetivo el cual es tener la aplicación publicada en internet todo completamente gratis y sin problemas. Podes leer mas en la [documentación de heroku](https://devcenter.heroku.com/categories/rails-support){:target="____blank"}

Espero que haya sido de ayuda y comienza a publicar aplicaciones web en Heroku!!

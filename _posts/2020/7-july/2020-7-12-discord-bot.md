---
title: Como hacer un bot para discord - Julio de Bots
layout: post
read_time: 5 min.
locale: es
---

Veremos como hacer un simple bot para discord y que haga matemáticas sencillas.

Este es el segundo post de la serie donde crearemos bots todo el mes de Julio y al final veremos como publicarlos en heroku. Podes ver los otros bots en la siguiente lista:
- [Como hacer un bot para reddit](/2020/07/04/reddit-bot)
- [Como hacer un bot para twitter](/2020/07/18/twitter-bot)

### Preparación de la cuenta

Asumo que ya tenes una cuenta de Discord y que la usaras para gestionar tu bot, asi que dirigete a [https://discord.com/developers/applications](https://discord.com/developers/applications){:target="____blank"} para crear una nueva aplicacion usa el nombre que desees.

Una vez crees la aplicación podrás ver el **Client Id**, esto lo necesitaremos para agregar el bot a nuestro servidor, ademas tenemos un menú a la izquierda con un elemento que dice Bots, debemos ingresar a esa opción para crear nuestro Bot recibirás una advertencia que una vez creas un Bot no podrás eliminarlo, aceptalo y nombra a tu Bot, en esta pantalla también veras el **token** que usaremos en nuestro script.

Ahora es momento de agregar el Bot a nuestro servidor de Discord, para eso debes ingresar a la siguiente dirección para generar un link de invitación [https://discordapi.com/permissions.html](https://discordapi.com/permissions.html){:target="____blank"}, veras una lista de permisos y lo único que necesitamos este post son los de Leer y Enviar Mensajes.

Debajo de los permisos veras un campo donde copiaremos el Client Id de nuestro Bot y al final veremos un link el cual haremos click y Discord te pedirá confirmación para agregar el Bot a uno de tus servidores. Deberías poder ver al Bot en estado offline.

### discordrb

Ahora codificaremos nuestro script y para eso usaremos la gema `discordrb` que puedes revisar en [github](https://github.com/discordrb/discordrb){:target="____blank"} para otras funcionalidades que puedan servirte. Ahora veamos nuestro código

{% highlight ruby linenos %}
# frozen_string_literal: true

require 'discordrb'

bot = Discordrb::Bot.new token: 'tu_token'

bot.message(start_with: '!calc') do |event|
  # !calc 1 + 2
  #    0  1 2 3
  args = event.content.split ' '
  puts args
  a = args[1].to_f
  b = args[3].to_f
  case args[2]
  when '+'
    result = a + b
  when '-'
    result = a - b
  when '*'
    result = a * b
  when '/'
    result = a / b
  else
    result = 0
  end
  event.respond "The result is #{result}"
end

bot.run
{% endhighlight %}

Como mencione al inicio del post, nuestro Bot hará cálculos simples así que recibirá dos parámetros separados por el símbolo de operación que se desea hacer.

Veras que inicializamos la sesión con el token del Bot y luego le decimos que a cualquier mensaje que inicie con '!calc' intente realizar el calculo y responda con el resultado. Solo ejecuta el script y veras algo como esto
![rubyvania bot](/assets/images/posts/discord-bot/live.png){:.img-fluid}

##### Servidor Rubyvania

Para la realización de este post tuve que crear mi propio servidor así que aproveche y cree un servidor para el blog, si deseas unirte podes usar el siguiente [enlace](https://discord.gg/vnbCpUZ){:target="____blank"} no tendrá mucha actividad al principio pero podrás encontrarme allí y crecer juntos.

### Conclusión

Al igual que reddit, Discord es bastante autónomo para la creación Bots aunque requiere saber un poco mas sobre los permisos necesarios para el funcionamiento adecuado de nuestro Bot, la gema que usamos tambien soporta opciones para audio así que te recomiendo explorarla mas.

Espero que haya sido de ayuda y comienza a desarrollar bots en discord!!

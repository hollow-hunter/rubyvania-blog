---
title: Como hacer un bot para Twitter - Julio de Bots
layout: post
read_time: 5 min.
locale: es
---

Realizaremos un simple bot que twittear y retwitteara cualquier tweet que contenga el hashtag #rubyvania

Este es el ultimo post de la serie donde crearemos bots todo el mes de Julio y al final veremos como publicarlos en heroku. Podes ver los otros bots en la siguiente lista:
- [Como hacer un bot para reddit](/2020/07/04/reddit-bot)
- [Como hacer un bot para discord](/2020/07/12/discord-bot)

### Cuenta de Desarrollador

Para poder desarrollar un bot en twitter necesitas aprobación de ellos, esto se realiza rellenando un [formulario](https://developer.twitter.com/en/apply-for-access){:target="____blank"} y debe ser aprobado manualmente por ellos. Tardara unos dias y te confirmaran por correo, te recomiendo responder a sus consultas con honestidad.

Una vez tengas acceso al panel, deberás crear una app para obtener 4 valores; un API Key y su Secret, un Bearer Token y su Secret.
Antes de generar estos últimos dos, asegurate de darle [permisos de escritura](https://developer.twitter.com/en/docs/basics/apps/guides/app-permissions){:target="____blank"} a tu app, además para generar estas se debe asociar a una cuenta aqui es donde debes asociarla a tu cuenta bot, en mi caso solo lo asocie a mi mismo.

### Gema twitter

Ahora que tenemos todos estos datos, podemos hacer uso de la [gema](https://github.com/sferik/twitter){:target="____blank"} Ruby que nos facilita la comunicación con la API de Twitter.

Veamos el primer codigo para twittear
{% highlight ruby linenos %}
require 'twitter'

client = Twitter::REST::Client.new do |config|
  config.consumer_key        = 'tu_api_key'
  config.consumer_secret     = 'tu_secret_key'
  config.access_token        = 'tu_access_token'
  config.access_token_secret = 'tu_secret_token'
end

client.update("I'm tweeting with @gem!")

{% endhighlight %}

Inicializamos la conexión y el método `update` envia el tweet con el contenido que le indicamos.

Ahora usaremos esta variable `client` para retwittear cualquier uso del hashtag #rubyvania

{% highlight ruby linenos %}
stream_client = Twitter::Streaming::Client.new do |config|
  config.consumer_key        = 'tu_api_key'
  config.consumer_secret     = 'tu_secret_key'
  config.access_token        = 'tu_access_token'
  config.access_token_secret = 'tu_secret_token'
end

topic = '#rubyvania'
stream_client.filter(track: topic) do |object|
  client.retweet(object) if object.is_a?(Twitter::Tweet)
end
{% endhighlight %}

Declaramos una nueva variable `stream_client` que se encargara de escuchar los nuevos tweets que contengan el valor de variable `topic`.

### Conclusión

Aunque obtener la cuenta de desarrollador de Twitter no es tan automático como reddit o discord igual contamos con una API extensa (y una gema muy útil) para poder realizar muchas acciones con la data. Eso si, con la debida autorización de Twitter.

Espero que haya sido de ayuda y comienza a desarrollar bots en Twitter!!

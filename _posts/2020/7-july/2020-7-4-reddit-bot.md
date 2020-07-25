---
title: Como hacer un bot para reddit - Julio de Bots
layout: post
read_time: 5 min.
locale: es
---

En este primer post veremos como hacer un simple bot para reddit, para que responda a comentarios con ciertos comandos.

Este es el primer post de una serie donde crearemos bots todo el mes de Julio y al final veremos como publicarlos en heroku. Podes ver los otros bots:
- [Como hacer un bot para discord](/2020/07/12/discord-bot)
- [Como hacer un bot para twitter](/2020/07/18/twitter-bot)

### Redd

Al igual que muchas plataformas web, Reddit expone un API para la automatización de procesos, esta se puede consumir directamente con HTTP o (de la forma mas recomendada) con una librería wrapper que en nuestro caso sera [Redd](https://github.com/avinashbot/redd){:target="____blank"}, aunque la pagina de Github tiene un ejemplo bastante directo requiere ciertos datos que se deben obtener de la cuenta de reddit que sera el bot.

Asumo que ya tenes la cuenta de reddit así que ingresa al sitio y luego dirígete a la siguiente dirección [https://old.reddit.com/prefs/apps/](https://old.reddit.com/prefs/apps/){:target="____blank"} y veras un botón que dice 'crear otra aplicación' para crear un Id y Clave Secreta que necesitara nuestro bot, en el formulario te pedirá un nombre, puedes llamarlo como quieras; el tipo de aplicación, aquí elige 'script'; puedes rellenar lo que quieras en la descripción aunque no es requerida; y por ultimo en la uri de redireccion puedes usar http://localhost:3000 ya que se trata de un script local.

Cuando presiones el botón 'crear aplicación' podrás ver el id y clave secreta asignado
![reddit created](/assets/images/posts/reddit-bot/created.jpg){:.img-fluid}

Ahora crearemos el script, instala la gema 'redd' mediante bundler y usaremos el siguiente código de ejemplo
{% highlight ruby linenos %}
require 'redd'

session = Redd.it(
  user_agent: 'Redd:AppleOrangeBot:v1.0.0', # Nombre de tu bot
  client_id:  'tu_id',
  secret:     'tu_secret',
  username:   'tu_username',
  password:   'tu_contraseña'
)
keyword = 'apple'
value = 'orange'

session.subreddit('all').comment_stream do |comment|
  comment_text = comment.body
  if comment_text.include?(keyword)
    puts "Replying to: #{comment_text}"
    my_answer = comment_text.gsub(keyword, value)
    comment.reply(my_answer)
  end
end
{% endhighlight %}

Revisemos lo que realiza el codigo, primero creamos la sesion con todas las credenciales necesarias, luego declaramos una palabra clave (`keyword`) que sera reemplazada por el valor (`value`), despues iniciamos algo parecido a un ciclo infinito que lee los comentarios entrantes de un subreddit, aunque en este caso recibiremos los comentarios de todo reddit apuntando a r/all. Entonces tenemos el comentario en la variable `comment` y podemos leer el contenido para reemplazar la palabra clave y al final con ese resultado responder al comentario original.

### Conclusión

Bots en reddit son bastantes requeridos para distintos objetivos; responder con memes, proveer información de APIs y hasta procesar videos. Solo dale un vistazo a [https://www.reddit.com/r/RequestABot/](https://www.reddit.com/r/RequestABot/){:target="____blank"} para saber lo que requieren.

Espero que haya sido de ayuda y comienza a desarrollar bots en reddit!!

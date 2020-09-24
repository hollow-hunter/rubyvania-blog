---
title: Definir pagina inicial en Rails
layout: post
read_time: 5 min.
locale: es
---

Realicemos una pagina inicial para nuestra aplicación de Rails.

Podes ver todo el progreso de esta aplicación a continuación
- [Haciendo una aplicación en Rails](/2020/03/28/stock-app)
- [Haciendo una aplicación en Rails - Parte 2](/2020/04/15/stock-app-2)
- [Haciendo una aplicación en Rails - Parte 3](/2020/05/17/stock-app-3)
- [Haciendo una aplicación en Rails - Parte 4](/2020/06/21/stock-app-4)
- [Como publicar Rails en heroku](/2020/09/09/heroku-rails)

### Routes

Ya hemos visto algo del archivo `config/routes.rb` y ahora agregaremos la pagina inicial de nuestra con esta linea

{% highlight ruby %}
root 'pages#index'
{% endhighlight %}

Y te habrás dado cuenta que ese controlador no existe así que ejecutemos el comando correspondiente para crearlo `rails g controller Pages index` y luego en el archivo `app/views/pages/index.html.erb` usaremos este contenido

{% highlight html linenos %}
<h1>Home</h1>
<ul>
  <li>
    <a href="/categories">Categories</a>
  </li>
  <li>
    <a href="/products">Products</a>
  </li>
  <li>
    <a href="/storages">Storages</a>
  </li>
  <li>
    <a href="/stocks">Stocks</a>
  </li>
</ul>
{% endhighlight %}

Así podemos navegar a todas los listados que tenemos en la aplicación.    
Ya de aquí podes agregar un enlace de navegación a todos los listados para esta pantalla.

### Conclusión

Como en anteriores posts vemos que la configuración de rutas es bastante versátil y sencilla. Si fueramos a utilizar otra pagina como pantalla inicial simplemente cambiamos una linea de codigo para apuntar a nuestra nueva vista.

Espero que haya sido de ayuda y comienza a crear paginas iniciales para Rails!!

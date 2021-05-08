---
title: Como enviar multipart/form en Ruby
layout: post
read_time: 2 min.
locale: es
---

Ya vimos como enviar datos un API en formato JSON pero algunas pueden requerir la data en otros formatos, en este caso, veremos el formato `multipart/form`.

{:.mt-5 .mb-3}
### Multipart/form

Si tenes experiencia con HTML te habras dado cuenta que este formato se utiliza cuando se necesita subir uno o varios archivos. Ahora veamos como podemos hacer este mismo tipo de peticion desde codigo Ruby, para eso utilizaremos la gema `http` que viene incluida en la libreria estandar de Ruby.    
Ademas esta gema incluye una clase helper para que enviamos un archivo dentro del formulario, asi que el codigo resultante es de lo mas sencillo
{% highlight ruby linenos %}
require 'http'
response = HTTP.post('https://api.everypixel.com/v1/keywords',
                     form: { data: HTTP::FormData::File.new('./image.jpeg') })
puts response.body
{% endhighlight %}

En nuestro ejemplo estamos haciendo uso del API de everypixel que requiere la imagen dentro de un `multipart/form` en una variable llamada `data`, el valor de esta variable es la clase helper `HTTP::FormData::File` que carga nuestro archivo. Vemos que al inicializar la variable `form`, el metodo `post` ya sabe que se trata de un `multipart/form`.

{:.mt-5 .mb-3}
### Conclusión

Obviamente esta solucion es la mas simple y directa, tal vez necesites encapsular esto en una clase para soportar varios escenarios como autorizacion y multiples archivos, en un siguiente post veremos como utilizar la gema **OAuth2** para autenticarnos contra un API.

Espero que haya sido de ayuda y comienza a subir archivos!!

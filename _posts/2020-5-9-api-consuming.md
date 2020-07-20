---
title: Consumo de APIs
layout: post
read_time: 4 min.
locale: es
---
La integracion entre sistemas es cada vez mas comun y la forma que estos sistemas se comunican es a traves de APIs, en este post veremos como consumirlas.

### Libreria Net/Http

Para poder hacer llamadas HTTP necesitamos cargar las clases necesarias con la siguiente linea `require 'net/http'`, puedes usar la clase `http` directamente en tu codigo pero en este post haremos una clase que puede ser reutilizada con distintas URLs, asi que primero declararemos nuestro objeto `http`.

{% highlight ruby linenos %}
require 'net/http'

class ApiClient
  def initialize(url_base, port)
    @request = Net::HTTP.new(url_base, port)
    @request.use_ssl = port == 443
  end
end
{% endhighlight %}

Vamos a recibir una base URL y el puerto al que nos conectaremos, tambien estamos diciendole que si el puerto es 443 significa que la peticion sera realizada por SSL (HTTPS). Nuestro objeto `@request` ya tiene los metodos que utilizaremos y son los estandar para APIs RESTfull (GET, POST, PUT, DELETE), todos estos metodos devuelven un objeto `HTTPResponse` que, valga la redundancia, contiene los datos de respuesta que nos de la API pero los mas importantes son el `code` y el `body`.

Ahora crearemos nuestro primer metodo que realizara peticiones GET

{% highlight ruby linenos %}
def get(route)
  JSON.parse(@request.get(route).body, symbolize_names: true)
end
{% endhighlight %}

Recibiremos la ruta donde se realizara el GET, luego obtenemos el contenido de la respuesta y como casi todas las APIs responden en JSON (si preferis XML podes irte de mi blog ahora mismo) pasaremos el `body` directamente a la clase `JSON` para leer el contenido como un hash (podes leer mi post sobre JSON [aqui](/2020/04/26/json)).

Antes de crear nuestro metodo para peticiones POST haremos un metodo auxiliar que mapeara la data que necesitamos enviar ya que el metodo `post` necesita la data en este formato `'key=value'`, asi que nuestro metodo auxiliar recibira un hash y lo convertira a dicho formato
{% highlight ruby linenos %}
private

def stringify(data)
  plain_data = []
  data.each_pair do |key,value|
    plain_data << "#{key}=#{value}"
  end
  plain_data.join(',')
end
{% endhighlight %}

Una vez declarado, podemos implementar el metodo post
{% highlight ruby linenos %}
def post(route, data)
  @request.post route, stringify(data)
end
{% endhighlight %}

No recomiento parsear el JSON directamente ya que una peticion POST puede tener distintas respuestas, asi que mejor exponer directamente el objeto respuesta.

El metodo PUT y el DELETE no tienen nada raro asi que veamos la implementacion directamente
{% highlight ruby linenos %}
def delete(route)
  @request.delete route
end

def put(route, data)
  @request.put route, stringify(data)
end
{% endhighlight %}

Ahora veamos como usar realmente nuestra clase
{% highlight ruby linenos %}
require_relative 'api_client'
client = ApiClient.new('api.github.com', 443)
puts client.get('/users/uribenelson')
{% endhighlight %}

##### Headers

Muchas APIs leen parametros desde el header de la peticion, uno especificamente especial es el de Authentication, todos los metodos que vimos de clase `http` pueden recibir headers en forma de `hash` asi que tengamos uno dentro de nuestra clase y lo usaremos en todos nuestros metodos, veamos como lo declaramos y como se usaria en el metodo `get` (es basicamente lo mismo para los otros metodos)
{% highlight ruby linenos %}
attr_accessor :headers

def initialize(url_base, port)
  @request = Net::HTTP.new(url_base, port)
  @request.use_ssl = port == 443
  @headers = {}
end

def get(route)
  JSON.parse(@request.get(route, @headers).body, symbolize_names: true)
end
{% endhighlight %}

Eso seria todo para cubrir las peticiones basicas, obviamente el tema puede ser mas complejo lidiando con timeouts, cache y otras cosas. Si queres ver o descargar la clase completa te dejo un enlace github [aqui](https://gist.github.com/UribeNelson/2a4bd95a3bd6790c874e59a03ee78793){:target="_blank"}

### Conclusion

Como en los otros posts vemos que Ruby tiene herramientas ya incluidas en su libreria estandar para realizar peticiones HTTP, obviamente existen gemas que puedan hacer el trabajo de maneras diferentes pero es bueno tener la posibilidad de hacerlo directamente sin dependencias de terceros.

Espero que haya sido de ayuda y comienza a consumis APIs!!

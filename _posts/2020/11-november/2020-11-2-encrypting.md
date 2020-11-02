---
title: Encriptacion simetrica en Ruby.
layout: post
read_time: 2 min.
locale: es
---
Veamos como encriptar y desencriptar en Ruby.

### OpenSSL
Para trabajos de encriptacion Ruby utiliza la librería libre OpenSSL y apropiadamente tiene su gema incluida en la librería estándar de Ruby.    
Luego solo tenemos que inicializar un objeto de encriptacion `AES256` y entregarle los parámetros necesarios para encriptar

{% highlight ruby linenos %}
require 'openssl'
cipher = OpenSSL::Cipher::AES256.new
cipher.encrypt
# Generamos un vector aleatorio, este debería ser almacenado
iv = cipher.random_iv
# Guardaremos el resultado en un archivo
f = File.open('result.txt', 'w')
# Nuestra llave debe tener 32 caracteres
cipher.key = 'q1w2e3r4t5y6u7i8o9p0Q!W@E#R$T%Y^'
# Encriptamos el contenido
cipher_text = cipher.update('Mensaje encriptado en Ruby') + cipher.final
# Y escribimos el resultado en el archivo de texto
f.puts cipher_text
f.close
{% endhighlight %}

Ahora si abres el archivo veras algo como **ﬁ˚9E›•∂pÌjÊÿÙ–Ô	:Æ:ÊŸ kB;\4÷º}**, no lo mostramos en consola porque el enconding no puede procesar ese tipo de caracteres.

Ahora si queremos desencriptar es básicamente lo mismo solo que cambiamos el modo llamando al método `decrypt`, asi que expandamos el código anterior

{% highlight ruby linenos %}
cipher.decrypt
# Utilizamos el vector anteriormente generado
cipher.iv = iv
cipher.key = 'q1w2e3r4t5y6u7i8o9p0Q!W@E#R$T%Y^'
# Ahora utilizamos el texto encriptado anteriormente
plain_text = cipher.update(cipher_text) + cipher.final
# Y luego lo escribimos en el archivo resultado
f.puts plain_text
f.close
{% endhighlight %}

Al final tu archivo resultado deberá tener ambos textos, el encriptado y el original desencriptado.

### Conclusión

La encriptacion es una excelente medida de seguridad pero hay que tomar en cuenta que agrega carga y tiempo a cualquier proceso así que aunque sea bastante fácil utilizarlo, debemos analizar si existe alguna alternativa (eg. HTTPS) que no afecte el proceso normal de nuestra aplicación.

Espero que haya sido de ayuda y comienza encriptar!!

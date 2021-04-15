---
title: Como crear archivos Zip en memoria
layout: post
read_time: 2 min.
locale: es
---

Este post sera bastante corto ya que anteriormente vimos como crear un archivo Zip directamente en disco, ahora lo tendremos en memoria.    
Asumire que ya sabes (o seguiste mi [post](/2021/03/14/zips)) como utilizar la libreria estandar **zip** para crear archivos en disco.

{:.mt-5 .mb-3}
### OutputStream

Vayamos directo al grano, a diferencia del anterior post aqui tenemos que utilizar la clase `OutputStream` para tener el Zip dentro de un buffer.    
El siguiente metodo es como se realizaria en una aplicacion Rails
{% highlight ruby linenos %}
require 'zip'

def make_zip
  # Obtenemos la data que queremos exportar
  staffs = Staff.all
  # Abrimos el buffer en memoria (lo mas importante)
  file_stream = Zip::OutputStream.write_buffer do |zip|
    staffs.each do |a|
      # Agregamos el elemento en nombre
      zip.put_next_entry("#{a.name}.txt")
      # Y luego el contenido del elemento
      zip.print a.name
      # Si quisieramos agregar un archivo ya existente, utilizamos la clase IO
      zip.print IO.read(a.path)
    end
  end
  # Como estamos lidiando con una stream, volvemos el puntero al inicio
  file_stream.rewind
  # Para que sea descargada por Rails
  send_data file_stream.read, filename: 'staffs.zip'
end
{% endhighlight %}

Mientras realizaba este post me encontre primero con una forma para utilizar archivos temporales pero esta manera es mucho mas limpia y directa. Hay que tener cuidado con los metodos a utilizar ya que no son los mismo que utilizamos antes.

{:.mt-5 .mb-3}
### Conclusión

Como mencione en el anterior post de Zips, la mayoria de los casos no se guardaria en disco sino que se utilizaria para descargarlo o enviarlo por correo electronico y vemos que no hay mucha diferencia para lo que ya hicimos.

Espero que haya sido de ayuda y comienza a crear Zips en memoria!!

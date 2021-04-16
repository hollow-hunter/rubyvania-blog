---
title: VSCodium para Ruby en MacOS
layout: post
read_time: 5 min.
locale: es
---
Veamos como podemos tener la experiencia de VSCode en MacOS sin la telemetria de Microsoft.

{:.mt-5 .mb-3}
### VSCodium

Seguramente te preguntaras, ¿que es VSCodium y por que deberia importarme?. Bueno, primero podemos comenzar con preguntarte a vos mismo(a) *¿por que no uso Windows?*; es probable que tu respuesta sea o incluya algo como "no uso o no quiero usar productos Microsoft" y cada uno tiene sus propias razones, pero eso conlleva a la pregunta de VSCodium inicial.    
VSCode es codigo abierto pero lo que descargas de la pagina de Microsoft de VSCode no lo es. Basicamente lo que hacen es descargar la rama del repositorio de VSCode, insertar sus librerias propietarias para telemetria y otras cosas, y eso llega a ser lo que descargas de la pagina de VSCode.    
VSCodium hace lo mismo excepto que no inserta ninguna libreria propietaria asi que la compilacion resultante termina siendo limpia de todo codigo de Microsoft (Ya se que tecnicamente VSCode fue creado por Microsoft pero entendes a lo que me refiero).    
Si ya usas Windows tal vez esto no sea tan relevante para vos, es por eso que este post esta dirigido a usuarios Mac.

{:.mt-5 .mb-3}
##### Instalacion

La instalacion es lo mas sencilla, ejecutando un comando brew
{% highlight shell %}
brew install --cask vscodium
{% endhighlight %}
Es posible que te solicite credenciales de github ya que como mencione anteriormente, se clona el codigo base de VSCode para compilarlo. Cuando termine lo veras en tu Launchpad
![vscodium launchpad](/assets/images/posts/vscodium/vscodium_logo.png){:.img-fluid,:height="50px"}

Y al iniciarlo veras que es exactamente igual que VSCode incluso con las extensiones!! asi que instalemos algunas que nos ayudaran con Ruby, en preparacion instala las siguientes gemas
- {:.mb-2} `gem install ruby-debug-ide`
- {:.mb-2} `gem install byebug`
- {:.mb-2} `gem install solargraph`

Las primeras dos son para la extension que nos ayudara en hacer debugging con la extension [Ruby](https://open-vsx.org/extension/rebornix/ruby){:target="_blank"} y la tercera es para autocompletar (con ciertas limitantes) mientras codificamos con la extension [Ruby Solargraph](https://open-vsx.org/extension/castwide/solargraph){:target="_blank"}. Desde este punto podes seguir las indicaciones de VSCodium para crear el archivo `launch.json` o incluso podes seguir la segunda parte de mi [anterior post de VSCode](/2020/01/11/vscode).

{:.mt-5 .mb-3}
### Conclusión

Quizas penses que tener esta version es innecesaria pero hay mucha gente que le importa su privacidad y seguir una mentalidad en su estilo de vida, asi que tener la opcion es lo mas importante de esto, y que nadie te obligue a hacer o dar tu data de una manera que no desees. Por lo tanto, tener las geniales funcionalidades de VSCode sin compromisos es lo mas valioso de este proyecto.

Espero que haya sido de ayuda y comienza a programar en VSCodium!!

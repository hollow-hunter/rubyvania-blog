---
title: Instalar Ruby en Windows
layout: post
read_time: 3 min.
locale: es
---
Veremos como instalar Ruby en Windows a través de la herramienta llamada Ruby Installer, si estas aqui es porque ya habrás leído que tener Ruby en Windows no es tan sencillo como otros lenguajes de programación, asi que vayamos paso por paso.

{:.mt-5 .mb-3}
### RubyInstaller

Lo primero que debes hacer es ingresar a [https://rubyinstaller.org/downloads/](https://rubyinstaller.org/downloads/) y descargar la version mas estable, al momento de edición la version más estable sugerida por el mismo sitio web es 2.6.5-1.

![Versiones de Ruby](/assets/images/posts/installation/ruby_version.jpg){:.img-fluid}

Es importante que uses la version con DevKit ya que esta herramienta se encarga de compilar las Gemas para que funcionen en Windows.
Una vez descargado solo ejecutás el instalador y sigue las siguientes instrucciones:

1. {:.mb-2}Acepta la licencia.
2. {:.mb-2}Recomiendo que mantengas la ruta de instalación pero podés cambiarla a gusto. Mantené los checkboxes marcados por defecto.
3. {:.mb-2}Deja todo marcado, MSYS2 es muy importante para las Gemas.
4. {:.mb-2}Asegurate de que la checkbox para ejecutar la configuración de MSYS2 esté marcada en el ultimo paso antes de hacer click en Finish

Al final se abrirá una ventana de consola con el siguiente contenido

![Instalacion MSYS2](/assets/images/posts/installation/msys2_installation.jpg){:.img-fluid}

Simplemente presionando Enter como indica el prompt es suficiente. Si todo va bien se iniciara la instalación de la siguiente herramienta llamada **pacman**, dale un poco de tiempo para que todo se complete.

![Instalacion MSYS2 Final](/assets/images/posts/installation/msys2_installation_2.jpg){:.img-fluid}

De nuevo habrá un prompt de instalación, solo presiona Enter y finalizara todo.

Con eso ya tenés disponible Ruby en Windows, podés probarlo ejecutando `ruby -v` en una ventana de consola para verificar la version instalada.

![Ruby Instalado](/assets/images/posts/installation/ruby_installed.jpg){:.img-fluid}

Incluso podés iniciar la consola interactiva de Ruby que te permite escribir codigo Ruby en linea, busca la herramienta Interactive Ruby o inicia el comando `irb`

![IRB](/assets/images/posts/installation/irb.jpg){:.img-fluid}

Espero haber sido de ayuda, en el siguiente post veremos como configurar Visual Studio Code para programar y debuggear nuestras aplicaciones.

Hasta luego!
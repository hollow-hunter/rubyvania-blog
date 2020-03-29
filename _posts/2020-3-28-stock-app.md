---
title: Haciendo una aplicación en Rails
layout: post
read_time: 10 min.
locale: es
---
Como mencionamos en nuestro post de instalación de Rails, este framework web es sencillo para desarrollar buenas aplicaciones, asi que en esta serie de posts iremos paso por paso para completar una aplicacion Rails no muy compleja.

### Aplicación para Almacenes

Desarrollaremos una aplicacion sencilla para registrar productos en almacenes, algo básico para comenzar un sistema de inventario. Estos serán nuestros modelos:

![diagram](/assets/images/posts/stock-app/diagram.jpg){:.img-fluid}

Comencemos primero creando nuestra aplicacion ejecutando el comando `rails new stock_app --database=postgresql`, si te das cuenta utilizaremos el motor de base de datos PostgreSQL, te dejo el [enlace](https://www.postgresql.org/download/) para poder descargarlo e instalarlo.

Una vez finalizada la creación de la carpeta de la aplicacion, iremos al archivo de configuración base de datos para introducir las credenciales postgresql, este archivo esta ubicado `\config\database.yml` en la sección de development y test tenemos que descomentar las líneas de `username` y `password` obviamente debes introducir las credenciales de tu propia instancia (luego veremos como usar variables de entorno para esta configuración), si querés cambiar el nombre de la base de datos aqui es donde podés hacerlo, por ultimo se vera algo como lo siguiente:

![diagram](/assets/images/posts/stock-app/database_yml.jpg){:.img-fluid}

Rails por defecto considera tu ambiente como development pero tambien podés asegurar creando una variable de entorno llamada **RAILS_ENV** con el valor **development**. Luego ejecutaremos el comando `rails db:create` para crear las bases de datos de desarrollo y prueba.

##### Scaffold

Un comentario frecuente que se dice sobre Rails es que fácil prototipar aplicaciones y esto se debe a la generación rápida del pipeline completo para un modelo veámoslo en acción en nuestro primer modelo `Category`, ejecutemos el siguiente comando `rails g scaffold Category name:string`, esto generara una serie de archivos algunos creados solamente para guiarte como ordenar tu codigo pero en este post nos importa solo 3 secciones el modelo (con su migración), el controlador y las vistas. Asi que vayamos parte por parte.

Los modelos se encuentran en la carpeta `\app\models` y en el comando le dijimos a Rails que genere un modelo llamado `category` con un campo `name` de tipo string, Rails enlaza esta clase con su migración correspondiente en la ruta `\db\migrate` con una archivo llamado como `20200319013951_create_categories.rb`, la sección del timestamp puede ser diferente para vos pero el contenido debería ser lo siguiente:

{% highlight ruby linenos %}
class CreateCategories < ActiveRecord::Migration[6.0]
  def change
    create_table :categories do |t|
      t.string :name

      t.timestamps
    end
  end
end

{% endhighlight %}

Algo que puede llamar la atención es la linea `t.timestamps`, esta linea le dice a la base de datos que agregue dos columnas a la tabla de este modelo, la primera `created_at` y la otra es `updated_at` que por si sus nombres no son suficiente estas columnas indican cuando una fila se creo y cuando se actualizo por ultima vez.

El comando que ejecutamos genera las operaciones CRUD básicas y eso lo podemos en el controlador ubicados en la ruta `\app\controllers`. El scaffold tambien nos ayuda a ver el estándar en los nombres de métodos que debe tener un controlador para cumplir con la rutas configuradas en el archivo `config\routes.rb`. Si ves este archivo veras una linea como esta `resources :categories` esto hace que Rails busque las rutas que mencione, podes ver mas sobre esto en la [documentacion](https://guides.rubyonrails.org/routing.html#crud-verbs-and-actions) oficial de Rails.

Ahora veamos el contenido del controlador generado sección por sección.

En la linea despues de declaración de la clase podemos ver que se aplica un filtro para los métodos show, edit, update y destroy. Este filtro es un método que se ejecuta antes de dichas acciones (como lo dice su nombre `before_action`).

Despues vemos nuestro primer método `index` que obtiene todas las categorías creadas con el método `all` de la clase `Category` que debido a que hereda la clase `ActiveRecord` tiene acceso a la base de datos.
Luego esta el método `show` que aunque parece vacío pero como vimos antes este es uno de los métodos que tiene el filtro `set_category` asi que lo veremos en la sección final.
{% highlight ruby linenos %}
class CategoriesController < ApplicationController
  before_action :set_category, only: [:show, :edit, :update, :destroy]

  # GET /categories
  # GET /categories.json
  def index
    @categories = Category.all
  end

  # GET /categories/1
  # GET /categories/1.json
  def show
  end
{% endhighlight %}

En esta sección esta el método `new` que nos devuelve el formulario para crear una nueva **Category**, por eso vemos que inicializa una variable llamada `@category` que será utilizada en la vista.
Luego esta `edit` que de nuevo este método tambien tiene el filtro `set_category`
{% highlight ruby linenos %}
  # GET /categories/new
  def new
    @category = Category.new
  end

  # GET /categories/1/edit
  def edit
  end
{% endhighlight %}

El método `create` es el que realiza la inserción en la base de datos no sin antes realizar validaciones de modelos que se ejecutan dentro del método `save`. El método `category_params` sanitiza los parámetros que lleguen a la aplicacion.
Si el método `save` falla, Rails se encargara de mostrar los errores de por que fallo la creación en base de datos.
{% highlight ruby linenos %}
  # POST /categories
  # POST /categories.json
  def create
    @category = Category.new(category_params)

    respond_to do |format|
      if @category.save
        format.html { redirect_to @category, notice: 'Category was successfully created.' }
        format.json { render :show, status: :created, location: @category }
      else
        format.html { render :new }
        format.json { render json: @category.errors, status: :unprocessable_entity }
      end
    end
  end
{% endhighlight %}

Podemos ver que el método `update` recibe los parámetros para actualizar la `@category` que ya se habia inicializado en el filtro `set_category`, este método tambien realiza validaciones de modelos y si algo falla, se ven los errores al igual que el método de la anterior sección.

{% highlight ruby linenos %}
  # PATCH/PUT /categories/1
  # PATCH/PUT /categories/1.json
  def update
    respond_to do |format|
      if @category.update(category_params)
        format.html { redirect_to @category, notice: 'Category was successfully updated.' }
        format.json { render :show, status: :ok, location: @category }
      else
        format.html { render :edit }
        format.json { render json: @category.errors, status: :unprocessable_entity }
      end
    end
  end
{% endhighlight %}

El método `destroy` elimina fisicamente la `@category` identificada, en una aplicacion real tal vez lo recomendado seria una eliminación lógica solamente.

{% highlight ruby linenos %}
  # DELETE /categories/1
  # DELETE /categories/1.json
  def destroy
    @category.destroy
    respond_to do |format|
      format.html { redirect_to categories_url, notice: 'Category was successfully destroyed.' }
      format.json { head :no_content }
    end
  end
{% endhighlight %}

Por ultimo vemos los métodos privados del controlador, el primer `set_category` busca la **Category** correspondiente al `id` de la ruta y lo almacena en la variable `@category` para que sea manipulada por otros métodos. El otro método privado `category_params` es el que filtra y sanitiza los parámetros que lleguen a la aplicacion, por seguridad, no podemos aceptar cualquier parámetro que sea enviado.
{% highlight ruby linenos %}
  private
    # Use callbacks to share common setup or constraints between actions.
    def set_category
      @category = Category.find(params[:id])
    end

    # Only allow a list of trusted parameters through.
    def category_params
      params.require(:category).permit(:name)
    end
end

{% endhighlight %}

Ahora veamos las vistas, para ser breves solo veremos la vista `index` y la vista `_form` ya que con estas vistas podremos entender las demas. Se encuentran ubicadas en la ruta `\app\views\categories` y veamos la primera vista llamada `index.html.erb`
Podemos ver etiquetas como `<%= %>` que le indican a Rails que ejecute el codigo Ruby que se encuentra encerrado y luego muestre el resultado en el documento HTML, en cambio las etiquetas `<% %>` solo ejecutan el codigo Ruby sin imprimir algo en el documento HTML. Vemos que la vista itera las `@categories` que se cargaron previamente en el controlador durante el metodo `index`. Dentro del ciclo vemos que se generan enlaces de navegación con el helper de Rails `link_to`. Por orden, se generan un enlace para visualizar (Show), editar (Edit) y eliminar (Destroy). Y al final vemos el link para crear una nueva **Category**.
{% highlight erb linenos %}
<p id="notice"><%= notice %></p>

<h1>Categories</h1>

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @categories.each do |category| %>
      <tr>
        <td><%= category.name %></td>
        <td><%= link_to 'Show', category %></td>
        <td><%= link_to 'Edit', edit_category_path(category) %></td>
        <td><%= link_to 'Destroy', category, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Category', new_category_path %>

{% endhighlight %}

Ahora veamos el archivo `_form.html.erb`, vamos directo a este archivo porque esta siendo reutilizado por dos vistas (new y edit), ya que ambas requieren de un formulario del mismo modelo. Vemos que genera un formulario con el helper `form_with` y dentro vemos que si existen errores los muestra en una lista. Mas abajo vemos que se genera el campo para editar el name de la **Category** y por ultimo se genera el botón submit para enviar todo el formulario.
{% highlight erb linenos %}
<%= form_with(model: category, local: true) do |form| %>
  <% if category.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(category.errors.count, "error") %> prohibited this category from being saved:</h2>

      <ul>
        <% category.errors.full_messages.each do |message| %>
          <li><%= message %></li>
        <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= form.label :name %>
    <%= form.text_field :name %>
  </div>

  <div class="actions">
    <%= form.submit %>
  </div>
<% end %>
{% endhighlight %}

Al final del post dejare unos enlaces para todos estos helpers y etiquetas que hemos estado viendo hasta ahora, continuamos ejecutando la aplicacion. Primero debemos ejecutar el siguiente comando para aplicar las migraciones `rails db:migrate`, despues podemos configurar VS Code para ejecutar la aplicacion y en un explorador vamos a la dirección http://localhost:3000/categories y veremos algo como esto
![index categories](/assets/images/posts/stock-app/categories_index.jpg){:.img-fluid}

Hacemos click en el enlace que dice New Category y veremos el formulario para crear una nueva **Category**, llenamos el unico campo de texto que tenemos (correspondiente a `name`) y hacemos click en el botón Create Category para crear nuestra primera categoría y listo. La siguiente pantalla de confirmación nos da la opcion para editar la **Category** recien creada o ir al listado (index), vayamos al listado y podremos ver nuestro dato recien creado, de aqui ya podes experimentar por tu propia cuenta como funcionan las demas acciones.

##### Referencias

Algunos enlaces oficiales que te permitirán entender mejor el funcionamiento de Rails

- [https://guides.rubyonrails.org/command_line.html#rails-generate](https://guides.rubyonrails.org/command_line.html#rails-generate)
- [https://guides.rubyonrails.org/routing.html](https://guides.rubyonrails.org/routing.html)
- [https://guides.rubyonrails.org/form_helpers.html](https://guides.rubyonrails.org/form_helpers.html)
- [https://guides.rubyonrails.org/action_controller_overview.html#parameters](https://guides.rubyonrails.org/action_controller_overview.html#parameters)
- [https://edgeguides.rubyonrails.org/active_record_migrations.html](https://edgeguides.rubyonrails.org/active_record_migrations.html)


### Conclusión

Hemos visto que es muy sencillo comenzar a desarrollar nuevas ideas en Rails, el comando `scaffold` nos permite plasmar rápidamente nuestro diseño, obviamente en casos mas complejos haríamos las cosas mas manualmente pero es bueno tener esta opcion de generación de codigo.

En un siguiente post continuaremos con los otros modelos y como funcionan la relaciones y llaves foráneas en Rails, cuando la serie de posts este completa hare disponible el codigo en GitHub.

Espero que haya sido de ayuda comienza a usar Rails!!
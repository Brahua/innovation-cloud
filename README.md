# Innovation Cloud

Proyecto simple para iniciantes en Ruby on Rails.

Pasos:

1. Crear un nuevo proyecto de rails llamado `innovation-cloud`.
```bash
$ rails new innovation-cloud
```

2. Moverse al nuevo directorio en instalar las gemas que se encuentran en el Gemfile.
```bash
$ bundle install
```

3. Generar un controlador llamado `Pages` con una acción llamada `thanks`, así mismo definir su ruta `/thanks` en `config/routes.rb`.
```bash
$ rails g controller Pages
```

4. Agregue la plantilla para las vista `app/views/pages/thanks.html.erb`

5. Agregue sus estilos en `app/assets/stylesheets/pages.scss`.

6. Generar un controlador llamado `Signups` con las acciones `new` y `create`.
```
$ rails g controller Signups
```

7. Generar un modelo llamado `Signup`. 
```bash
$ rails g model Signup
```

8. Abra el archivo de migración en `db/migrate/` y agregue una columna de cadena llamada `email`. Finalmente, corra el comando de migración
```bash
$ bundle exec rake db:migrate
```

9. En el archivo de rutas, debajo de la ruta `thanks`, agregue:
```
resources :signups
```
Esto se llama una ruta de recursos. Asigna direcciones URL a las siete acciones estándar del controlador de registros.

10. Para ver todas las nuevas URL que creó la ruta de recursos, ingresar lo siguiente en la terminal.
```
$ bundle exec rake routes 
```
¿Qué rutas debemos utilizar? Mirando hacia atrás en el ciclo de solicitud/respuesta, necesitamos una acción de controlador para manejar las solicitudes GET y otra acción de controlador para manejar las solicitudes POST. De acuerdo con la salida de `bundle exec rake routes`, podemos usar:
- `signups#new` para manejar las solicitudes GET mostrando el formulario de registro.
- `signups#create` para gestionar las solicitudes POST guardando un correo electrónico en la base de datos.

11. Configuraremos la acción `signups#new` del controlador para manejar las solicitudes GET.
```ruby
def new
    @signup = Signup.new
end

```

12. Dentro de `app/views/signups/new.html.erb`, cree un formulario para recopilar registros de correo electrónico. Use `form_for` para crear un formulario con los campos del objeto `@signup`.

13. En el archivo de rutas, establezca la acción `signups#new` como la ruta raíz.

14. Configuraremos la acción `signups#create` del controlador para manejar las solicitudes POST. En el controlador `Signups`, escribe un método privado llamado `signup_params`:
```ruby
private
def signup_params
    params.require(:signup).permit(:email)
end
```

15. En el mismo controlador, adapte la acción `create`. Utilice signup_params para recopilar datos del formulario de forma segura y actualizar la base de datos. Después de guardar en la base de datos, redirija a `/thanks`. Si el registro no se guardó correctamente, renderice la plantilla `new`.
```ruby
def create
    @signup = Signup.new(signup_params)
    if @signup.save
        redirect_to '/thanks'
    else
        render 'new'
    end
end
```

16. Inicie su servidor Rails.
```bash
$ rails s -p PORT_NUMBER
```

17. Para visualizar los emails agregados, ejecute lo siguiente en una nueva terminal
```bash
rails console
```

18. Luego ingrese `Signup.all` en la nueva consola y veras los resultados asociados a la tabla.

![Innovation cloud web app](app/assets/images/innovation-cloud.png)
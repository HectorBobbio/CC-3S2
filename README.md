# Manejo de Excepciones
El manejo de expeciones nos permite definir un "camino a seguir" estructurado en caso se encuentren comportamientos no previstos, ayudando a la identificación de estos comportamientos así como previniendo la interrupción del programa mismo. 

El manejo de excepciones responde también a buenas costumbres de programación: No intentar atrapar excepciones muy generales (Preferible atrapar `AuthenticationError` sobre `Error`); si muchas acciones son propensas a arrojar excepciones recordar DRY, no es necesario manejarlas inmediatamente si eso implica repetir el mismo código en varias acciones; no es necesario manejar todas las excepciones, sobretodo si no hay una forma clara y definida de cómo es que hay que manejarla.  

A continuación se muestra un pequeño ejemplo de manejo de excepciones en la aplicación de `myrottenpotatoes` al momento de crear una película.

```ruby
class MoviesController < ApplicationController
  # [...]
  def create
    # Definimos la sentencia en la que buscas excepciones
    begin
      # Crea una pelicula con los parámetros dados.
      # Los metodos que terminan en ! arrojan una excepcion si su resultado es nil
      @movie = Movie.create!(movie_params)

    # Definimos el tipo de excepcion a atrapar
    rescue ActiveRecord::RecordInvalid => e
      # Guardamos el mensaje del error para mostrarlo mas adelante
      flash[:alert] = "La pelicula no pudo ser creada. " + e.message
      render 'new'

    # En caso no encontrar excepcion...
    else
      redirect_to movies_path, :notice => "#{@movie.title} creada!"
    end
  end
  # [...]
end
```

Si detectamos que la película a guardar es inválida (no pasa las validaciones en `models/movie.rb`) guardamos el mensaje asociado a la excepción y se lo mostramos al usuario, y renderizamos la vista `new` para que pueda volver a ingresar otra película.

![](/imgs/ExceptionHandling.png)

Respecto a la diferencia en el manejo de excepciones entre Ruby y Javascript, Ruby tiene la siguiente estructura:

```ruby
begin
    # Codigo que pueda tener excepciones
    tarea1
    tarea2
rescue excepcion_especifica => e
    # En caso de encontrar una excepcion especifico, se ejecuta...
    tarea_por_exc1
    tarea_por_exc2
else
    # En caso no se encuentren excepciones
    tarea3
ensure
    # Encuentre excepciones o no, se ejecutara siempre...
    tarea4
end
```

mientras que Javascript:

```javascript
try{
    // Codigo que pueda tener excepciones
    tarea1
    tarea2
}
catch (e){
    // En caso de encontrar excepciones
    tarea_por_exc1
    tarea_por_exc2
}
finally{
    // Se ejecuta siempre
    tarea3
}

```
Ruby nos permite definir el tipo de excepcion que vamos a atrapar, mientras que Javascript atrapa cualquier tipo de excepcion que encuentre.

# Arquitectura orientada a Microservicios

Al momento de seguir el tutorial indicado, encontre errores al instalas las dependecias requeridas.

![](/imgs/Error-Nodejs.png)

Respecto a las diferencias entre una arquitectura orientada a Microservicios y la arquitectura usual cliente-servidor, podemos ver que con los microservicios encapsulamos cada funcionalidad de la aplicacion en unidades pequeñas e independientes de las demás, que se comunican entre sí mediante un broker. Esto significaría una mejoría en escabilidad y mantenimiento pues no será necesario una revisión exhaustiva de toda la aplicación cuando se quiera agregar una función nueva o modificar una existente gracias a la autonomía entre microservicios.
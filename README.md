# Práctica Calificada 5
## Pregunta 1
#### 1. En las actividades relacionados a la Introducción de Rails los métodos actuales del controlador no son muy robustos: si el usuario introduce de manera manual un URI para ver (Show) una película que no existe (por ejemplo /movies/99999), verás un mensaje de excepción horrible. Modifica el método show del controlador para que, si se pide una película que no existe, el usuario sea redirigido a la vista Index con un mensaje más amigable explicando que no existe ninguna película con ese.
Para mejorar la robustez de los métodos del controlador, se ha añadido manejo de excepciones en el método Show. Ahora, si un usuario intenta acceder a una película que no existe (por ejemplo, /movies/99999), en lugar de mostrar un error interno del servidor, se le redirige a la vista Index con un mensaje amigable.

- En MoviesController:

```ruby
 def show
    begin
      @movie = Movie.find(params[:id])
    rescue ActiveRecord::RecordNotFound
      redirect_to movies_path, alert: "No existe ninguna película con ese ID." and return
    end
  end
```
Resultado:

![image](https://github.com/Daniel349167/PC2-DesarrolloDesSoftware/assets/62466867/b895f7a5-be69-4af2-b5a6-1eacc3ffb28c)

## Pregunta 2



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

#### 4. De la actividad relacionada a BDD e historias de usuario crea definiciones de pasos que te permitan escribir los siguientes pasos en un escenario de RottenPotatoes:
```ruby
Given the movie "Inception" exists
And it has 5 reviews
And its average review score is 3.5
```

- pasos BDD:
  
```ruby
Característica: Calificaciones de reseñas de películas
  Para mantener la integridad y la utilidad de las calificaciones de las películas
  Como un usuario del sitio web
  Quiero asegurarme de que las calificaciones promedio de las películas sean correctas

Escenario: Verificar el promedio de calificación de una película
  Dado que la película "Inception" existe
  Y tiene 5 reseñas
  Cuando calculo su calificación promedio
  Entonces el promedio de calificación debería ser 3.5
```

## Pregunta 2



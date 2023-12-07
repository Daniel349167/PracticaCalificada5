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

- Pasos:
  
```ruby
# features/step_definitions/movie_steps.rb

Given(/^the movie "(.*?)" with (\d+) reviews and average score of (\d+\.\d+) exists$/) do |title, num_reviews, avg_score|
  Movie.create!(title: title, rating: 'PG', description: 'A great movie', release_date: '2010-01-01')
end
```
#### 5. De la actividad relacionadas a BDD e historias de usuario, supongamos que en RottenPotatoes, en lugar de utilizar seleccionar la calificación y la fecha de estreno, se opta por rellenar el formulario en blanco. Primero, realiza los cambios apropiados al escenario. Enumera las definiciones de pasos a partir que Cucumber invocaría al pasar las pruebas de estos nuevos pasos. (Recuerda: rails generate cucumber:install)
- inicializando Cucumber:
![image](https://github.com/Daniel349167/PracticaCalificada5/assets/62466867/38f2e0ec-b9b4-461a-b54c-540ed3cc92c4)
- completando el escenario:
```ruby
# features/agregar_pelicula.feature

Característica: Agregar nueva película
  Como usuario
  Quiero agregar detalles de una nueva película
  Para poder llevar un registro de todas las películas en mi colección

Escenario: Agregar una nueva película con título, calificación y fecha de lanzamiento
  Dado que he decidido agregar una nueva película
  Cuando estoy en la página de Crear Nueva Película
  Y lleno el campo "Título" con "The Shawshank Redemption"
  Y lleno el campo "Calificación" con "PG-13"
  Y lleno el campo "Fecha de Lanzamiento" con "1994-09-23"
  Y presiono "Guardar Cambios"
  Entonces debería ver "The Shawshank Redemption ha sido agregada"
 ```

- definiciones de pasos:

#### 6. De la actividad relacionadas a BDD e historias de usuario indica una lista de pasos para implementar el siguiente paso:
```ruby
When / I delete the movie: "(.*)"/ do |title|
 ```

- completando la lista de pasos:
```ruby
# Eliminar una película
When /^I delete the movie "(.*)"/ do |title|
  movie = Movie.find_by_title(title)
  movie.destroy
end
 ```


## Pregunta 2



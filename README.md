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

Given(/^the movie "(.*?)" with (\d+) reviews and average review of (\d+\.\d+) exists$/) do |title, num_reviews, avg_review|
  Movie.create!(title: title, rating: 'PG', description: 'A great movie', release_date: '2010-01-01')
end
```
#### 5. De la actividad relacionadas a BDD e historias de usuario, supongamos que en RottenPotatoes, en lugar de utilizar seleccionar la calificación y la fecha de estreno, se opta por rellenar el formulario en blanco. Primero, realiza los cambios apropiados al escenario. Enumera las definiciones de pasos a partir que Cucumber invocaría al pasar las pruebas de estos nuevos pasos. (Recuerda: rails generate cucumber:install)
- inicializando Cucumber:
![image](https://github.com/Daniel349167/PracticaCalificada5/assets/62466867/38f2e0ec-b9b4-461a-b54c-540ed3cc92c4)
- completando el escenario:
```ruby
# language: es
Característica: Añadir nueva película
  Como un usuario de RottenPotatoes
  Quiero añadir una nueva película
  Para poder expandir la base de datos de películas del sitio

Escenario: Añadir una película con información completa
  Dado que estoy en la página de añadir una nueva película
  Cuando relleno el formulario con la siguiente información:
    | title        | rating | release_date |
    | El Padrino   | R      | 2020-03-24   |
  Y presiono el botón "Save Changes"
  Entonces debería estar en la página principal
  Y debería ver "El Padrino"
 ```

- definiciones de pasos:
```ruby

Dado("que estoy en la página de añadir una nueva película") do
    visit new_movie_path
  end
  
Cuando("relleno el formulario con la siguiente información:") do |table|
movie_data = table.hashes.first
fill_in 'movie[title]', with: movie_data['title']
select movie_data['rating'], from: 'movie[rating]' unless movie_data['rating'].nil?

unless movie_data['release_date'].nil?
    date = Date.parse(movie_data['release_date'])
    select date.year.to_s, from: 'movie_release_date_1i'
    select I18n.t('date.month_names')[date.month], from: 'movie_release_date_2i'
    select date.day.to_s, from: 'movie_release_date_3i'
end
end

Y("presiono el botón {string}") do |button_name|
click_button button_name
end

Entonces("debería estar en la página principal") do
    expect(page).to have_current_path(movies_path)
end
  
Entonces("debería ver {string}") do |content|
expect(page).to have_content(content)
end
```

- paso las pruebas:
![image](https://github.com/Daniel349167/PracticaCalificada5/assets/62466867/8b21e2e8-1288-47bf-a0b7-c2c0100925ed)


#### 6. De la actividad relacionadas a BDD e historias de usuario indica una lista de pasos para implementar el siguiente paso:
```ruby
When / I delete the movie: "(.*)"/ do |title|
 ```
- delete_movie.feature
```ruby
# language: es
Característica: Eliminar una película existente
  Como usuario de RottenPotatoes
  Quiero eliminar una película
  Para mantener actualizada la base de datos de películas del sitio

Escenario: Eliminar una película de la lista
  Dado que he añadido "El Padrino" con rating "R"
  Y estoy en la página principal
  Cuando selecciono la película "El Padrino"
  Y presiono "Delete"
  Entonces soy redirigido a la página principal
  Y no debería ver "El Padrino"
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



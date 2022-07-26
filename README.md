# Dojo Ninja

1.	Empieza un nuevo proyecto (el nombre del proyecto debe ser 'dojo_ninjas').
rails new dojo_ninjas

2.	Crea las tablas/modelos apropiados.
rails g model Dojo name:string city:string state:string
rails g model Ninja first_name:string last_name:string dojo:references

3.	Usando la consola de Ruby:
3.1.	Cree 3 dojos (ingrese algunos datos en blanco solo para asegurarse que le permite ingresar datos en blanco).
Dojo.create(name:"CodingDojo Silicon Valley", city:"Mountain View", state:"CA")
Dojo.create(name:"CodingDojo S", city:"Seattle", state:"WA")
Dojo.create(name:"CodingDojo New York", city:"New York", state:"NY")
Dojo.create(name:"CodingDojo Prueba", city:"", state:"CL")

4.	Cambia tu modelo para que haga las siguientes validaciones:
4.1.	Para dojo, requerir nombre, ciudad, estado. También haga que el estados sea de 2 caracteres de longitud.
class Dojo < ApplicationRecord
  validates :name, :city, :state, presence: true
  validates :state, length: { maximum: 2}
end

4.2.	Para ninja, requerir nombre y apellido.
class Ninja < ApplicationRecord
  validates :first_name, :last_name, presence: true
end

5.	Asegúrate que el modelo ninja pertenece a dojo (especifique esto en ambos modelos tanto dojo como en ninja).
class Dojo < ApplicationRecord
  has_many :ninjas
end

class Ninja < ApplicationRecord
  belongs_to :dojo
end

6.	Usando la consola de Ruby
6.1.	Elimine los 3 dojos que creó (ej. Dojo.find(1).destroy, también consulte el método destroy_all).
Dojo.destroy_all

6.2.	Cree 3 dojos adicionales usando el comando Dojo.new.
dojo = Dojo.new(name: "Dojo 1", city: "Santiago", state: "RM")
dojo.save
dojo = Dojo.new(name: "Dojo 2", city: "Puerto Montt", state: "PM")
dojo.save
dojo = Dojo.new(name: "Dojo 3", city: "Valparaiso", state: "VP")
dojo.save

6.3.	Intente crear algunos dojos adicionales pero sin especificar algunos de los campos requeridos. Asegúrese que las validaciones están funcionando y que le devuelve los mensajes de error.
dojo = Dojo.create(name:"Dojo 4", city:"Antofagasta", state:"")
{:state=>["can't be blank"]}

dojo = Dojo.create(name:"", city:"Antofagasta", state:"AT")
{:name=>["can't be blank"]}

6.4.	Cree 3 ninjas que pertenezcan al primer dojo que creó.
Ninja.create(first_name:"Diego", last_name:"Hernandez", dojo_id:6)
Ninja.create(first_name:"Andres", last_name:"Ramos", dojo_id:6)
Ninja.create(first_name:"Pedro", last_name:"Martinez", dojo_id:6)

6.5.	Cree 3 ninjas más que pertenezcan al segundo dojo que creó.
Ninja.create(first_name:"Osvaldo", last_name:"Hernandez", dojo_id:7)
Ninja.create(first_name:"Eric", last_name:"Ramos", dojo_id:7)
Ninja.create(first_name:"Gonzalo", last_name:"Martinez", dojo_id:7)

6.6.	Cree 3 ninjas más que pertenezcan al tercer dojo que creó.
Ninja.create(first_name:"Daniel", last_name:"Hernandez", dojo_id:8)
Ninja.create(first_name:"Carlos", last_name:"Ramos", dojo_id:8)
Ninja.create(first_name:"Jose", last_name:"Martinez", dojo_id:8)

6.7.	Asegúrate de entender cómo obtener todos los ninjas para cualquier dojo (ej. obtener todos los ninjas para el primer dojo, segundo dojo, etc).
Ninja.where("Dojo_id = 7")
Ninja.where("Dojo_id = 8")
Ninja.where("Dojo_id = 9")

6.8.	¿Cómo recuperar solo el nombre de los ninjas que pertenecen al segundo dojo y ordenar el resultado por created_at en orden descendiente?
Ninja.where("Dojo_id = 7").order("created_at DESC").select("first_name")

6.9.	Elimine el segundo dojo. ¿Cómo podrías ajustar tu modelo para que automáticamente elimine todos los ninjas asociados con ese dojo?
class Dojo < ApplicationRecord
    has_many :ninjas, dependent: :destroy
end

Dojo.destroy(7)

*	Asegúrate de sentirte complétamente cómodo utilizando .all, .valid?, .where, .order, .limit, .save, .create, .errors y los otros métodos que vimos en el video.
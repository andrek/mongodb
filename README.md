# mongodb
Praticas em MongoDB para a disciplina de Banco de Dados Não Relacionais (NOSQL) 

# Exercício 1 - Aquecendo com os pets

1. Adicione outro Peixe e um Hamster com nome Frodo

> db.pets.insert({name: "Frodo", species: "Peixe"})
WriteResult({ "nInserted" : 1 })
> db.pets.insert({name: "Frodo", species: "Hamster"})
WriteResult({ "nInserted" : 1 })

2. Faça uma contagem dos pets na coleção

> db.pets.count()
8


3. Retorne apenas um elemento o método prático possível

> db.pets.findOne()
{
        "_id" : ObjectId("5e69840d545d40410bce06de"),
        "name" : "Mike",
        "species" : "Hamster"
}


4. Identifique o ID para o Gato Kilha.

> db.pets.find({name: "Kilha", species: "Gato"}, {"_id":1})
{ "_id" : ObjectId("5e69841c545d40410bce06e0") }


5. Faça uma busca pelo ID e traga o Hamster Mike

> db.pets.find({_id: db.pets.findOne({name: "Mike", species: "Hamster"}, {"_id":1})._id})
{ "_id" : ObjectId("5e69840d545d40410bce06de"), "name" : "Mike", "species" : "Hamster" }


6. Use o find para trazer todos os Hamsters

> db.pets.find({species: "Hamster"})
{ "_id" : ObjectId("5e69840d545d40410bce06de"), "name" : "Mike", "species" : "Hamster" }
{ "_id" : ObjectId("5e698467545d40410bce06e5"), "name" : "Frodo", "species" : "Hamster" }


7. Use o find para listar todos os pets com nome Mike

> db.pets.find({name: "Mike"})
{ "_id" : ObjectId("5e69840d545d40410bce06de"), "name" : "Mike", "species" : "Hamster" }
{ "_id" : ObjectId("5e698421545d40410bce06e1"), "name" : "Mike", "species" : "Cachorro" }


8. Liste apenas o documento que é um Cachorro chamado Mike

> db.pets.find({name: "Mike", species: "Cachorro"})
{ "_id" : ObjectId("5e698421545d40410bce06e1"), "name" : "Mike", "species" : "Cachorro" }
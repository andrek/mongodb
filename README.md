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

```
{
        "_id" : ObjectId("5e69840d545d40410bce06de"),
        "name" : "Mike",
        "species" : "Hamster"
}
```

4. Identifique o ID para o Gato Kilha.

> db.pets.find({name: "Kilha", species: "Gato"}, {"_id":1})

{ "_id" : ObjectId("5e69841c545d40410bce06e0") }


5. Faça uma busca pelo ID e traga o Hamster Mike

> db.pets.find({_id: db.pets.findOne({name: "Mike", species: "Hamster"}, {"_id":1})._id})

{ "_id" : ObjectId("5e69840d545d40410bce06de"), "name" : "Mike", "species" : "Hamster" }


6. Use o find para trazer todos os Hamsters

> db.pets.find({species: "Hamster"})

```
{ "_id" : ObjectId("5e69840d545d40410bce06de"), "name" : "Mike", "species" : "Hamster" }
{ "_id" : ObjectId("5e698467545d40410bce06e5"), "name" : "Frodo", "species" : "Hamster" }
```

7. Use o find para listar todos os pets com nome Mike

> db.pets.find({name: "Mike"})

```
{ "_id" : ObjectId("5e69840d545d40410bce06de"), "name" : "Mike", "species" : "Hamster" }
{ "_id" : ObjectId("5e698421545d40410bce06e1"), "name" : "Mike", "species" : "Cachorro" }
```

8. Liste apenas o documento que é um Cachorro chamado Mike

> db.pets.find({name: "Mike", species: "Cachorro"})

{ "_id" : ObjectId("5e698421545d40410bce06e1"), "name" : "Mike", "species" : "Cachorro" }

# Exercício 2 – Mama mia!

1. Liste/Conte todas as pessoas que tem exatamente 99 anos. Você pode usar um count para indicar a quantidade.

> db.italians.find({age: 99}).count()

0


2. Identifique quantas pessoas são elegíveis atendimento prioritário (pessoas com mais de 65 anos)

> db.italians.find({age: {"$gte":65}}).count()

1882


3. Identifique todos os jovens (pessoas entre 12 a 18 anos).

> db.italians.find({age: {"$gte":12, "$lte":18}}).count()

925

4. Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas não tem nenhum dos dois

> db.italians.find({cat: {$exists:true}}).count()<br>

6042

> db.italians.find({dog: {$exists:true}}).count()

3965

> db.italians.find({dog: {$exists:false}, cat: {$exists:false}}).count()

2388

5. Liste/Conte todas as pessoas acima de 60 anos que tenham gato

> db.italians.find({"$and":[{age:{$gt:60}},{cat:{$exists:true}}]}).count()

1447

6. Liste/Conte todos os jovens com cachorro

> db.italians.find({$and:[{age:{$gte:12,$lte:18}},{dog:{$exists:true}}]}).count()

336

7. Utilizando o $where, liste todas as pessoas que tem gato e cachorro

> db.italians.find({ $where: "this.cat != null && this.dog != null" }).count()

2396

8. Liste todas as pessoas mais novas que seus respectivos gatos.

> db.italians.find({$where:"(this.cat!=null)&&(this.age<this.cat.age)"}).count()

628

9. Liste as pessoas que tem o mesmo nome que seu bichano (gato ou cachorro)


> db.italians.find({$where:"((this.cat!=null)&&(this.firstname==this.cat.name))||((this.dog!=null)&&(this.firstname==this.dog.name))"})

10. Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo

> db.italians.find({"bloodType":/-/}).count()

5032

> db.italians.find({"bloodType":/-/},{firstname:1,surname:1,_id: 0})

```
{ "firstname" : "Michele", "surname" : "Mancini" }
{ "firstname" : "Teresa", "surname" : "Conti" }
{ "firstname" : "Valeira", "surname" : "Bernardi" }
{ "firstname" : "Gianluca", "surname" : "Basile" }
{ "firstname" : "Elisabetta", "surname" : "Russo" }
{ "firstname" : "Matteo", "surname" : "Rossetti" }
{ "firstname" : "Mattia", "surname" : "Conte" }
{ "firstname" : "Emanuele", "surname" : "Santoro" }
{ "firstname" : "Antonella", "surname" : "Ferrara" }
{ "firstname" : "Andrea", "surname" : "D’Amico" }
{ "firstname" : "Angelo", "surname" : "Fontana" }
{ "firstname" : "Anna", "surname" : "De Santis" }
{ "firstname" : "Giorgia", "surname" : "Marini" }
{ "firstname" : "Chiara", "surname" : "Riva" }
{ "firstname" : "Luigi", "surname" : "De Santis" }
{ "firstname" : "Tiziana", "surname" : "Sala" }
{ "firstname" : "Serena", "surname" : "Costa" }
{ "firstname" : "Mattia", "surname" : "Giordano" }
{ "firstname" : "Mirko", "surname" : "Santoro" }
{ "firstname" : "Giovanna", "surname" : "Caruso" }
Type "it" for more
```

11. Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId)

> db.italians.find({"$or":[{cat:{$exists:true}},{dog:{$exists:true}}]},{cat:1,dog:1,_id:0})

```
{ "cat" : { "name" : "Rosa", "age" : 15 } }
{ "cat" : { "name" : "Domenico", "age" : 9 } }
{ "cat" : { "name" : "Giorgia", "age" : 10 } }
{ "cat" : { "name" : "Monica", "age" : 15 } }
{ "cat" : { "name" : "Valentina", "age" : 1 } }
{ "cat" : { "name" : "Matteo", "age" : 9 } }
{ "cat" : { "name" : "Eleonora", "age" : 15 } }
{ "cat" : { "name" : "Serena", "age" : 1 } }
{ "cat" : { "name" : "Valeira", "age" : 0 } }
{ "cat" : { "name" : "Mattia", "age" : 9 }, "dog" : { "name" : "Pasquale", "age" : 15 } }
{ "cat" : { "name" : "Alberto", "age" : 8 } }
{ "cat" : { "name" : "Federico", "age" : 10 } }
{ "cat" : { "name" : "Emanuele", "age" : 12 } }
{ "cat" : { "name" : "Luigi", "age" : 3 } }
{ "cat" : { "name" : "Domenico", "age" : 3 }, "dog" : { "name" : "Enzo ", "age" : 9 } }
{ "dog" : { "name" : "Roberta", "age" : 10 } }
{ "cat" : { "name" : "Giovanna", "age" : 14 }, "dog" : { "name" : "Maurizio", "age" : 5 } }
{ "dog" : { "name" : "Rosa", "age" : 14 } }
{ "cat" : { "name" : "Fabrizio", "age" : 17 } }
{ "dog" : { "name" : "Nicola", "age" : 15 } }
Type "it" for more
```

12. Quais são as 5 pessoas mais velhas com sobrenome Rossi?

> db.italians.find({surname: "Rossi"},{firstname:1, age:1, _id:0}).sort({age: -1}).limit(5)

```
{ "firstname" : "Angelo", "age" : 79 }
{ "firstname" : "Gianluca", "age" : 78 }
{ "firstname" : "Giorgio", "age" : 78 }
{ "firstname" : "Pasquale", "age" : 77 }
{ "firstname" : "Salvatore", "age" : 75 }
```

13. Crie um italiano que tenha um leão como animal de estimação. Associe um nome e idade ao bichano

> db.italians.insert({"firstname":"Andre","lion":{"name":"Lion","age":15}})

WriteResult({ "nInserted" : 1 })

14. Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id.

> db.italians.find({"lion.name": "Lion"})

{ "_id" : ObjectId("5e8689e0396743a3ea20c69c"), "firstname" : "Andre", "lion" : { "name" : "Lion", "age" : 15 } }

> db.italians.remove({"_id": ObjectId("5e8689e0396743a3ea20c69c")})

WriteResult({ "nRemoved" : 1 })

15. Passou um ano. Atualize a idade de todos os italianos e dos bichanos em 1.

> db.italians.update({},{"$inc":{"age":1}}, {multi:true})

WriteResult({ "nMatched" : 10000, "nUpserted" : 0, "nModified" : 10000 })

> db.italians.update({"cat": {$exists: true}}, {$inc: {"cat.age": 1}}, {multi: true})

WriteResult({ "nMatched" : 6042, "nUpserted" : 0, "nModified" : 6042 })

> db.italians.update({"dog": {$exists: true}}, {$inc: {"dog.age": 1}}, {multi: true})

WriteResult({ "nMatched" : 3965, "nUpserted" : 0, "nModified" : 3965 })

16. O Corona Vírus chegou na Itália e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos.

> db.italians.remove({"$and":[{cat:{$exists:true}},{age:66}]})
WriteResult({ "nRemoved" : 112 })

17. Utilizando o framework agregate, liste apenas as pessoas com nomes iguais a sua respectiva mãe e que tenha gato ou cachorro.
> db.italians.aggregate([
  {'$match': { mother: { $exists: 1}}},
  {'$match': { $or: [{ cat: { $exists: true }}, { dog: { $exists: true }}]}},
  {'$project': { "firstname": 1, "mother": 1, "cat": 1, "dog": 1, "isEqual": { "$cmp": ["$firstname","$mother.firstname"]} }},
  {'$match': {"isEqual": 0}}
]);

```
{ "_id" : ObjectId("5e868d3d56be2d840cf08bef"), "firstname" : "Gianluca", "mother" : { "firstname" : "Gianluca", "surname" : "Barbieri", "age" : 57 }, "cat" : { "name" : "Giusy", "age" : 12 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d3f56be2d840cf08f84"), "firstname" : "Emanuela", "mother" : { "firstname" : "Emanuela", "surname" : "Benedetti", "age" : 44 }, "cat" : { "name" : "Daniele", "age" : 3 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4256be2d840cf09609"), "firstname" : "Antonella", "mother" : { "firstname" : "Antonella", "surname" : "Barone", "age" : 54 }, "dog" : { "name" : "Roberto", "age" : 17 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4256be2d840cf09722"), "firstname" : "Daniele", "mother" : { "firstname" : "Daniele", "surname" : "Battaglia", "age" : 39 }, "cat" : { "name" : "Daniela", "age" : 17 }, "dog" : { "name" : "Elena", "age" : 17 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4356be2d840cf0973b"), "firstname" : "Angela", "mother" : { "firstname" : "Angela", "surname" : "Mazza", "age" : 55 }, "dog" : { "name" : "Antonella", "age" : 5 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4556be2d840cf09ca8"), "firstname" : "Giulia", "mother" : { "firstname" : "Giulia", "surname" : "Montanari", "age" : 30 }, "cat" : { "name" : "Tiziana", "age" : 4 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4856be2d840cf0a337"), "firstname" : "Federico", "mother" : { "firstname" : "Federico", "surname" : "Bianchi", "age" : 59 }, "cat" : { "name" : "Luigi", "age" : 6 }, "dog" : { "name" : "Davide", "age" : 11 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4856be2d840cf0a40d"), "firstname" : "Mauro", "mother" : { "firstname" : "Mauro", "surname" : "Bellini", "age" : 57 }, "dog" : { "name" : "Pasquale", "age" : 15 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4856be2d840cf0a4bc"), "firstname" : "Manuela", "mother" : { "firstname" : "Manuela", "surname" : "Gallo", "age" : 65 }, "cat" : { "name" : "Luca", "age" : 12 }, "dog" : { "name" : "Elisa", "age" : 11 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4a56be2d840cf0a8b6"), "firstname" : "Filipo", "mother" : { "firstname" : "Filipo", "surname" : "Damico", "age" : 72 }, "dog" : { "name" : "Elisa", "age" : 1 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4a56be2d840cf0a993"), "firstname" : "Federica", "mother" : { "firstname" : "Federica", "surname" : "Negri", "age" : 68 }, "cat" : { "name" : "Emanuele", "age" : 2 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4b56be2d840cf0a9e2"), "firstname" : "Giusy", "mother" : { "firstname" : "Giusy", "surname" : "Bruno", "age" : 46 }, "cat" : { "name" : "Enzo ", "age" : 7 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4b56be2d840cf0aa0c"), "firstname" : "Roberta", "mother" : { "firstname" : "Roberta", "surname" : "Galli", "age" : 104 }, "dog" : { "name" : "Rita", "age" : 6 }, "isEqual" : 0 }
{ "_id" : ObjectId("5e868d4c56be2d840cf0ad74"), "firstname" : "Domenico", "mother" : { "firstname" : "Domenico", "surname" : "Pellegrino", "age" : 67 }, "dog" : { "name" : "Monica", "age" : 2 }, "isEqual" : 0 }
```

18. Utilizando aggrgate framework, faça uma lista de nomes única de nomes. Faça isso usando apenas o primeiro nome

> db.italians.aggregate([{$group: {_id: "$firstname"}},{'$project': {"firstname": 1}}])

```
{ "_id" : "Giovanna" }
{ "_id" : "Andrea" }
{ "_id" : "Angelo" }
{ "_id" : "Vincenzo" }
{ "_id" : "Lucia" }
{ "_id" : "Teresa" }
{ "_id" : "Sara" }
{ "_id" : "Mauro" }
{ "_id" : "Raffaele" }
{ "_id" : "Eleonora" }
{ "_id" : "Alessia" }
{ "_id" : "Daniele" }
{ "_id" : "Simona" }
{ "_id" : "Alessandra" }
{ "_id" : "Anna" }
{ "_id" : "Claudia" }
{ "_id" : "Ilaria" }
{ "_id" : "Laura" }
{ "_id" : "Angela" }
{ "_id" : "Alex" }
Type "it" for more
```

19. Agora faça a mesma lista do item acima, considerando nome completo.

> db.italians.aggregate([{$group: {_id: { firstname: "$firstname", surname: "$surname" }}}]);

```
{ "_id" : { "firstname" : "Valentina", "surname" : "Giordano" } }
{ "_id" : { "firstname" : "Valeira", "surname" : "Bruno" } }
{ "_id" : { "firstname" : "Anna", "surname" : "Martinelli" } }
{ "_id" : { "firstname" : "Filipo", "surname" : "Monti" } }
{ "_id" : { "firstname" : "Federico", "surname" : "Monti" } }
{ "_id" : { "firstname" : "Elisa", "surname" : "Milani" } }
{ "_id" : { "firstname" : "Alessandra", "surname" : "Martini" } }
{ "_id" : { "firstname" : "Paolo", "surname" : "Greco" } }
{ "_id" : { "firstname" : "Giacomo", "surname" : "Barbieri" } }
{ "_id" : { "firstname" : "Tiziana", "surname" : "Gallo" } }
{ "_id" : { "firstname" : "Roberta", "surname" : "Leone" } }
{ "_id" : { "firstname" : "Emanuele", "surname" : "Sartori" } }
{ "_id" : { "firstname" : "Matteo", "surname" : "Guerra" } }
{ "_id" : { "firstname" : "Giorgia", "surname" : "Ferraro" } }
{ "_id" : { "firstname" : "Martina", "surname" : "Barbieri" } }
{ "_id" : { "firstname" : "Giacomo", "surname" : "Ferri" } }
{ "_id" : { "firstname" : "Angela", "surname" : "Guerra" } }
{ "_id" : { "firstname" : "Rita", "surname" : "Serra" } }
{ "_id" : { "firstname" : "Tiziana", "surname" : "Messina" } }
{ "_id" : { "firstname" : "Antonella", "surname" : "Parisi" } }
Type "it" for more
```

20. Procure pessoas que gosta de Banana ou Maçã, tenham cachorro ou gato, mais de 20 e menos de 60 anos.

> db.italians.find({"$and": [
  {"$or": [{ cat: { $exists: true }}, { dog: { $exists: true }}]},
  {"age": {"$gt": 20, "$lt": 60 }},
  {"$and": [{"favFruits": {"$exists": true}}, {"favFruits": ["Banana", "Maçã"]}]}
]}).count();

11


# Exercício 3 – Stockbrokers

1. Liste as ações com profit acima de 0.5 (limite a 10 o resultado

> db.stocks.find({"Profit Margin":{$gt: 0.5}}).limit(10)

2. Liste as ações com perdas (limite a 10 novamente)

> db.stocks.find({"Profit Margin":{$lt: 0}}).limit(10)

3. Liste as 10 ações mais rentáveis

> db.stocks.find({}).sort({"Profit Margin" : -1}).limit(10)

4. Qual foi o setor mais rentável?

> db.stocks.aggregate([{"$group":{_id:"$Sector", total:{$sum:"$Profit Margin" }}},{$sort:{total: -1}}])

```
{ "_id" : "Financial", "total" : 162.5356 }
{ "_id" : "Services", "total" : 20.5515 }
{ "_id" : "Consumer Goods", "total" : 13.23 }
{ "_id" : "Industrial Goods", "total" : 11.0327 }
{ "_id" : "Utilities", "total" : 7.423 }
{ "_id" : "Conglomerates", "total" : 0.3835 }
{ "_id" : "Basic Materials", "total" : -9.190900000000001 }
{ "_id" : "Technology", "total" : -18.8968 }
{ "_id" : "Healthcare", "total" : -316.68649999999997 }
```

5. Ordene as ações pelo profit e usando um cursor, liste as ações.

> var cursor = db.stocks.find({}).sort({"Profit Margin" : -1}).limit(10)

> cursor.forEach(function(x) {print(x.Ticker, x["Profit Margin"])})

```
BPT 0.994
CACB 0.994
ROYT 0.99
NDRO 0.986
WHZ 0.982
MVO 0.976
AGNC 0.972
VOC 0.971
MTR 0.97
OLP 0.97
```

6. Renomeie o campo “Profit Margin” para apenas “profit”.

> db.stocks.update({"Profit Margin": { $exists: true }}, { $rename: {"Profit Margin": "Profit"}}, {multi: true})

WriteResult({ "nMatched" : 4302, "nUpserted" : 0, "nModified" : 4302 })


7. Agora liste apenas a empresa e seu respectivo resultado

> db.stocks.find({},{"Company": 1,"Profit": 1, _id: 0})

```
{ "Company" : "iShares MSCI AC Asia Information Tech" }
{ "Company" : "Applied Optoelectronics, Inc.", "Profit" : -0.023 }
{ "Company" : "Atlantic American Corp.", "Profit" : 0.056 }
{ "Company" : "AAON Inc.", "Profit" : 0.105 }
{ "Company" : "Advantage Oil & Gas Ltd.", "Profit" : -0.232 }
{ "Company" : "WCM/BNY Mellon Focused Growth ADR ETF" }
{ "Company" : "Apple Inc.", "Profit" : 0.217 }
{ "Company" : "AllianceBernstein Holding L.P.", "Profit" : 0.896 }
{ "Company" : "Atlas Air Worldwide Holdings Inc.", "Profit" : 0.071 }
{ "Company" : "Alcoa, Inc.", "Profit" : 0.013 }
{ "Company" : "Abaxis Inc.", "Profit" : 0.1 }
{ "Company" : "ABB Ltd.", "Profit" : 0.069 }
{ "Company" : "AbbVie Inc.", "Profit" : 0.24 }
{ "Company" : "Agilent Technologies Inc.", "Profit" : 0.137 }
{ "Company" : "Cambium Learning Group, Inc.", "Profit" : -0.645 }
{ "Company" : "Ameris Bancorp", "Profit" : 0.166 }
{ "Company" : "AmerisourceBergen Corporation", "Profit" : 0.005 }
{ "Company" : "Advisory Board Co.", "Profit" : 0.055 }
{ "Company" : "ARCA biopharma, Inc." }
{ "Company" : "Aaron's, Inc.", "Profit" : 0.06 }
Type "it" for more
```

8. Analise as ações. É uma bola de cristal na sua mão... Quais as três ações
você investiria?

> db.stocks.find({}, {"Ticker":1, "Profit":1, "Company":1, "Sector":1, "_id":0 }).sort({"Profit": -1}).limit(3)

```
{ "Ticker" : "BPT", "Sector" : "Basic Materials", "Company" : "BP Prudhoe Bay Royalty Trust", "Profit" : 0.994 }
{ "Ticker" : "CACB", "Sector" : "Financial", "Company" : "Cascade Bancorp", "Profit" : 0.994 }
{ "Ticker" : "ROYT", "Sector" : "Basic Materials", "Company" : "Pacific Coast Oil Trust", "Profit" : 0.99 }
```
9. Liste as ações agrupadas por setor

> db.stocks.aggregate([{$group: { _id :"$Sector", empresas: { $push: "$Company" }}}])

# Atividade Oscars

#### 1. Quantas vezes Natalie Portman foi indicada ao Oscar?</br>
<strong>Resposta:</strong> Natalie Portman foi indicada 3 vezes

```js
> db.registros.countDocuments({nome_do_indicado: 'Natalie Portman'})
< 3
```
<hr>

#### 2. Quantos Oscars Natalie Portman ganhou?</br>
<strong>Resposta:</strong> Natalie Portman venceu 1 vez

```js
> db.registros.countDocuments({nome_do_indicado: 'Natalie Portman', vencedor: 'true'})
< 1
```
<hr>

#### 3. Amy Adams já ganhou algum Oscar?</br>
<strong>Resposta:</strong> Amy Adams não venceu nenhuma vez
```js
> db.registros.countDocuments({nome_do_indicado: 'Amy Adams', vencedor: 'true'})
< 0
```

<hr>

#### 4. A série de filmes Toy Story ganhou um Oscar em quais anos?</br>
<strong>Resposta:</strong> Toy Story ganhou o oscar nos anos: 2011 e 2020

```js
> db.registros.distinct("ano_cerimonia", {nome_do_filme: /Toy Story/, vencedor: 'true'})
< [ 2011, 2020 ]
```

<hr>

#### 5. A partir de que ano que a categoria "Actress" deixa de existir?</br>
<strong>Resposta:</strong> 1976 foi o ultimo ano em que a categoria actress existiu
```js
> db.registros.find({categoria: 'ACTRESS'}, {ano_cerimonia: 1, categoria: 1, _id: 0}).sort({ano_cerimonia: -1}).limit(1)
< {
  ano_cerimonia: 1976,
  categoria: 'ACTRESS'
  }
```

<hr>

#### 6. O primeiro Oscar para melhor Atriz foi para quem? Em que ano?</br>
<strong>Resposta:</strong> Foi em 1928 para a atriz Janet Gaynor

```js
> db.registros.find({categoria: 'ACTRESS', vencedor:'true'}, {ano_cerimonia: 1, categoria: 1, nome_do_indicado: 1}).limit(1)
< {
  _id: ObjectId('66ead39b088afa60aee4b4c8'),
  ano_cerimonia: 1928,
  categoria: 'ACTRESS',
  nome_do_indicado: 'Janet Gaynor'
  }
```
<hr>

#### 7. Na campo "Vencedor", altere todos os valores com "Sim" para 1 e todos os valores "Não" para 0.</br>
<strong>Resposta:</strong> Alterado 👍

```js
> db.registros.updateMany({vencedor: "true"}, {$set: {vencedor: 1}});
< {
  acknowledged: true,
  insertedId: null,
  matchedCount: 2464,
  modifiedCount: 2464,
  upsertedCount: 0
  }
> db.registros.updateMany({vencedor: "false"}, {$set: {vencedor: 0}});
< {
  acknowledged: true,
  insertedId: null,
  matchedCount: 8423,
  modifiedCount: 8423,
  upsertedCount: 0
  }
```

<hr>

#### 8. Em qual edição do Oscar "Crash" concorreu ao Oscar?</br>
<strong>Resposta:</strong> Na edição número 78

```js
> db.registros.distinct('cerimonia', {nome_do_filme: 'Crash'})
< [ 78 ]
```

<hr>

#### 9. Bom... dê um Oscar para um filme que merece muito, mas não ganhou.</br>
<strong>Resposta:</strong> merecido 😁

```js
> db.registros.updateOne({vencedor: 1, ano_cerimonia: 2021}, {$set: {vencedor: 0}})
< {
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
  }
> db.registros.updateOne({nome_do_indicado: 'Bert Hamelinck and Sacha Ben Harroche, Producers', ano_cerimonia: 2021}, {$set: {vencedor: 1}})
< {
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
  }
```

<hr>

#### 10. O filme Central do Brasil aparece no Oscar?</br>
<strong>Resposta:</strong> Sim, aparece

```js
> db.registros.countDocuments({nome_do_filme: 'Central Station'})
< 2
```

<hr>

#### 11. Inclua no banco 3 filmes que nunca foram nem nomeados ao Oscar, mas que merecem ser. </br>
<strong>Resposta:</strong> Adicionado 👨🏻‍💻

```js
>db.registros.insertMany([{
  id_registro: 45964,
  ano_filmagem: 2014,
  ano_cerimonia: 2015,
  cerimonia: 87,
  categoria: 'BEST PICTURE',
  nome_do_indicado: 'Black Bear Pictures',
  nome_do_filme: 'Imitation Game',
  vencedor: 0
    },{
  id_registro: 45964,
  ano_filmagem: 2014,
  ano_cerimonia: 2015,
  cerimonia: 87,
  categoria: 'DIRECTING',
  nome_do_indicado: 'Morten Tyldum',
  nome_do_filme: 'Imitation Game',
    vencedor: 0
    },{
  id_registro: 45964,
  ano_filmagem: 2014,
  ano_cerimonia: 2015,
  cerimonia: 87,
  categoria: 'CINEMATOGRAPHY',
  nome_do_indicado: 'Graham Moore',
  nome_do_filme: 'Imitation Game',
  vencedor: 0
    }])
< {
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('66eaeb4b81c952e0095bc597'),
    '1': ObjectId('66eaeb4b81c952e0095bc598'),
    '2': ObjectId('66eaeb4b81c952e0095bc599')
  }
  }
```

<hr>

#### 12. Pensando no ano em que você nasceu: Qual foi o Oscar de melhor filme, Melhor Atriz e Melhor Diretor?</br>
<strong>Resposta:</strong> Melhor filme: A Beautiful Mind. Melhor atriz: Halle Berry. Melhor ator: Denzel Washinton


```js
> db.registros.find({ano_cerimonia: 2002, vencedor: 1, categoria: { "$in": [/LEADING ROLE/, /PICTURE/]}}, {categoria: 1, nome_do_indicado: 1, nome_do_filme: 1, _id: 0})
< {
  categoria: 'ACTOR IN A LEADING ROLE',
  nome_do_indicado: 'Denzel Washington',
  nome_do_filme: 'Training Day'
}
{
  categoria: 'ACTRESS IN A LEADING ROLE',
  nome_do_indicado: 'Halle Berry',
  nome_do_filme: "Monster's Ball"
}
{
  categoria: 'BEST PICTURE',
  nome_do_indicado: 'Brian Grazer and Ron Howard, Producers',
  nome_do_filme: 'A Beautiful Mind'
}
  }
```

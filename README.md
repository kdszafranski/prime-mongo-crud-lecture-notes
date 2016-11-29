# Mongo CRUD/Intro Lecture Notes

Open Mongo shell: `mongo`

## Database

Create a new datbase called 'sigma':

`use sigma`

Create a new Collection called 'books':

`db.createCollection('books')`

Create a session variable as a short-hand:


`var books = db.books`

## Create (Insert)
```
books.insert({
  title: "Snow Crash",
  author: "Neal Stephenson",
  edition: 1.2
  });
```

See what's in the Collection.

`books.find()`

`books.findOne()` ???


books.insert({
  title: "A Tale of Two Cities",
  author: "Charles Dickens",
  edition: 2,
  published: new Date('05-20-1859')
});

books.insert({
  title: "Murder on the Orient Express",
  author: "Agatha Christie",
  edition: 2
});

books.insert({
  title: "Catcher in the Rye",
  author: "Someone",
  edition: 1,
  ratings: [
    {score: 2},
    {score: 5},
    {score: 3.5},
    {score: 4}
  ]}
);


## Read (Find)
All books in our Collection: `books.find()`

Specific: `books.find({title: "Snow Crash"});`

Like Search: `books.find({title: /Cat/});`


### Comparison Operators (greater-, less-than)
By Date. Before today: `books.find({published: {$lt: new Date('2016-11-28')}})`

Find based on given edition numbers range
`books.find({'edition':{$lt: 3}, 'edition':{$gt: 1.2} })`

Implicit AND is used when multiple fields are specified.

$lte, $gte (greater than or equals)

`$exists` checks if this Document has a property/field.


## Update
books.update()

```
books.update({title:"Catcher in the Rye"}, {$set: author: "J. D. Salinger"});
```

### Arrays

Push into an existing array on a Document:

```
books.update({title: "Catcher in the Rye"}, {$push: {ratings: {score: 4.5}}})
books.update({title: "Catcher in the Rye"}, {$push: {ratings: {score: 9}}})
```

Update in an array (? no idea)

Find It:
`books.find({title: "Catcher in the Rye"})`

Update It:
books.update({title: "Catcher in the Rye"}, {$set: {ratings.1.score: 999}})


db.orders.update({_id: <some object id>}, {$set: {lineItems.1.unitPrice: 42.99}})


## Delete (Remove)
books.remove({"_id" : ObjectId("583dc0b4482c7736c1d27058")})
583dc0b4482c7736c1d27058

// doesn't work

books.remove({title: "A Tale of Two Cities", published: null})


### Assignment Answers
```
db.orders.insert([
  {orderDate: new Date('2000-01-02-),
  orderTotal: 12.99,
  lineItems: [
  {unitPrice: 2.00, quantity: 5, productName: 'giraffe'}
  ]},
 {// second object},
 {// third object}
})

//3. Find a single order document, any order document.
db.orders.findOne()

//4. Find all orders and make them look pretty.
db.orders.find().pretty()

//5. Find all orders with an orderDate that is prior to 1/1/2016.
db.orders.find({orderDate: {$lt: new Date('2016-01-01')})

//6. Find all orders with an orderDate that is after 1/1/2016.
db.orders.find({orderDate: {$gt: new Date('2016-01-01')})

//7. Find orders with lineItems that have a quantity that is less than 50, but greater than 5.
db.orders.find({$and:['lineItems.quantity':{$lt: 50}, 'lineItems.quantity':{$gt: 5}]}})

//8. Update one of your line items to 42.99.
// Note that the 1 here is the index in the array. Arrays are zero based.
db.orders.update({_id: <some object id>}, {$set: {lineItems.1.unitPrice: 42.99}})

//9. Remove one of your orders.
db.orders.remove({_id: <some object id>})
```

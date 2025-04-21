# Embedding vs Referencing

## Embedding

- single query to retrieve data
- single operation to update/delete date
- data duplication
- large documents

## Referencing 

- no duplication
- smaller documents
- need to join data from multiple documents

__________________________________________________________________________
# Python Connection

```python
# firstly run 
# pip install 'pymongo[srv]'

from pymongo import MongoClient
MONGODB_URI = f'mongodb+srv://PowerCell46:PowerCell46@cluster0.xhq46jq.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0'

client = MongoClient(MONGODB_URI)

for db_name in client.list_database_names():
    print(db)
```

## [[Mongo DB Python]]

__________________________________________________________________________
# Insertion

## Inserting a single document

```
db.grades.insertOne({
  student_id: 654321,
  products: [
    {
      type: "exam",
      score: 90,
    },
  ],
  class_id: 550,
})
```

## Insert multiple documents

```
db.grades.insertMany([
  {
    student_id: 546789,
    products: [
      {
        type: "quiz",
        score: 50,
      },
    ],
    class_id: 551,
  },
  {
    student_id: 777777,
    products: [
      {
        type: "exam",
        score: 83,
      },
    ],
    class_id: 550,
  },
  {
    student_id: 223344,
    products: [
      {
        type: "exam",
        score: 45,
      },
    ],
    class_id: 551,
  },
])
```

__________________________________________________________________________
# Finding

## Searching Documents

```
db.zips.find({ _id: ObjectId("5c8eccc1caa187d17ca6ed16") })
```

## Comparison operators

```
db.sales.find({ "items.price": { $gt: 50}})
db.sales.find({ "customer.age": { $gte: 65}})
```

```
db.sales.find({ "items.price": { $lt: 50}})
db.sales.find({ "customer.age": { $lte: 65}})
```

## Search in subarrays
#### Use the `$elemMatch` operator to find all documents that contain the specified **subdocument**. 
- find documents where at least one element in an array meets multiple conditions.
- The `$elemMatch` operator ensures that all specified criteria are applied to the same array element.

```
db.sales.find({
	items: {
		$elemMatch: {name: "laptop", price: {$gt: 800}, quantity: {$gte: 1} }
	},
})
```

## And & Or Operator

```
db.routes.find({
  $or: [{ dst_airport: "SEA" }, { src_airport: "SEA" }],
})
```

```
db.routes.find({
  $and: [
    { $or: [{ dst_airport: "SEA" }, { src_airport: "SEA" }] },
    { $or: [{ "airline.name": "American Airlines" }, { airplane: 320 }] },
  ]
})
```
## Sorting

```
# Ascending
db.companies.find({ category_code: "music" }).sort({ name: 1 });

# Descending
db.companies.find({ category_code: "music" }).sort({ name: -1, _id: -1 });
```

__________________________________________________________________________
## Limiting

```
db.companies
  .find({ category_code: "music" })
  .sort({ number_of_employees: -1, _id: 1 })
  .limit(3);
```

## Selecting shown fields

```
# 1 shown, 0 not shown
db.inspections.find(
  { sector: "Restaurant - 818" },
  { business_name: 1, result: 1, _id: 0 }
)
```

__________________________________________________________________________
# Altering

## Replace Document

```
db.books.replaceOne(
  {
    _id: ObjectId("6282afeb441a74a98dbbec4e"),
  },
  {
    title: "Data Science Fundamentals for Python and MongoDB",
    isbn: "1484235967",
    publishedDate: new Date("2018-5-10"),
    thumbnailUrl:
      "https://m.media-amazon.com/images/I/71opmUBc2wL._AC_UY218_.jpg",
    authors: ["David Paper"],
    categories: ["Data Science"],
  }
)
```

## Update Single Document

```
db.podcasts.updateOne(
  {
    _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8"),
  },

  {
    $set: {
      subscribers: 98562,
    },
  }
)
```

## Upsert 

- adds the document if it does not exist

```
db.podcasts.updateOne(
  { title: "The Developer Hub" },
  { $set: { topics: ["databases", "MongoDB"] } },
  { upsert: true }
)
```

## Update Many Documents

```
db.books.updateMany(
  { publishedDate: { $lt: new Date("2019-01-01") } },
  { $set: { status: "LEGACY" } }
)
```

### Push - adding an element to an array

```
db.podcasts.updateOne(
  { _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8") },
  { $push: { hosts: "Nic Raboy" } }
)
```

__________________________________________________________________________

## Deleting a Document

```
db.podcasts.deleteOne({ _id: Objectid("6282c9862acb966e76bbf20a") })
```

## Delete multiple Documents

```
db.podcasts.deleteMany({category: “crime”})
```

__________________________________________________________________________
# Aggregations

## Counting entries

```
db.trips.countDocuments({})
```

## Match
- The `$match` stage filters for documents that match specified conditions.

```
{
  $match: {
     "field_name": "value"
  }
}
```

## Group
- The `$group` stage groups documents by a group key.

```
{
  $group:
    {
      _id: <expression>, // Group key
      <field>: { <accumulator> : <expression> }
    }
 }
```

```
db.zips.aggregate([
{   
   $match: { 
      state: "CA"
    }
},
{
   $group: {
      _id: "$city",
      totalZips: { $count : { } }
   }
}
])
```

## Sorting the Results

```
db.zips.aggregate([
{
  $sort: {
    pop: -1
  }
},
{
  $limit:  5
}
])
```

## Selecting Shown Columns

```
{
    $project: {
        state:1, 
        zip:1,
        population:"$pop",
        _id:0
    }
}
```


## Creating new sugar column

```
{
    $set: {
        place: {
            $concat:["$city",",","$state"]
        },
        pop:10000
     }
  }
```

## Counting the entries

```
{
  $count: "total_zips"
}
```


# Indexing

```
db.customers.createIndex({
  birthdate: 1
})
```

## Adding unique constraint

```
db.customers.createIndex({
  email: 1
},
{
  unique:true
})
```

## View indexes

```
db.customers.getIndexes()
```

## Compound Index

```
db.customers.createIndex({
  active:1, 
  birthdate:-1,
  name:1
})
```

## Drop Index

```
db.customers.dropIndex(
  'active_1_birthdate_-1_name_1'
)
```

## Drop all indexes

```
db.customers.dropIndexes()
```


# db.inventory.find().toArray()

# Insertion
## Inserting a single document

```
db = client.bank

accounts_collection = db.accounts

new_account = {
    "account_holder": "Linus Torvalds",
    "account_id": "MDB829001337",
    "account_type": "checking",
    "balance": 50352434,
    "last_updated": datetime.datetime.utcnow(),
}

result = accounts_collection.insert_one(new_account)

print(f"_id of inserted document: {result.inserted_id}")

client.close()
```

## Inserting multiple documents

```
db = client.bank

accounts_collection = db.accounts

new_accounts = [
    {
        "account_id": "MDB011235813",
        "account_holder": "Ada Lovelace",
        "account_type": "checking",
        "balance": 60218,
    },
    {
        "account_id": "MDB829000001",
        "account_holder": "Muhammad ibn Musa al-Khwarizmi",
        "account_type": "savings",
        "balance": 267914296,
    },
]

result = accounts_collection.insert_many(new_accounts)

print("# of documents inserted: " + str(len(result.inserted_ids)))
print(f"_ids of inserted documents: {result.inserted_ids}")

client.close()
```

# Finding 

## Finding a single document

```
db = client.bank

accounts_collection = db.accounts

document_to_find = {"_id": ObjectId("62d6e04ecab6d8e1304974ae")}

result = accounts_collection.find_one(document_to_find)
pprint.pprint(result)

client.close()
```

## Finding multiple documents

```
db = client.bank

accounts_collection = db.accounts

documents_to_find = {"balance": {"$gt": 4700}}

cursor = accounts_collection.find(documents_to_find)

num_docs = 0
for document in cursor:
    num_docs += 1
    pprint.pprint(document)
    print()
print("# of documents found: " + str(num_docs))

client.close()
```

# Updating

## Updating a single document

```
db = client.bank

accounts_collection = db.accounts

document_to_update = {"_id": ObjectId("62d6e04ecab6d8e130497482")}

add_to_balance = {"$inc": {"balance": 100}}

pprint.pprint(accounts_collection.find_one(document_to_update))

result = accounts_collection.update_one(document_to_update, add_to_balance)
print("Documents updated: " + str(result.modified_count))

pprint.pprint(accounts_collection.find_one(document_to_update))

client.close()
```

## Updating multiple documents

```
db = client.bank

accounts_collection = db.accounts

# Filter
select_accounts = {"account_type": "savings"}

set_field = {"$set": {"minimum_balance": 100}}

result = accounts_collection.update_many(select_accounts, set_field)

print("Documents matched: " + str(result.matched_count))
print("Documents updated: " + str(result.modified_count))
pprint.pprint(accounts_collection.find_one(select_accounts))

client.close()
```

# Deleting 

```
db = client.bank

accounts_collection = db.accounts

documents_to_delete = {"balance": {"$lt": 2000}}

print("Searching for sample target document before delete: ")
pprint.pprint(accounts_collection.find_one(documents_to_delete))

result = accounts_collection.delete_many(documents_to_delete)

print("Searching for sample target document after delete: ")
pprint.pprint(accounts_collection.find_one(documents_to_delete))

print("Documents deleted: " + str(result.deleted_count))

client.close()
```

# ~~Transactions

```
#Connect to MongoDB cluster with MongoClient
client = MongoClient(MONGODB_URI)

# Step 1: Define the callback that specifies the sequence of operations to perform inside the transactions.
def callback(
    session,
    transfer_id=None,
    account_id_receiver=None,
    account_id_sender=None,
    transfer_amount=None,
):

    # Get reference to 'accounts' collection
    accounts_collection = session.client.bank.accounts

    # Get reference to 'transfers' collection
    transfers_collection = session.client.bank.transfers

    transfer = {
        "transfer_id": transfer_id,
        "to_account": account_id_receiver,
        "from_account": account_id_sender,
        "amount": {"$numberDecimal": transfer_amount},
    }

    # Transaction operations
    # Important: You must pass the session to each operation

    # Update sender account: subtract transfer amount from balance and add transfer ID
    accounts_collection.update_one(
        {"account_id": account_id_sender},
        {
            "$inc": {"balance": -transfer_amount},
            "$push": {"transfers_complete": transfer_id},
        },
        session=session,
    )

    # Update receiver account: add transfer amount to balance and add transfer ID
    accounts_collection.update_one(
        {"account_id": account_id_receiver},
        {
            "$inc": {"balance": transfer_amount},
            "$push": {"transfers_complete": transfer_id},
        },
        session=session,
    )

    # Add new transfer to 'transfers' collection
    transfers_collection.insert_one(transfer, session=session)

    print("Transaction successful")

    return


def callback_wrapper(s):
    callback(
        s,
        transfer_id="TR218721873",
        account_id_receiver="MDB343652528",
        account_id_sender="MDB574189300",
        transfer_amount=100,
    )


# Step 2: Start a client session
with client.start_session() as session:
    # Step 3: Use with_transaction to start a transaction, execute the callback, and commit (or cancel on error)
    session.with_transaction(callback_wrapper)


client.close()
```


# Aggregations

```
select_by_balance = {"$match": {"balance": {"$lt": 1000}}}
```


```
# Separate documents by account type and calculate the average balance for each account type.
separate_by_account_calculate_avg_balance = {
    "$group": {"_id": "$account_type", "avg_balance": {"$avg": "$balance"}}
}
```


```
select_by_balance = {"$match": {"balance": {"$lt": 1000}}}

separate_by_account_calculate_avg_balance = {
    "$group": {"_id": "$account_type", "avg_balance": {"$avg": "$balance"}}
}

pipeline = [
    select_by_balance,
    separate_by_account_calculate_avg_balance,
]

results = accounts_collection.aggregate(pipeline)

print()
print(
    "Average balance of checking and savings accounts with balances of less than $1000:", "\n"
)

for item in results:
    pprint.pprint(item)

client.close()
```

## Sorting

```
# Organize documents in order from highest balance to lowest.
organize_by_original_balance = {"$sort": {"balance": -1}}
```

## Selecting specific rows

```
return_specified_fields = {
    "$project": {
        "account_type": 1,
        "balance": 1,
        "gbp_balance": {"$divide": ["$balance", conversion_rate_usd_to_gbp]},
        "_id": 0,
    }
}
```


```
conversion_rate_usd_to_gbp = 1.3

select_accounts = {"$match": {"account_type": "checking", "balance": {"$gt": 1500}}}

organize_by_original_balance = {"$sort": {"balance": -1}}

return_specified_fields = {
    "$project": {
        "account_type": 1,
        "balance": 1,
        "gbp_balance": {"$divide": ["$balance", conversion_rate_usd_to_gbp]},
        "_id": 0,
    }
}

pipeline = [
    select_accounts,
    organize_by_original_balance,
    return_specified_fields,
]

results = accounts_collection.aggregate(pipeline)

print(
    "Account type, original balance and balance in GDP of checking accounts with original balance greater than $1,500,"
    "in order from highest original balance to lowest: ", "\n"
)

for item in results:
    pprint.pprint(item)

client.close()
```

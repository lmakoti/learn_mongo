<center><img src="logo.png" style="height: 80px;align:center;"></center>

# 1. Get started

**Documentation Platform**: https://www.mongodb.com/docs/

- **Create an account**: https://www.mongodb.com/docs/atlas/tutorial/create-atlas-account/

- **Sign up to mongoDB university**: https://learn.mongodb.com/

- **Select the learning path:** https://learn.mongodb.com/pages/certification-program

  This selection can be based on industry or tech; for my context I made the decision based on tech, and I chose the Python learning path, it aligns with my long-term goal of learning how to utilise Python across a wide range of applications https://learn.mongodb.com/learn/learning-path/mongodb-python-developer-path 

  - Node.js
  - **Python**
  - Java
  - php

- **Student discount:** https://education.github.com/discount_requests/application; you should be able to provide proof of enrolment in a university acceptance letter or valid student card.

# 2. MongoDB Atlas

**Overview URL:** https://cloud.mongodb.com



## 3. Jupyter Notebooks and MongoDB

### Prerequisites

MongoDB is installed and running on your machine or accessible remotely, Jupyter Notebook is installed, and Python MongoDB driver `pymongo` is installed. You can install it using pip:

- Step 1: Import `pymongo` and Establish a Connection
- Step 2: Accessing the Database and Collection
- Step 3: Querying Data
- Step 4: Working with Data in Jupyter Notebook

#### 3.1 Basic Implementation

```python
# Import necessary libraries
import pymongo
import pandas as pd
import matplotlib.pyplot as plt

# Connect to MongoDB
# Replace '<<db_url>>' with your MongoDB instance URL.
client = pymongo.MongoClient("<<db_url>>")
# If MongoDB is running locally, you can use:
# client = pymongo.MongoClient("mongodb://localhost:27017/")

# Access the database and collection
# Replace 'your_db_name' and 'your_collection_name' with your actual database and collection names.
db = client['your_db_name']
collection = db['your_collection_name']

# Querying the database
# Example: Find one document
first_document = collection.find_one()
print("First document:", first_document)

# Example: Find all documents and print them
print("\nAll documents:")
for doc in collection.find():
    print(doc)

# Converting data to a Pandas DataFrame for analysis
data_frame = pd.DataFrame(list(collection.find()))

# Display the first few rows of the DataFrame
print("\nDataFrame head:")
print(data_frame.head())

# Data Visualization
# Example: Simple plot (modify this according to your data and visualization needs)
data_frame.plot(kind='bar')
plt.show()
```



#### 3.2 Security-Thinking Implementation (Using Azure Key Vault)

```python
# Import necessary libraries
# %pip install pymongo[srv]
# %pip install azure-keyvault-secrets azure-identity

import pandas as pd
import pymongo
from pymongo import MongoClient
import os

# azure key vault 
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

# use env var or databricks secrets
key_vault_url = os.environ.get('azure_key_vault_url')
print(key_vault_url)

# Use DefaultAzureCredential to authenticate - this method can use various authentication methods
credential = DefaultAzureCredential()

# Create a SecretClient using the Key Vault URL and the credential
client = SecretClient(vault_url=key_vault_url, credential=credential)

# Retrieve a secret using its name
secret_name = "mongoCluster"
retrieved_secret = client.get_secret(secret_name)

# get the secret
connString = retrieved_secret.value
print(connString)

# database and collection selection
dbName="sample_training"
collectionName="companies" 

# Connect to MongoDB
client = pymongo.MongoClient(connString)

# Access the database and collection
db = client[dbName]
collection = db[collectionName]

results = collection.find()

# Convert to DataFrame
df = pd.DataFrame(list(results))

```



## 4. Improving Code Structure

#### 4.1. Identify Reusable Components

First, look for parts of your code that perform specific tasks and are likely to be reused. Examples could be data processing, API calls, file operations, etc. In your original code, connecting to Azure Key Vault and MongoDB were such components.

#### 4.2. Define Functions

Once you've identified a reusable component, encapsulate its logic in a function. This makes your code modular and easier to maintain. Each function should do one thing and do it well (this is known as the `Single Responsibility Principle`).

For example, if you frequently read data from different sources (CSV, SQL, etc.), you can create separate functions like `read_csv_file(filepath)` or `read_from_sql(database_url, query)`.

#### 4.3. Add Exception Handling

Inside each function, add try-except blocks to handle potential errors specific to the task the function performs. This way, if an error occurs, it can be gracefully handled, and the rest of your code can continue to run. It's also a good practice to log or print the error information for debugging purposes.

For instance, in a `read_csv_file(filepath)` function, you might want to catch and handle `FileNotFoundError` or `pd.errors.ParserError`.

#### 4.4. Test the Functions

After defining your functions, it’s crucial to test them with different inputs and scenarios to ensure they work as expected and handle errors properly.

**Translation of 3.2 using the above principles**

```python

```














































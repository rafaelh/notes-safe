# AZURE STORAGE
  =============

A couple of quick benefits of cloud storage are encryption, georeplication, support for analytics on data consumption, and storage tiers to prioritize access based on how frequently you use data.

## Types of Data
There are thre primary types of data that Azure holds:

* Structured Data - data that adheres to a schema, meaning it all has the same fields or properties. Structured data has keys to show relationships, and is also known as relational data.

* Semi-structured Data - doesn't fit neatly into tables, rows and columns. Instead it uses **tags** or **keys** to organise and provide a heirarchy for the data. Also known as non-relational or NoSQL data.

* Unstructured data - doesn't have an organising structure. For example, a blob might contain a PDF, JPG and a JSON file.

# Storage Services

## SQL Database
This is the latest stable version of MS SQL Server, and always remains up to date, removing the need for periodic migrations. Data can be migrated from on-prem to Azure SQL using the **Azure Database Migration Service**, which in turn uses the Azure Data Migration Assistant tool. This generates recommendations prior to migration, then migrates the data once they are addressed. After that, you just change the connection string for your apps.

## Cosmos DB
Cosmos is a globally distributed service that supports schema-less data. It is designed for highly responsive **Always On** applications with constantly changing data. 

## Blob Storage
Blob storage is **unstructured**, and can hold any type of data. This doesn't need to be in a common file format - they can hold gigabytes of streamed data from a scientific instrument. They can store up to 8 TB of data for VMs.

## Data Lake Storage
This is a large repository which stores structured and unstructured data. It is designed for reliability and performance as a Big Data file system. It is used as follows:

* Ingest - in native format (eg .csv)
* Prepare - clean, enrish, annotage and schematize
* Store - keep availabile for analysis
* Analyse - feed data into engines like hadoop & spark. Works with batch queries, interactive queries, real-time analytics, machine learning and data warehousing.

## Azure Files
Creates an SMB network file share in the cloud that can be attached to your machines.

## Azure Queue
This is a queuing system for large numbers of messagesthat can decouple applications. It can also be used to distribute load amongst servers.

## Disk Storage
This allows you to attach SSDs or HDDs to VMs, and isn't designed for external access. 


# Storage Tiers
There are three storage tiers for blob object storage:

* **Hot Storage** - optimized for frequent access
* **Cool Storage** - optimized for infrequent access, and stored for at least 30 days
* **Archive Storage** - for rarely accessed data, stored for at least 180 days with flexible latency requirements


# Encryption and Replication
The following encrpytion types are available for cloud resources:

* **Azure Storage Service Encryption (SSE)** - protects data at rest. It encrypts the data before storing it, and decrypts before retrieving it. It is transparent to the user.

* **Client-side encryption** - is where the data is already encrypted by the client libraries. Azure stores the data in the encrypted stage at rest, which is decrypted during retrieval.

A replication type is set up when you create a storage account. 



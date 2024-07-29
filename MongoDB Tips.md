## Change database name in MongoDB Atlas

When you are going to use MongoDB Atlas generated connection string by default the databse name will be `test`.

To change your databse compare your connection string with the following string: 
`mongodb+srv://<username>:<password>@cluster0.pfose.mongodb.net/<dbname>?retryWrites=true&w=majority`
Just change the `<dbname>` to your desired one and now by this name the database will be created

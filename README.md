# A simple GraphQL server that uses the file system to store nodes

[Work in progress...] (obviously)

## How it works:

```
\\ schema definition

interface Node {
  id: String!
}

type Person implements Node {
  id: Int!
  name: String,
  age: Int
}

type Group implements Node {
  id: Int!
  name: String
  members: [Person]
}
```

Based on the schema, the server will create a directory based on the type name.
The directory will contain a file called ".meta". In that directory, it will store
information like max_id and indexes.

The following queries will be created for each type:
```
// find any node by id.
person(id: Int)
group(id: Int)
```
One could also imagine creating additional queries, such as:

```
// find all people with matching name and/or given age
person(name: String, age: Int): [Person]`
```

In addition, the following mutations will be created for each type:
```
create_<type>
update_<type>
delete_<type>
```
Items are stored one per file in JSON format. The filename is simply the id. Type Group
would be stored as follows:
```
{
  id: 1,
  name: "Group 1",
  members: [1,5,7,9]
}
```

Of course this server is incredibly basic and doesn't actually let you do all that much.
Building this on top of a database would allow you to do much more, but I think it's
an interesting experiment to see how far you can get relying only on GraphQL and the 
filesystem.


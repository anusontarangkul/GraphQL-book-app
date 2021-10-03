# book-app

This project is refactoring a Google Book search app from REST API to GraphQL. A user can create an account and login. Users can search for books and saved the ones they like. A user has a saved book list and can also delete saved books.

![screenshot](screenshot.png)

|                                         |                                         |                                                   |
| :-------------------------------------: | :-------------------------------------: | :-----------------------------------------------: |
|        [Introduction](#book-app)        | [Table of Contents](#table-of-contents) | [Development Highlights](#development-highlights) |
|      [Installation](#installation)      |    [Page Directory](#page-directory)    |       [Code Hightlights](#code-highlights)        |
| [Technologies Used](#Technologies-Used) |           [Credits](#Credits)           |                [License](#License)                |

## Development Highlights

- Convert REST API to GraphQL
- Use JSON Web Tokens for authenitication
- Create mutations and queries

## Installation

1. Install node modules

```
npm i
```

2. Start App

```
npm start
```

## Page Directory

### Client

The client grapqhl is in the utils directory. There is a seperate file for mutations and queries.

### Server

The server graphql is in the schema. There is a resolvers and typeDefs file.

## Code Highlights

Cretaing and login user resolver. A token is created and applied to the local storage after the mutation.

```JavaScript
    Mutation: {
        addUser: async (parent, args) => {
            const user = await User.create(args);
            const token = signToken(user);
            return { token, user }
        },
        login: async (parent, { email, password }) => {
            const user = await User.findOne({ email });

            if (!user) {
                throw new AuthenticationError('Incorrect credentials')
            }
            const correctPw = await user.isCorrectPassword(password);

            if (!correctPw) {
                throw new AuthenticationError('Incorrect credentials')
            }
            const token = signToken(user);
            return { token, user }
        },
```

Saving a book. The app checks for authentication and adds the input from the searched book.

```JavaScript
  const handleSaveBook = async (bookId) => {
    // find the book in `searchedBooks` state by the matching id
    const bookToSave = searchedBooks.find((book) => book.bookId === bookId);
    console.log(bookToSave)
    // get token
    const token = Auth.loggedIn() ? Auth.getToken() : null;

    if (!token) {
      return false;
    }

    try {
      const { data } = await saveBook({
        variables: { input: { ...bookToSave } }
      })
      setSavedBookIds([...saveBookIds, bookToSave.bookId])
    } catch (err) {
      console.error(err)
    }

  };
```

## Technologies

### Frontend

- [HTML](https://www.w3schools.com/html/)
- [JavaScript](https://www.javascript.com/)
- [CSS](https://www.w3schools.com/css/)

### Frontend Framework/Library

- [ReactJS](https://reactjs.org/)
- [ApolloClient](https://www.apollographql.com/docs/react/)

### Backend Language

- [Node.js](https://nodejs.org/en/)

### GraphQL

- [graphql](https://graphql.org/)
- [ApolloServer](https://www.apollographql.com/docs/apollo-server/)

### Database

- [MongoDB](https://www.mongodb.com/)

## Credits

|                           |                                                                                                                                                                                                       |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **David Anusontarangkul** | [![Linkedin](https://i.stack.imgur.com/gVE0j.png) LinkedIn](https://www.linkedin.com/in/anusontarangkul/) [![GitHub](https://i.stack.imgur.com/tskMh.png) GitHub](https://github.com/anusontarangkul) |

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

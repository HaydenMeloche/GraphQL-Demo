# GraphQL-Demo
Really simple client/server app that utilizes GraphQL

## How does it work?
Rather than having endpoints for different data. You have one endpoint and pass it queries.

Define you schema
```js
// GraphQL schema
var schema = buildSchema(`
    type Query {
        course(id: Int!): Course
        courses(topic: String): [Course]
    },
    type Course {
        id: Int
        title: String
        author: String
        description: String
        topic: String
        url: String
    }
`);
```
Create an array of some fake data
```js
var coursesData = require('./coursesData.json');
```

Then write some filtering functions
```js
var getCourse = function(args) { 
    var id = args.id;
    return coursesData.filter(course => {
        return course.id == id;
    })[0];
}
var getCourses = function(args) {
    if (args.topic) {
        var topic = args.topic;
        return coursesData.filter(course => course.topic === topic);
    } else {
        return coursesData;
    }
}
```

Write one endpoint that utilizes GraphQL and your filtering functions & voila
```js
var root = {
    course: getCourse,
    courses: getCourses
};
// Create an express server and a GraphQL endpoint
var app = express();
app.use('/graphql', express_graphql({
    schema: schema,
    rootValue: root,
    graphiql: true //this enables the web UI for graphql
}));
```

You can now query your data
![Picture of query](https://i.imgur.com/Mrvh0u6.png)

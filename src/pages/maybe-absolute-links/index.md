---
title: Maybe Absolute Links?
date: '2019-07-27'
spoiler: A weird error message
---

An error message suddenly appeared from Gatsby while building this site

```bash
info changed file at /Users/oisu/Workspace/blog.oisulab.com/src/pages/started-my-blog/index.md
[ { GraphQLError: Cannot query field "maybeAbsoluteLinks" on type "fields_4".
    at Object.Field (/Users/oisu/Workspace/blog.oisulab.com/node_modules/graphql/validation/rules/FieldsOnCorrectType.js:65:31)
    at Object.enter (/Users/oisu/Workspace/blog.oisulab.com/node_modules/graphql/language/visitor.js:324:29)
    at Object.enter (/Users/oisu/Workspace/blog.oisulab.com/node_modules/graphql/language/visitor.js:366:25)
    at visit (/Users/oisu/Workspace/blog.oisulab.com/node_modules/graphql/language/visitor.js:254:26)
    at visitUsingRules (/Users/oisu/Workspace/blog.oisulab.com/node_modules/graphql/validation/validate.js:74:22)
    at validate (/Users/oisu/Workspace/blog.oisulab.com/node_modules/graphql/validation/validate.js:59:10)
    at graphqlImpl (/Users/oisu/Workspace/blog.oisulab.com/node_modules/graphql/graphql.js:106:50)
    at /Users/oisu/Workspace/blog.oisulab.com/node_modules/graphql/graphql.js:66:223
    at Promise._execute (/Users/oisu/Workspace/blog.oisulab.com/node_modules/bluebird/js/release/debuggability.js:313:9)
    at Promise._resolveFromExecutor (/Users/oisu/Workspace/blog.oisulab.com/node_modules/bluebird/js/release/promise.js:483:18)
    at new Promise (/Users/oisu/Workspace/blog.oisulab.com/node_modules/bluebird/js/release/promise.js:79:10)
    at graphql (/Users/oisu/Workspace/blog.oisulab.com/node_modules/graphql/graphql.js:63:10)
    at graphqlRunner (/Users/oisu/Workspace/blog.oisulab.com/node_modules/gatsby/dist/bootstrap/index.js:372:14)
    at Promise (/Users/oisu/Workspace/blog.oisulab.com/gatsby-node.js:25:7)
    at Promise._execute (/Users/oisu/Workspace/blog.oisulab.com/node_modules/bluebird/js/release/debuggability.js:313:9)
    at Promise._resolveFromExecutor (/Users/oisu/Workspace/blog.oisulab.com/node_modules/bluebird/js/release/promise.js:483:18)
    message: 'Cannot query field "maybeAbsoluteLinks" on type "fields_4".',
    locations: [ [Object] ],
    path: undefined } ]
```

I don't dig into this yet, but realized that it's not shown if an internal link to the other post in markdown like [this](/started-my-blog/) exists anywhere in my whole posts:

```markdown
[this](/started-my-blog/)
```

So, this is the post only for solving the problem! Anyway, the error disappeared because I wrote the line above 😂

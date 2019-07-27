---
title: Maybe Absolute Links?
date: '2019-07-27'
spoiler: 変なエラーメッセージ
---

Gatsbyでサイトをビルド中に、なんかこんなエラーメッセージがでました。

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

詳しくは調べてないけど、[こういう](/started-my-blog/)感じの他の記事への内部リンクがサイトのどこかに1つあったら、出なくなることが判明。

```markdown
[this](/started-my-blog/)
```

なので、これはそのためだけの記事です！今上に書いたので、エラー出なくなりました😂

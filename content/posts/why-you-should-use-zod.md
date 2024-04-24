---
title: "Why you should use zod as your validator?"
date: 2023-03-07T16:00:00-03:00
draft: false
toc: true 
readTime: true
---

# Why you should use zod as your validator?

The title is just to bring some attention... I know, you use what you want,
but today I'll be giving some insights on zod, from someone that is using it daily for the last 6-8 months.

To keep it simple, I'm gonna omit most of the `express`
app setup because it's kinda easy to do, however you can find it in their [documentation](https://github.com/expressjs/express).

------------------------------------
## Installation
```bash
$ npm i zod express typescript body-parser
$ npm i -g npx
$ npx tsc --init
$ npm i --save-dev @types/express
```

## Practical examples
Let's say that you have an `express` api with a `/user` endpoint.

```ts
// src/index.ts
import express, { Express } from 'express';
import bodyParser from 'body-parser';

import User from './controller/user.controller';

const app: Express = express();

// parse application/json
app.use(bodyParser.json())

app.post('/user', User.create);

app.listen(3000, () => {
  console.log(`⚡️[server]: Server is running at http://localhost:${3000}`);
});
```

You usually would do something like:

```ts
// controller/user.controller.ts
import { Request, Response } from 'express';

const User = (() => {
    const create = async (req: Request, res: Response) => {
        const { name, age } = req.body;

        // ...

        return res.send({ name, age })
    }

    return {
        create,
    }
})();


export default User;
```
But as we can see,
we lack typing inference, type validation, and a lot of other things.


Also you could say something like:

> **1garo, you are wrong, I know better!.**

```ts
// controller/user.controller.ts

import { CreateUser } from '@/types/user.typings

/// ...
const { name, age } = req.body as CreateUser;
```

Well, you are right! Kinda...

I just don't wanna be there when the **user** sends something wrong (and he WILL! I ensure you of that).

## Let me show you the `zod` way!

One of the things that I really like about `zod` is this "composition" approach. It
makes me feel that I'm writing in a functional language and it's very easy to use,
so let's solve the problem that we had discussed.

We have to describe our schema, the one to be validated.

```ts
// src/validation/user.validation.ts
import z from 'zod';

export const userValidation = z.object({
  name:
  z.string()
    .min(1, { message: "Must be 1 or more characters long" })
    .trim(),
  age:
  z.number()
    .gte(18)
    .int()
    .positive()
});

```

Now back to the last example and let's use our new validator.

```ts
- const { name, age } = req.body as CreateUser;
+ const { name, age } = await userValidation.parse(req.body);
```

Yeahhhh, We did it! Now your input is type safe and has inference.


------------------------------------
I just showed you a tiny bit about what `zod` can do, but you can check it out more
<a href="https://github.com/colinhacks/zod#basic-usage" target="_blank">here</a>.

My favorite functionalities are: `.refine()`, `.transform()`, and `.partial()`.
Maybe I could write another post using those features, I think it could be fun!

See u on the next episode.


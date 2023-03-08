---
title: "why you should use zod as your validator"
date: 2023-03-07T16:00:00-03:00
draft: false
categories: ['Tech']
---
The title is just to bring some attention... I know, you use what you want,
but today I'll be giving some insights on zod, from someone that is using it daily for the last 6-8 months.

To keep it simple, I'm gonna omit most of the `express`
app setup because it's kinda easy, however you can find it in their [documentation](https://github.com/expressjs/express).

We can begin by installing the packages that we're gonna use.
```bash
$ npm install zod express
```

Let's say that you have an `express` api with an `/user` endpoint.

```ts
// src/routes.ts

import { Router } from 'express';
import user from '@/controller/user.controller';

const router = Router();

router.post('/user', user.create);
```

You usually would do something like the example below, but as we can see,
we lack typing inference, type validation, and a lot of other things.

```ts
// src/controller/user.controller

const user = (() => {
    const create = async (req: Request, res: Response) => {
        const {
            name,
            age,
        } = req.body;

        // call a service or whatever you want to do

        return res.send({ age })
    }

    return {
        create,
    }
}

export default user;
```

Also you could say, "_1garo_ you are wrong, I know better!!!". And send me the example below:

```ts
// src/controller/user.controller

import { CreateUser } from '@/types/user.typings'

const user = (() => {
    const create = async (req: Request, res: Response) => {
        const {
            name,
            age,
        } = req.body as CreateUser;

        // call a service or whatever you want to do

        return res.send({ name, age })
    }

    return {
        create,
    }
}

export default user;
```

Well, you are right! Kinda... I just don't wanna be there when your **user** sends the wrong format (and he WILL! I ensure you of that).

Let me show you the `zod` way!

One of the things that I really like about `zod` is this "composition" approach. It
makes me feel that I'm writing in a functional language and it's very easy to use,
so let's solve the problem that we had discussed.

We have to describe our **type/schema/whateverYouWantToCall**.

```ts
// src/validation/user.validation.ts

export const userValidation = z.object({
  name:
  z.string()
  .min(1, { message: "Must be 1 or more characters long" })
  .trim(),
  age: z.number().gte(13).int().positive().,
});

```

Now back to the last example and let's use our new validator.

```ts
// src/controller/user.controller

import {
    userValidation,
} from '@/validations/user.validation';

const user = (() => {
    const create = async (req: Request, res: Response) => {
        const {
            name,
            age,
        } = await userValidation.parse(req.body);

        // call a service or whatever you want to do

        return res.send({ name, age })
    }

    return {
        create,
    }
}

export default user;
```

Yeahhhh, We did it! Now your input is type safe and has inference.

I just showed you a tiny bit about what `zod` can do, but you can check it out more
<a href="https://github.com/colinhacks/zod#basic-usage" target="_blank">here</a>.

My favorite functionalities are: `.refine()`, `.transform()`, and `.partial()`.
Maybe I could write another post using those features, I think it could be fun!

See u on the next episode.


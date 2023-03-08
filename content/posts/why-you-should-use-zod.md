---
title: "why you should use zod as your validator"
date: 2023-03-07T16:00:00-03:00
draft: false
categories: ['Tech']
---
The title is just to bring some attention, I know, you use what you want,
but today I'll be giving some insights on zod, from someone using it daily for the last 6-8 months.

Let's say that you have an `express` api with an `/user` endpoint.

```ts
// src/routes.ts

import { Router } from 'express';
import user from '@/controller/user.controller';

const router = Router();

router.post('/user', user.create);
```

You usually would do something like the example below, but as we can see,
we lack typing inference, type validation and a lot other things.

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

But you could say, _1garo_ you are wrong, I know better!!! And send me the example below.

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

Well, you are right!!!!! I just don't wanna be there when your **user** send the wrong format,
and he WILL, I ensure you that.

One of the things that I really like about `zod` is this "composition" approach,
makes me feel that I'm writing in an functional language and it's very easy to use,
so let's solve the problem that we talked about.

We are going to need to describe our **type/schema/whateverYouWantToCall**.

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

We did it! Now you have type safe and inference in you input.

I just showed a tiny bit about what `zod` can do, you can check it out more
<a href="https://github.com/colinhacks/zod#basic-usage" target="_blank">here</a>,
my favorite ones are: `.refine()`, `.transform()`, and `.partial()`, see u on the next episode.


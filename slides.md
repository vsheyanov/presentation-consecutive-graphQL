---
# try also 'default' to start simple
theme: dracula
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
# highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-left
# use UnoCSS
# css: unocss
---

# Consecutive GraphQL mutations 

with offline support

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Plan
- About me
- App history
- Technologies of the talk
- App stack & requests example
- Problem definition, solution direction and acceptance criteria
- GraphQL & Apollo
- **New Consecutive link**
- Summary
- **Offline cherry on top**
- Wrap-up

---

# Who am I?

<div v-click>

- In software development for 10+ years
- Worked for companies with number of employees
  - 0 -> 20 successful start-ups
  - 100 000+ employees (3-5 letter companies)
- JS/TS ecosystems 
- 1st language was AS (ActionScript 3.0) for Flex Framework

</div>

<div class="grid grid-cols-2">

<div v-click>

My hobbies are:
- snowboarding (20+ years)
<img class="w-30" src="/snowboard.jpg"/>
- kitesurfing (< 1 year)

</div>

<div v-click>

At work:
- building complex systems
- mentoring junior developers
<img src="/ducks.gif"/>

</div>

</div>

---
layout: center
---

# App history

---

# Road to the present state

<v-clicks>

- mobile app ~5 years in development, 4 years in production
- backend ~6 years in development
- in the Frontend-end team only 1 team member left from the initial implementation team
- the plan for the product is
  - support legacy features
  - rebuild old stuff when possible
  - add new features

</v-clicks>

---
layout: center
---

# Technologies of the talk 

---

# Technologies of the talk 
Warm up your hands

<v-clicks>

<img src="/logo_js.png" class="absolute top-35 left-20 w-1/5"/>
<img src="/logo_ts.png" class="absolute top-35 left-100 w-1/7 "/>
<img src="/logo_rn.png" class="absolute top-30 left-180 w-1/6"/>
<img src="/logo_apollo.webp" class="absolute top-85 left-20 w-1/5"/>
<img src="/logo_appsync.png" class="absolute top-115 left-30 w-1/9"/>
<img src="/logo_salesforce.png" class="absolute top-100 left-3/7 w-1/7"/>
<img src="/wat.png" class="absolute top-100 left-8/10 w-1/7"/>

</v-clicks>

<!--
## JS / React
Nothing extra complex. It's a JS and React, so think of 1 week of online courses online and you can call yourself a Senior Developer. Basic language only.

## React-Native.
Same here - we'll use only a basic application to shows some UI elements and we'll add a GraphQL library

## GraphQL
Once added, we'll use basic feauters like queries/mutations, nothing that you won't be able to do after a
day of going through basic tutorials.

### It will become HARD 
Once we start implementing links and going into the source code of GraphQL Apollo to see how the 
internals are implemented

## WAT 
You won't survive this talk with an appeciation or acceptance of WAT existence. I'm sorry
-->

---
layout: center
---

# Stack & requests example

---

# Company's stack

<img src="/overview-app.png"/>

<!--

## React-Native
iOS / Android app

## GraphQL
Client - Apollo Graphql
Server - AWS Appsync

## Data source
Salesforce Rest API

Tell about 1 to 1 connection between lambdas and Rest API
-->

---

# Typical request example 

<br/><br/>

<div class="grid grid-cols-5 gap-6">
  <div class="col-span-2">
  
  ```ts
    updateOrderChecklist({
      door: 'checked'
    });
    ...
    updateOrderChecklist({
      boiler: 'works'
    });
    ...
    updateOrderChecklist({
      boiler_CO2: 33.3
    });
  ```
  
  </div>

  <div class="col-span-3">
    <img src="/overview-app.png"/>
  </div>
  
</div>

<!-- 

## Typical request 
A technician fills out a questionaire. Fast clicks through the list.

TODO: add app screenshot

-->

---
layout: center
---

# Problem definition, solution and acceptance criteria
---
layout: image-right
image: /salesforce.jpeg
---

# Backend from a frontend team perspective

Limits that we need to know about and respect

<v-clicks>

- number of long-running requests
- number of requests
- DB locks

</v-clicks>

<!-- 

## Long-runnig request
Explain requests in parallel

## Number of API requests
Typical limits hourly/daily/monthly

# DB locks
SF is a massive DB. Depending on the specific usage and automations you may have kind of locks. 
Example - Order and changes for it.

-->

---

# Problem 

<style>
  .img {
    height: 70%;
    margin-top: 30px;
  }
</style>

<br/>
<img class="img mx-auto" src="/update-parallel.png"/>

<!-- 
# Problems in numbers
* Update #1 takes 500ms - 3seconds to complete
* User clicks fast, as a result we have 2-3 overlapping requests
* On the Frontend we usually don't care, it's not our problem, Backend must handle it.
* In our case it was not an option. FE teams was told that this problem CANNOT be fixed on the BE.
-->

---

# Solution direction

<div>Updates must be done in sequence</div>
<br/>

<v-click>

From Salesforce point of view this will will help us:
- having fewer long lasting requests
- fewer DB locks

</v-click>


<!-- 
If we cannot run requests in parallel, we have to run them consecutively. One after another, yes.

- fewer long lasting requests
- since we’ll have fewer requests in parallel
-->

---

# How it should be

<style>
  .row {
    display: flex;
    align-items: center;
    flex-direction: row;
  }
  .img {
    height: 70%;
    margin-top: 30px;
  }
  .second {
    margin-left: 30px;
  }
  .third {
    margin-left: 80px;
  }
</style>

<br/>
<img class="img mx-auto" src="/consecutive.png"/>

---

# Success criteria

<img class="mx-auto w-3/5" src="/customer_first.png"/>

* from customer point of view nothing changes
* mutations are executed in sequence

<!-- 
- they should still be able work with app without noticing ANY delays, be able to click fast etc.
- this is known only by devs
-->

---
layout: center
---

# GraphQL & Apollo

---

# GraphQL links

<img src="/graphql-link.png"/>

GraphQL offers a way to customize Apollo Client's data flow with something called Links:
* logging link
* error handler link
* execution time link

The key here is **Link chain**.

<v-click>
  <img class="absolute top-30 left-60 w-1/2" src="/chain-breaker.png"/>
</v-click>

---

# Why not break the chain?

<div grid="~ cols-2" class="justify-items-center pt-15">

<img class="w-1/2" src="/forward.webp"/>

<img class="w-1/2" src="/back.webp"/>

Forward down the chain

Back up the chain

</div>

---
clicks: 3 
---

# What is a link?

```ts {3|5|5|7}
import { ApolloLink } from '@apollo/client';

const timeStartLink = new ApolloLink((operation, forward) => {
                  
  const result = forward(operation);

  return result;
});
```

The forward function's return type is an Observable provided by the **zen-observable** library. See the 
zen-observable documentation for details.


<img class="absolute w-3/4" src="/link_1.png"/>
<img v-click="1" class="absolute w-3/4" src="/link_2.png"/>
<img v-click="2" class="absolute w-3/4" src="/link_3.png"/>
<img v-click="3" class="absolute w-3/4" src="/link_4.png"/>

<!--
Hijack control over return - use Observable
 -->

---

# GraphQL mutation

<div grid="~ cols-2 gap-5">

```ts {all|3-4|7-9|all|0}
// Mutation definition
const UPDATE_COMMENT = gql`
  mutation UpdateComment($commentId: ID!, 
    $commentContent: String!) {
    updateComment(commentId: $commentId, 
      content: $commentContent) {
      id
      __typename
      content
    }
  }
`;
```

```ts {0|3|7-8|9-15}
// Component definition
function CommentPageWithData() {
  const [mutate] = useMutation(UPDATE_COMMENT);
  return (
    <Comment
      updateComment={({ commentId, commentContent }) =>
        mutate({
          variables: { commentId, commentContent },
          optimisticResponse: {
            updateComment: {
              id: commentId,
              __typename: "Comment",
              content: commentContent
            }
          }
        })
      }
    />
  );
}
```

</div>

---

# OrderChecklist updates

```ts
updateOrderChecklist({
  variables: {
    id: ‘order_1’,
    door: 'checked'
  },
  optimistic: {
    id: ‘order_1’,
    door: 'checked',
    price: 50
  },
});
```

<!-- At this point whenever a mutation is performed, user will always see a result of an optimistic response first and in 99.9% cases optimistic response will match the actual response.
But requests are still executed in parallel. Let’s fix it. -->

---
layout: center
---

# Let's create a link!

---

# Empty link

```ts {all|1-7|7-|11|8,13}
// operation - contains your variables, mutation name
// forward - pushes your request down the chain link

export const queuedLink = new ApolloLink((operation, forward) => {
  return forward(operation);
});

const linkChain = ApolloLink.from([
  authLink,
  errorLink,
  queuedLink,
  responseLogger,
  httpLink
]);
```

<img class="pt-4" src="/link-regular.png"/>

---

# Create a queue

```ts {1,4-7|all}
const queue = [];

export const queuedLink = new ApolloLink((operation, forward) => {
  queue.push({
    forward,
    operation,
  });
  return forward(operation);
});
```

<img class="" src="/link-new-queue.png"/>

---

# Return Observable, save item in the queue

<div v-click-hide>

```ts {4-20|9|all}
const queue = [];

export const queuedLink = new ApolloLink((operation, forward) => {
  
  return new Observable((observer) => {
    queue.push({
      operation,
      forward,
      observer,
    });

    return () => {};
  });
});
```

</div>

<img v-after class="pt-4 absolute top-40 w-4/5" src="/link-return-observer.png"/>

---

# Execute queue #1

```ts {3-5,16|all}
const queue = [];

const executeQueue = async () => {
  // execute an operation and return the result
}

export const queuedLink = new ApolloLink((operation, forward) => {

  return new Observable((observer) => {
    queue.push({
      operation,
      forward,
      observer,
    });

    executeQueue();

    return () => {};
  });
});
```

---

# Execute queue #2

```ts {3,6-17|all}
const queue = [];

let queueRuning = false;

const executeQueue = async () => {
  if (queue.length === 0 || queueRuning) {
    return;
  }

  queueRuning = true;

  const nextItem = queue.shift();

  // do some async work

  queueRuning = false;
  executeQueue();
 }

export const queuedLink = new ApolloLink((operation, forward) => {
  .......
```

---

# Execute queue #3

```ts {3-18|3|5|6|7-10|11-13|14-17|all}
const executeQueue = async () => {
  ...
  const { operation, forward, observer } = queue.shift();

  forward(operation)
    .subscribe(
      async (result) => {
        observer.next(result);
        observer.complete();
      },
      (error) => {
        observer.error(error);
      },
      () => {
        queueRuning = false;
        executeQueue();
      },
    );
}

export const queuedLink = new ApolloLink((operation, forward) => {
```

---

# All together


<style>
.all-code pre {
  font-size: 0.7em !important;
  line-height: 10px !important;
}
</style>


<div class="all-code">

```ts
const queue = [];

let queueRuning = false;

const executeQueue = async () => {
  if (queue.length === 0 || queueRuning) {
    return;
  }

  queueRuning = true;

  const { forward, operation, observer } = queue.shift();

  forward(operation)
    .subscribe(
      async (result) => {
        observer.next(result);
        observer.complete();
      },
      (error) => {
        observer.error(error);
      },
      () => {
        queueRuning = false;
        executeQueue();
      },
    );
}

export const queuedLink = new ApolloLink((operation, forward) => {
  return new Observable((observer) => {
    queue.push({
      operation,
      forward,
      observer,
    });
    executeQueue();
    return () => {};
  });
});
```

</div>

---

# Summary of what was done

### What did we achieve?

<v-clicks>

1. it works
2. we have something in our lives to be shamed about

</v-clicks>

### Important considerations

<v-clicks>

- All mutations that go to consecutive queue must have an optimistic response
- Optimistic response should be correct
- IDs

</v-clicks>

<!-- 

- To make sure a user doesn’t see any difference
- Otherwise a user will see UI changing after the actual response is received
- IDs handling makes this implementation A LOT harder

-->
---
layout: center
---

# Offline cherry on top

---

# Cherry on top
### Let's make it work in offline!

```ts {4-12,16}
const queue = [];

let queueRuning = false;
let isOffline = false;

NetInfo.addEventListener(state => {
  isOffline = !state.isInternetReachable
  
  if (!isOffline) {
    executeQueue();
  }
});

const executeQueue = async () => {
  if (queue.length === 0 || queueRuning 
    || isOffline) {
    return;
  }
```

---

# Further thoughts on IDs
### A rabbit starts it's journey

<p v-click>
* IDs
</p>

<p v-click>
&nbsp;&nbsp; * Dictionary?

  ``` ts
  const ids = {
    "TEMP_LOCAL_ID_1": "KA71LS07SF03865",
    ...
  }
  ```
</p>

<p v-click>
&nbsp;&nbsp;&nbsp;&nbsp; * Multiple updates (update/delete product, order, settings etc.)
</p>

<p v-click>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; * Automatic vs manual patches
</p>

<!-- 

- If you create a new object and then edit it before you received a response from server, 
you don’t know what Object.ID you want to update/delete.

- Can be fixed by having a dictionary with key-value pairs where the key is temp ID and 
  value is an actual ID after a response is receive. 

- For every mutation you’ll need to go through all variables in the payload and replace 
  temp value with actual one if found - all of them needs to be checked.

- Automated iteration in every field of the payload (may be intensive) or custom logic for 
  some of the mutations (needs to be maintained separately) - your choice.

-->

---
layout: center
---

<style>
  .imgg {
    height: 300px;
  }
</style>

# Don't fall into it

<div grid="~ cols-4">
  <span grid="~ col-span-1"></span>
  <img grid="~ col-span-2" src="/rabbit.jpeg"/>
  <span grid="~ col-span-1"></span>
</div>

<v-click>

<h1 class="text-right">It won't be that pretty!</h1>

</v-click>

---

# Wrap-up
Final thoughts

<v-clicks>

- if you have a choice, don't do it
- if you need to do it, think of edge-cases
- if you want to learn about Apollo, links, caching and optimistic responses:
  - read documentation
  - open source code
- if you ever need to find a creative solution like that, embrase the opportunity and try 
enjoying the process

</v-clicks>

---

# Credits

Images:
- https://www.reddit.com/r/DaenerysWinsTheThrone/comments/103jlnc/breaker_of_chains/
- https://www.apollographql.com/docs/react/api/link/introduction/
- https://www.vitakraft.nl/vl/tips/sos-vitakraft-eerste-hulp-bij-selectieve-knabbelaars/

Tech:
- https://www.apollographql.com/
- https://reactnative.dev/
- https://www.typescriptlang.org/
- https://aws.amazon.com/appsync/
- https://salesforce.com/
- https://miro.com/
- https://sli.dev/

---

# How to reach me?

Find me after the talks in a hall

<div grid="~ cols-2" class="justify-items-center">

<img class="w-70" src="/qr_linkedin.png"/>

<img class="w-55" src="/qr_telegram.jpeg"/>

https://www.linkedin.com/in/victor-sheyanov/ 

https://t.me/vsheyanov

</div>

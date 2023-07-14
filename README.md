# README: GitPod Starter Edition

Get started quickly and efficiently by launching [RedwoodJS](https://redwoodjs.com) inside [GitPod](https://gitpod.io)!

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/redwoodjs/gitpod-starter).

### GitPod + Redwood

This repository enables you to easily launch [RedwoodJS](https://redwoodjs.com/) projects inside GitPod.

It provides a **preconfigured development environment** with all the necessary tools and dependencies, allowing you to focus on building your RedwoodJS application without worrying about the setup.

## GitPod: Getting Started

Click on the Open in Gitpod button (top of page above).

This will launch GitPod and ask you to configure a new workspace. Click continue.

GitPod will then begin to build your workspace. This may take several minutes.

What's going on behind the scenes:
- GitPod is setting the workspace set up
- It installs our recommended VS Code plugins:
- It installs the latest stable version of Redwood as a TypeScript project
- It runs `yarn install`, adding all the dependencies for the project
- Changes the database to a **Postgres database**

> **Project Language Target**
> The project default language target is **TypeScript**; however, you can change it to **JavaScript** if you prefer via the command:
> `yarn redwood ts-to-js`

Once everything is up and running, you can click on the **Ports** tab

You can click on the address or the globe icon to open that particular port in a new tab.

- Port `5432` is the database. So, if you click on that port, you'll probably see a "Port 5432 Not Found" error, but it is working!
- Port `8910` is your frontend
- Port `8911` is your backend and will show you a list of all available functions. If you add `/graphql` to the end of the URL, you should see the GraphQL Playground

### Restarting the Workspace

If you need to restart the dev server, you can't just run `yarn rw dev`, you'll run into an "Invalid Host File" error.

Since we’re running in a cloud workspace, URLs like `localhost:3000` should be converted to something like `3000-abc-123.ws-eu0.gitpod.io.` ([Additional documentation](https://www.gitpod.io/guides/gitpodify#invalid-host-header).)

The following command allows us to forward the `--client-web-socket-url` to the GitPod URL.

```bash
yarn rw dev --fwd="--client-web-socket-url=ws$(gp url 8910 | cut -c 5-)/ws"
```

## Redwood: Getting Started

> **The Redwood CLI**
> From dev to deploy, the CLI is with you the whole way.
> And there's quite a few commands at your disposal:
> ```
> yarn redwood --help
> ```
> For all the details, see the [CLI reference](https://redwoodjs.com/docs/cli-commands).

### Prisma and the database

Redwood wouldn't be a full-stack framework without a database. It all starts with the schema. Open the [`schema.prisma`](api/db/schema.prisma) file in `api/db` and replace the `UserExample` model with the following `Post` model:

```
model Post {
  id        Int      @id @default(autoincrement())
  title     String
  body      String
  createdAt DateTime @default(now())
}
```

Redwood uses [Prisma](https://www.prisma.io/), a next-gen Node.js and TypeScript ORM, to talk to the database. Prisma's schema offers a declarative way of defining your app's data models. And Prisma [Migrate](https://www.prisma.io/migrate) uses that schema to make database migrations hassle-free:

```
yarn rw prisma migrate dev

# ...

? Enter a name for the new migration: › create posts
```

> `rw` is short for `redwood`

You'll be prompted for the name of your migration. `create posts` will do.

Now let's generate everything we need to perform all the CRUD (Create, Retrieve, Update, Delete) actions on our `Post` model:

```
yarn redwood g scaffold post
```

Navigate to http://localhost:8910/posts/new, fill in the title and body, and click "Save":

Did we just create a post in the database? Yup! With `yarn rw g scaffold <model>`, Redwood created all the pages, components, and services necessary to perform all CRUD actions on our posts table.

### Frontend first with Storybook

Don't know what your data models look like?
That's more than ok—Redwood integrates Storybook so that you can work on design without worrying about data.
Mockup, build, and verify your React components, even in complete isolation from the backend:

```
yarn rw storybook
```

Before you start, see if the CLI's `setup ui` command has your favorite styling library:

```
yarn rw setup ui --help
```

### Testing with Jest

It'd be hard to scale from side project to startup without a few tests.
Redwood fully integrates Jest with the front and the backends and makes it easy to keep your whole app covered by generating test files with all your components and services:

```
yarn rw test
```

To make the integration even more seamless, Redwood augments Jest with database [scenarios](https://redwoodjs.com/docs/testing.md#scenarios)  and [GraphQL mocking](https://redwoodjs.com/docs/testing.md#mocking-graphql-calls).

### Ship it

Redwood is designed for both serverless deploy targets like Netlify and Vercel and serverful deploy targets like Render and AWS:

```
yarn rw setup deploy --help
```

Don't go live without auth!
Lock down your front and backends with Redwood's built-in, database-backed authentication system ([dbAuth](https://redwoodjs.com/docs/authentication#self-hosted-auth-installation-and-setup)), or integrate with nearly a dozen third party auth providers:

```
yarn rw setup auth --help
```

### Next Steps

The best way to learn Redwood is by going through the comprehensive [tutorial](https://redwoodjs.com/docs/tutorial/foreword) and joining the community (via the [Discourse forum](https://community.redwoodjs.com) or the [Discord server](https://discord.gg/redwoodjs)).

### Quick Links

- Stay updated: read [Forum announcements](https://community.redwoodjs.com/c/announcements/5), follow us on [Twitter](https://twitter.com/redwoodjs), and subscribe to the [newsletter](https://redwoodjs.com/newsletter)
- [Learn how to contribute](https://redwoodjs.com/docs/contributing)

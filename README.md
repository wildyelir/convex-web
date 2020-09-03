![Tests](https://github.com/Convex-Dev/convex-web/workflows/Tests/badge.svg)

## Development

### Prerequisites
- [Java Development Kit 11](https://adoptopenjdk.net/)
- [Clojure CLI](https://clojure.org/guides/getting_started#_clojure_installer_and_cli_tools)

### App

Install dependencies:

```
npm install
```

Compile CSS:
```
bin/compile_css
```

Run Shadow CLJS:
```
npm run watch
```

### Server

Run REPL with the `dev` alias:
```
clj -A:dev
```

Start server on port 8080:
```
(go)
```

## Production

### App

Install dependencies:

```
npm install
```

Compile & bundle app:
```
bin/compile_app
```

Compile CSS:
```
NODE_ENV=production bin/compile_css
```

The `NODE_ENV` environment variable is very important because
that's how Tailwind knows it should prune unused classes.

### Server

```
bin/run
```

### Create a Linux `systemd` Service

- Copy:
    `deployment/convex_web.service` to `/etc/systemd/system/convex_web.service`;
- Enable:
    `sudo systemctl enable convex_web`
- Start:
    `sudo systemctlm start convex_web`
    
## Logging

Google Cloud Logging is used for log storage, and there are two types of logs in Convex Web:
- SLF4J/Logback for Convex and libraries,
- and u/Log events for structured logs.

### u/Log Events

| Event | Description |
|:--|:--|
| `:logging.event/endpoint` | Logged for every request |
| `:logging.event/system-error` | Whenever an unexpected Exception is raised  |
| `:logging.event/user-error` | User errors e.g.: Faucet amount above limit  |
| `:logging.event/repl-user` | What users type in the REPL  |
| `:logging.event/repl-error` | Errors users make in the REPL  |
| `:logging.event/faucet` | Faucet request  |
| `:logging.event/new-account` | User created a new account |
| `:logging.event/confirm-account` | User confirmed the account  |

There's a Google Cloud Logging Sink set up to send events log to Big Query. Once our events log are in Big Query we can use SQL to query all different sort of things. (There are some saved queries in project)

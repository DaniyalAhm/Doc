# Index

This file handles the initialization of the game server, including setting up the `AdminContext`, registering game components such as `Classic`, `Lobby`, and custom callbacks using `Empirica`. Additionally, it sets up error handling and logging.

## Table of Contents

- [Command Line Arguments](#command-line-arguments)
- [AdminContext Initialization](#admincontext-initialization)
- [Component Registration](#component-registration)
- [Error Handling](#error-handling)

---

## Command Line Arguments

The application accepts several command-line arguments using `minimist` for configuration:

```javascript
const argv = minimist(process.argv.slice(2), { string: ["token"] });
```

### Supported Arguments:
- `token`: Used to pass a session token.
- `url`: URL of the game server. Defaults to `http://localhost:3000/query`.
- `sessionTokenPath`: Path for the session token.
- `loglevel`: Defines the logging level. Defaults to `info`.

---

## AdminContext Initialization

The `AdminContext.init` method initializes the game environment by connecting to the game server. It also registers the necessary components such as the classic game loader and lobby.

```javascript
const ctx = await AdminContext.init(
  argv["url"] || "http://localhost:3000/query",  // Server URL
  argv["sessionTokenPath"],                      // Session token path (optional)
  "callbacks",                                   // Context name (used for identification)
  argv["token"],                                 // Token for authentication
  {},                                            // Additional options
  classicKinds                                   // Classic game types
);
```

- **Server URL**: Uses `localhost:3000` as the default URL for the game server.
- **Session Token Path**: Optionally provides a path to a session token.
- **"callbacks"**: Name for the context.
- **Token**: If passed via command line, it’s used to authenticate the session.
- **Game Types**: The `classicKinds` argument registers the classic game kinds.

---

## Component Registration

Once the `AdminContext` is initialized, various game components are registered:

```javascript
ctx.register(ClassicLoader);
ctx.register(Classic());
ctx.register(Lobby());
ctx.register(Empirica);
```

### Components Registered:
- **`ClassicLoader`**: Loads classic game types.
- **`Classic`**: Registers classic game logic and stages.
- **`Lobby`**: Handles player matchmaking and pre-game activities.
- **`Empirica`**: Custom callback logic defined in the `Empirica` module.

---

## Ready Event Listener

The server is considered "ready" when it starts successfully. This is indicated using the `ready` event.

```javascript
ctx.register(function (_) {
  _.on("ready", function () {
    info("server: started");
  });
});
```

- **Event**: The server listens for the `ready` event to indicate that it's fully initialized and ready to start.

---

## Error Handling

The process listens for any unhandled promise rejections and logs the error to the console before exiting with a failure code.

```javascript
process.on("unhandledRejection", function (reason, p) {
  process.exitCode = 1;
  console.error("Unhandled Promise Rejection. Reason: ", reason);
});
```

This ensures that unexpected errors are caught, logged, and handled appropriately, preventing the server from crashing silently.

---
```
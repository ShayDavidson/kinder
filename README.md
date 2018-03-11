# kinder

Easily create simple shell commands with a local config file.

## Installation

Run this in your project folder:

```
npm install --save kinder
```

## Usage

Here is an example for a shell command called `mycommand`, configured via `kinder`:

```javascript
const Kinder = require("kinder");

Kinder({
  name: "mycommand",
  commands: {
    default: {
      description: "The default command which runs when you simply enter `mycommand` in your shell.",
      execute: ({ info, hostname, users, repos} = config) -> { /* do something with config argument */ }
    },
    custom: {
      description: "A custom command which runs when you enter `mycommand custom` in your shell.",
      execute: ({ info, hostname, users, repos} = config) -> { /* do something with config argument */ }
    }
  },
  props: {
    info: {
      description: "Essential information",
      required: true
    },
    hostname: {
      description: "Your website hostname",
      default: "my.website.com"
    }
  },
  lists: {
    users: {
      description: "Users to query"
    },
    repos: {
      description: "Repos to query"
    }
  }
});
```

Assuming this is an `index.js` file, in order to make it a shell command, add a `bin` property to your project's `package.json`, like so:

```json
"bin": {
  "mycommand": "./index.js"
}
```

Assuming the above configuration, you'll now have these shell commands at you disposal:
- `mycommand` - Executes the function under the `commands.default.execute`. The function will receive your persisted configuration object as its first argument.
- `mycommand custom` - Executes the function under the `commands.custom.execute`. The function will receive your persisted configuration object as its first argument.
- `mycommand reset` - Resets your local config.
- `mycommand get info` - Shows the saved info value in your local config.
- `mycommand set info <info>` - Sets <info> as the saved info property in your local config.
- `mycommand get hostname` - Shows the saved hostname value in your local config. Defaults to `my.website.com`.
- `mycommand set hostname <hostname>` - Sets <hostname> as the saved hostname property in your local config.
- `mycommand list users` - Lists all the users in your local config.
- `mycommand add users <users, command separated>` - Adds <users> (command separated) to the users list in your local config.
- `mycommand remove users <users, command separated>` - Removes <users> (command separated) from the saved users list in your local config.
- `mycommand list repos` - Lists all the saved repos in your local config.
- `mycommand add repos <repos, command separated>` - Adds <repos> (command separated) to the repos list in your local config.
- `mycommand remove repos <repos, command separated>` - Removes <repos> (command separated) from the saved repos you in your local config.
- `mycommand help` - Shows an help message with all the above commands.

## Tips

- `required` properties which are not set in your local config, will trigger entering their values whenever you run the default or a custom command.
- Kinder validates your config object for any missing essential keys, typos or unsupported keys.

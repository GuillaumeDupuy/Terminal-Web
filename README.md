# Terminal-Web

Simple command-line terminal for your browser app.

Web apps are great. But sometimes instead of all the double-clicks,
mouse pointers, taps and swipes across the screen - you just want
good old keyboard input. This terminal runs in a browser, desktop or mobile. It provides a simple
and easy way to extend the terminal with your own commands.

Enjoy.

## How to use

Include `terminal.js` and `terminal.css` in your HTML:

```html
<script src="terminal.js"></script>
<link rel="stylesheet" media="all" href="terminal.css">
```

Define an HTML div tag where the terminal will be contained:

```html
<div id="terminal"></div>
```

Create a new terminal instance and convert the DOM element
into a live terminal.

```js
var terminal = new Terminal('terminal', {}, {});
```

This won't be terribly useful because by default the terminal
doesn't define any commands. It's meant to be extended. Let's
define some:

```js
var terminal = new Terminal('terminal', {}, {
  execute: function(cmd, args) {
    switch (cmd) {
      case 'clear':
        terminal.clear();
        return '';

      case 'help':
        return 'Commands: clear, help, theme, version, ls, ll, cd, pwd, cat, sudo, open, rm, rmdir, mkdir, touch, echo, date, hostname, grep, tail, ping <br>';

      case 'theme':
        if (args && args[0]) {
          if (args.length > 1) return 'Too many arguments';
          else if (args[0].match(/^dark-light|dark|white|kali|ubuntu$/)) { terminal.setTheme(args[0]); return ''; }
          else return 'Invalid theme';
        }
        return terminal.getTheme();

      case 'ver':
      case 'version':
        return '1.0.2';

      default:
        // Unknown command.
        return false;
    };
  }
});
```

This gives us 21 commands: `clear`, `help`, `theme`, `version`, `ls`, `ll`, `cd`, `pwd`, `cat`, `sudo`, `open`, `rm`, `rmdir`, `mkdir`, `touch`, `echo`, `date`, `hostname`, `grep`, `tail`, `ping`.
[Go ahead and try it out using our live example.](https://guillaumedupuy.github.io/Terminal-Web/terminal.html)

## Features

### Welcome message

The default welcome message is empty but you can set it to be anything.

```js
var terminal = new Terminal('terminal', {welcome: 'Welcome to awesome terminal!'}, {});
```

### Command-line history

History is automatically kept track of. You can use up and down arrow keys to
go through the command-line history, just like a standard shell.

### Prompt and separator

You can change the prompt. The default is an empty prompt with a `>` separator.
Set the prompt option to change.

```js
var terminal = new Terminal('terminal', {prompt: 'C:\\', separator: '&gt;'}, {});
```

This will display the prompt like this: `C:\>`

### Theme

There are 6 built-in themes:

* `dark-light`
* `dark`
* `white`
* `kali`
* `ubuntu`
* `hacker`

`dark-light` theme is the default

`dark` is a clean modern theme that's white on black, while the `white` theme is
black on white.

If you're nostalgic about the old-school CRT monitors then the `dark-light` theme is for you.

Set the theme in options:

```js
var terminal = new Terminal('terminal', {theme: 'dark-light'}, {});
```

Or alternatively use the API to change the theme after creating the terminal object.

### Command extensions

The default terminal doesn't provide any built-in commands. You must define your
own set of commands. This can be done through options when creating the object.

```js
var terminal = new Terminal('terminal', {}, {
  execute: function(cmd, args) {
    switch (cmd) {
      case 'hello':
        return 'world';

      // Place your other commands here.

      default:
        // Unknown command.
        return false;
    };
  }
});
```

When executing commands you can run whatever code you want. Just return a string
that you want to be displayed in the terminal as the output of your command. The
example above would produce:

```
> hello
world
> 
```

Return `false` to indicate that this is an unknown command.

You can have more than one command set and you can list them out one after another:

```js
var terminal = new Terminal('terminal', {}, {
  execute: function(cmd, args) {
    switch (cmd) {
      case 'hello': return 'world';
      default: return false;
    };
  }
},{
  execute: function(cmd, args) {
    switch (cmd) {
      case 'echo': return args[0];
      default: return false;
    };
  }
});
```

You might want to use multiple command sets, for example, if your app has modules
that hook into the terminal independently.

### Local storage persistence

Command-line history is persisted using HTML5 local storage.

## API

### Clear the screen

```js
terminal.clear();
```

### Change the theme

```js
terminal.setTheme('dark-light');
```

Sets CSS class `terminal-dark-light` on the terminal DOM element. The three built-in
themes are `dark-light`, `dark`, `white`, `kali`, `ubuntu` and `hacker`. You can also make your own theme and
pass in your theme's name into `setTheme`.

To retrieve the name of the active theme, use `getTheme`.

```js
console.log(terminal.getTheme());
```

### Set the prompt

```js
terminal.setPrompt('test');
```

Because the default separator is `$` this will display:

```
test>
```

You can set the separator in options while creating the Terminal object. At this
time it's not possible to change the separator after the terminal is created.

Prompt can be changed at any time.
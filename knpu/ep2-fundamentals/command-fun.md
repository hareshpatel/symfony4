# Fun with Commands

Ok, let's make our command a bit more awesome. Give it a description: "Returns some
article stats". Each command can have *arguments* - which are strings passed after
the command and *options*, which are prefixed with `--`, like `--option1`.

Let's rename the argument to `slug`, change it to `InputArgument::REQUIRED` - which
means that you *must* pass this argument to the command, and give it a description:
"The article's slug". Rename the option to `format`: I want to be able to say
`--format=json` to get the article stats as JSON. Change this to `VALUE_REQUIRED`:
instead of just `--format`, this means we need to say `--format=json`. Update its
description, *and* give it a default value: `text`.

Perfect! We're not *using* these options yet, but we can already go back and run
the command with a `--help` flag:

```terminal
php bin/console article:stats --help
```

You can add `--help` to *any* command to get all the info about it - like the
description, arguments and options... including a bunch of options that are built
in to all commands.

## Customizing our Command

Ok, so the `configure()` method is where we set things up. But `execute()` is where
the magic happens. We can do *whatever* we want down here!

To get the argument value, update the `getArgument()` call to `slug` and rename
the variable too. Let's just invent some article "data" - give this array a `slug`
key and, how about, `hearts` set to a random number between 10 and 100 for now.

Clear out the rest of the code, and then add a `switch` statement on
`$input->getOption('format')`. Here's the plan: we're going to support two different
formats: `text` - don't forget the `break` - and `json`. If someone tries to use
a different format, yell at them! 

> What kind of crazy format is that?

## Printing Things

Notice that `execute()` is passed two arguments: `$input` and `$output`. Input
lets us *read* arguments and options. And, you can even use it to ask questions
interactively. `$output` is all about *printing* things. To make both of these
even *easier* to use, we have a special `SymfonyStyle` object that's *full* of
shortcut methods.

For example, to print a list of things, just say `$io->listing()` and pass the
array.

For `json`, to print raw text, use `$io->write()` - then `json_encode($data)`.

And... we're done! Let's try this out! Find your terminal and run:

```terminal
php bin/console article:stats khaaaaaan
```

Nice! And now pass `--format=json`:

```terminal-silent
php bin/console article:stats khaaaaaan --format=json
```

Woohoo!

## Printing a Table

But... this listing isn't very helpful: it just prints out the *values*, not the
keys. What does this 88 mean?

Instead of using listing, let's create a *table*. 

Start with an empty `$rows` array. Now loop over the data as `$key => $val` and
start adding rows with `$key` and `$val`. We're doing this because the SymfonyStyle
object has an awesome method called `->table()`. Pass it an array of headers -
`Key` and `Value`, then `$rows`.

Let's rock! Try the command again without the `--format` option:

```terminal-silent
php bin/console article:stats khaaaaaan
```

Oh, *so* much better. And yea, that `$io` variable has a *bunch* of other features,
like interactive questions, a progress bar and more. Not only are commands *fun*,
but they're super easy to create thanks to MakerBundle.

Oh my gosh, you did it! You made it through Symfony Fundamentals! This was serious
work that will *seriously* unlock you for *everything* else you do with Symfony!
We now understand the configuration system and - most importantly - *services*.
Guess what? Commands are services. So if you needed your `SlackClient` service
here, just add a `__construct()` method and autowire it!

***TIP
When you do this, you need to call `parent::__construct()`. Commands are a rare
case where there is a parent constructor!
***

With our new knowledge, let's keep going and start mastering features, like the
Doctrine ORM, form system, API stuff and a lot more.

Alright guys, seeya next time!
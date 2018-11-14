# Pokelang

A new programming language inspired by pokemon.

## Um... why?

The better question is: why hasn't this been done already?

## Language notes

I haven't begun to build the compiler for pokelang yet; I am still fleshing out how I want the language to work.  My background is predominantly ruby, with some C++, some Elixir, and some other stuff thrown in, and you will definitely see those influences.

My notes:

```
introduce MyPoke evolves from Pokemon
	type OtherPoke  (-o-) mutiple inheritance a la ruby, like “include”

	On_choose:
		(-o-) constructor goes here

	On_use ‘command’’:   (-o-) single quotes are like ruby symbols
		(-o-) do the thing

	Secret  (-o-) scope change to private

	On_use ‘some_secret_command’:
		(-o-) do the thing
```

(-o-) whitespace matters, a la coffeescript or HAML
(-o-) private scope is called “secret” and methods are called “commands”
(-o-) instead of self, keyword is pokemon

- Variables are pokeballs.  They are immutable (a pokeball can only ever contain one thing at a time).  If pokeball contents are empty or nil, the pokeball is available.  Myvar.available → #blank?

- Pokemon is like BaseObject in Ruby

- Scripts require explicit object and usually use StarterPokemon.  StarterPokemon is a special type that will respond to either ‘bulbasaur’, ‘charmander’, or ‘squirtle’ for the print command. ???

‘Say’ command is part of Pokemon pokemon, and is like ruby’s puts
Pokemon’s name is part of Pokemon pokemon, and is like ruby’s print

Hello World script:
```
my_poke = StarterPokemon.choose
my_poke.say “hello, world”  (-o-) or my_poke.bulbasaur “hello, world”
```

(-o-) double quotes define strings
(-o-) single quotes define symbols/atoms, basically names of commands and error reasons

Pokemon don’t “error,” they faint in battle.  To rescue errors, use on_faint.  To raise an error, use faint.

```
use ‘always_true’:
	faint ‘some_faint_reason’
	On_faint ‘some_faint_reason’:
		True
```

Looping
Much like in assembler/basic, you use a special keyword to indicate that looping should happen.  In this case, it’s rematch.

```
(-o-) this will print the numbers 1 - 10
Catch 1 in counter
Counter.release battles 10:
	Lose, tie:
		my_poke.say counter.release
		catch (counter.release + 1) in counter
		rematch
```

(-o-) note  that ‘win’ case was not included
(-o-) when this is reached, it will find no command and move on with the code

Logic comparisons
The only real idea of equivalence in pokemon is the idea of a tie in battle.  Therefore, value comparisons are done with battling, as you can see above.

```
(-o-) this will print “good”
100 battles 100:
	Lose, win: my_poke.say “bad”
	tie: my_poke.say “good”

(-o-) this will return 4
1 battles 4:
	Lose: 4
	Win, tie: 1

(-o-) this will also return 20
20 battles 4:
	Lose: 4
	Win, tie: 20
```

Case exists in the form of a battle party.

```
battle_party letter.release:
	‘A’: “the letter is ‘a’”
	‘B’: “the letter is ‘b’”
	Lose: “the letter is something else”   (-o-) like “else” in ruby
```

Closures
Closures exist.  They are defined with rocket syntax =>
A closure is, in essence, a fancy command a pokeball will use upon capture.  So to use the closure, you have to first catch any arguments, and then use it.

If there are no arguments, there is no need to catch.

```
(-o-) no arguments
Greet => Squirtle.choose.say “hello, my name is squirtle!”
Greet.release

(-o-) one argument
Greet => (pokemon) pokemon.say “hello, my name is (-pokemon-)!”
Greet.catch Squirtle.choose
greet.release

(-o-) two arguments
Greet => (pokemon, greeting) pokemon.say “(-greeting-), my name is (-pokemon-)!”
Greet.catch (Squirtle.choose, “hello”)
Greet.release
```

All three print “hello, my name is squirtle!\n”

Calling greet.release in the latter two cases without first catching values will result in failures, since the code cannot be independently executed.  It would be like doing yield() with the wrong number of arguments in ruby.

```
Greet => (pokemon) pokemon.say “hi there!”
Greet.release   (-o-) Pokemon fainted due to ‘argument required’
```

Also note string interpolation, with (- -).

Something similar happens if you try to issue a command with the wrong number of arguments.

```
introduce TacoMonster:
	on_use ‘top_with’: (topping)
		Catch topping in pokemon.topping  (-o-) @topping = topping in ruby

TacoMonster.choose.use ‘top_with’  (-o-) Pokemon fainted due to ‘argument required’
TacoMonster.choose.use ‘top_with’ (“lettuce”, “tomato”)  (-o-) Pokemon fainted due to “too many arguments”
```

Differentiating between say and poke name could require metaprogramming, which is not a good idea.  Fix that.

Releases are "generations" and are given "regions" to identify them.  Anything in gen 1 (major release 1) is Kanto, including gen 1.2, gen 1.3.4, etc.

Currently unused concepts
- Training/trainers
- Pokecenter
- Team Rocket

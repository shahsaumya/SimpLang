SimpLang - A Simple Language
============================

All values are integers.  All global definitions are functions.  Calls
are not tail recursive.  All functions return values.  Functions are
not first class.

Some pieces in the language grammar, like `end`, are superfluous.
They are there to make parsing and later extension easier.

# Example program

	def fac n =
		loop acc = 1 and
			 i = 2
		in
			if i > n then
        		acc
      		else
				recur (acc * i) (i + 1)
			end
		end
	end

	def main n =
		fac n
	end

# Control constructs

`let` introduces variable bindings.  They are only visible within the
right hand sides of later bindings in the same `let` expression and
the body of the `let` expression.  Later bindings can hide earlier
ones:

    let a = 1
		b = a + 1
	in
		b
	end

gives `2`.

	let a = 31415
	in
		let a = 1
			a = a + 1
		in
			a
		end
	end

also gives `2`, because the first `a` in the inner `let` hides the
outer `a`.

`if` works as expected.  It must have both `then` and `else` clauses.
It considers a value to be false when it is `0`, otherwise true.

`loop` introduces variable bindings like `let`, but it also provides a
jump target for uses of `recur` in the body.  `recur` invocations jump
to the innermost `loop` containing that `recur`, giving new values to
the bound variables.

Functions can only be used in their own definitions or in later
definitions.

# Operators

These are all the operators, in order of precedence:

	and, or
	not
	<, >, <=, >=, ==
	+, -
	*, /

`and` and `or` are logical operators, using shortcut evaluation.  That
means `and` will not evaluate its right hand side if the left hand
side evaluates to `0`, and `or` will only evaluate its right hand side
if its left hand side evaluates to `0`.
[![Build Status](https://travis-ci.org/marcelog/ex_abnf.svg)](https://travis-ci.org/marcelog/ex_abnf)

## About

A parser and interpreter for ABNF grammars. ABNF is defined in:
[https://tools.ietf.org/html/rfc4234](https://tools.ietf.org/html/rfc4234),
which is updated in [https://tools.ietf.org/html/rfc5234](https://tools.ietf.org/html/rfc5234).

## Use example

    iex(1)> grammar = ex_abnf.load_file "samples/ipv4.abnf"
    iex(2)> ABNF.apply grammar, "ipv4address", '250.246.192.34'
    {'250.246.192.34', []}

## More complex examples

The [unit tests](https://github.com/marcelog/ex_abnf/blob/master/test/ex_abnf_test.exs)
use different [sample RFCs](https://github.com/marcelog/ex_abnf/tree/master/samples) to test the
[grammar parser](https://github.com/marcelog/ex_abnf/blob/master/lib/grammar.ex) and
[the interpreter](https://github.com/marcelog/ex_abnf/blob/master/lib/interpreter.ex)

## How it works
This is not a parser generator, but an interpreter. It will load up an ABNF grammar,
and generate an (kind of) [AST](http://en.wikipedia.org/wiki/Abstract_syntax_tree) for it. Then
you can apply any of the rules to an input and the interpreter will parse the input according to
the rule.

## Using it with Mix

To use it in your Mix projects, first add it as a dependency:

```elixir
def deps do
  [{:ex_abnf, "~> 0.1.1"}]
end
```
Then run mix deps.get to install it.

## Adding custom code to reduce rules
After a rule, you can add your own code, for example:
```
userinfo      = *( unreserved / pct-encoded / sub-delims / ":" ) !!!
  state = Map.put state, :userinfo, userinfo
  {:ok, state}
!!!
```

Your code will be called with a binding called *state* and another one called like
the rule that matched, and should return either **{:ok, state}** or **{:error, state}**. In
the last case, the full grammar will be aborted and that error will be thrown.

**NOTE**: All rules are lowercased and all dashes are replaced with "_".

## TODO
 * Implement [RFC7405](https://tools.ietf.org/html/rfc7405)

## License
The source code is released under Apache 2 License.

Check [LICENSE](https://github.com/marcelog/ex_abnf/blob/master/LICENSE) file for more information.

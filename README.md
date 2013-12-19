# Minimal code to show parse_trans compilation problem

Check out the repo and run `make` with Erlang R16B03. Here is sample output from my machines
(Ubuntu 12.10 with and OSX 10.9, both with R16B03 ESL erlang packages installed):

    $ erl -version
    Erlang (ASYNC_THREADS) (BEAM) emulator version 5.10.4

    $ make
    ./rebar get-deps
    ==> parse_trans_problem (get-deps)
    Pulling parse_trans from {git,"git://github.com/esl/parse_trans.git",
                                  {tag,"2.6"}}
    Cloning into 'parse_trans'...
    ==> parse_trans (get-deps)
    Pulling edown from {git,"git://github.com/esl/edown.git","HEAD"}
    Cloning into 'edown'...
    ==> edown (get-deps)
    ./rebar compile
    ==> edown (compile)
    Compiled src/edown_make.erl
    Compiled src/edown_xmerl.erl
    Compiled src/edown_layout.erl
    Compiled src/edown_lib.erl
    Compiled src/edown_doclet.erl
    ==> parse_trans (compile)
    Compiled src/parse_trans.erl
    Compiled src/parse_trans_pp.erl
    Compiled src/parse_trans_codegen.erl
    /home/martin/src/parse_trans_problem/deps/parse_trans/src/exprecs.erl:none: internal error in lint_module;
    crash reason: {function_clause,
                      [{erl_internal,bif,
                           [{atom,467,do_transform},{integer,467,2}],
                           [{file,"erl_internal.erl"},{line,248}]},
                       {erl_lint,expr,3,[{file,"erl_lint.erl"},{line,2018}]},
                       {erl_lint,'-expr_list/3-fun-0-',3,
                           [{file,"erl_lint.erl"},{line,2151}]},
                       {lists,foldl,3,[{file,"lists.erl"},{line,1248}]},
                       {erl_lint,expr_list,3,[{file,"erl_lint.erl"},{line,2150}]},
                       {erl_lint,exprs,3,[{file,"erl_lint.erl"},{line,1934}]},
                       {erl_lint,clause,2,[{file,"erl_lint.erl"},{line,1307}]},
                       {erl_lint,'-clauses/2-fun-0-',2,
                           [{file,"erl_lint.erl"},{line,1296}]}]}
    ERROR: compile failed while processing /home/martin/src/parse_trans_problem/deps/parse_trans: rebar_abort
    make: *** [compile] Error 1

BTW, removing `parse_trans/src/exprecs.erl` makes the compilation pass, so only
this specific module seems to cause trouble.

**Update**: The issue was fixed with commit 763462825e4 on Dec 19, 2013.
See https://github.com/uwiger/parse_trans/issues/10

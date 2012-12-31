A tiny language called Z
===========================

A strict, impure, curried, partially applied language with rather
peculiar syntax:

Examples

    defmacro -- _
             "unit"

    -- This is the first macro. A comment macro.

    -- A simple function used in the next macro.
    defun ap x y
          ++ x
             y

    -- A messy macro (because it uses string manipulation),
    -- but demonstrates the idea well enough.
    defmacro when input
             fn blocks
                ap "if"
                   ++ z:indent-before 3
                                      car blocks
                      ++ "\n"
                         ++ z:indent 3
                                     car cdr blocks
                            ++ "\n"
                               z:indent 3
                                        "unit"
                z:blocks input

    -- Demo use of the macro. See how it looks native.
    -- Macros within macros are fine.
    when = 1
           1
         print ++ "The number is: "
                  when true
                       show 123

    -- This is the normal way to use strings.
    print "Hai, guys!"

    -- Here we define a macro to make writing strings easier,
    -- called “:”, it's meant to read like typical English,
    -- and lets you write arbitrary text as long as it's
    -- indented to the offside column.
    defmacro : input
             z:string input

    -- Example with print:
    print : Hello, World!
            What's going on in here?

    -- But also it works just fine as normal syntax within function applications:
    defun message msg
          do print : Here's a message
             print msg
             print : End of message.

    message ap : Hello,
               ++ " World! "
                  : Love ya!

    -- Expect you wouldn't write it like that, you'd just write:
    message : Everybody dance now! Alright?

    -- Map function.
    defun map f xs
          if unit? xs
             unit
             cons f car xs
                  map f
                      cdr xs

    -- ["foo","bar"] → foo\nbar\n
    defun unlines xs
          if unit? xs
             ""
             ++ car xs
                ++ "\n"
                   unlines cdr xs

    -- Simple id, helpful for testing.
    defun id x
          x

    -- Print the blocks of foo and bar with ! on the end.
    print unlines map fn x
                         ++ x
                            "!"
                      z:blocks : foo
                                 bar

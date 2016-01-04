Token [-v]

that prompts for (with the string "% ") and reads bash-like command lines from
the standard input, breaks the lines into tokens, and writes them to the
standard output one per line, followed by their type.  Here a token is

  (1) a maximal, contiguous, nonempty sequence of nonwhitespace characters
      other than the metacharacters <, >, ;, &, |, (, and ) [a SIMPLE token];

  (2) a redirection symbol (<, <<, >, >>, >&, |, |&);

  (3) a command terminator (;, &, &&, or ||);

  (4) a left or right parenthesis (used to group commands).

The types are given in the following table:

  Token   Type      Token   Type      Token   Type      Token   Type
  ~~~~~   ~~~~      ~~~~~   ~~~~      ~~~~~   ~~~~      ~~~~~   ~~~~
  SIMPLE   11       >>       32       ;        51        (       71
  <        21       >&       33       &        52        )       72
  <<       22       |        41       &&       61
  >        31       |&       42       ||       62

Whitespace can be used to separate tokens from one another, but is ignored
otherwise (Warning: whitespace can be escaped; see below).

With the -v option Token writes all characters input to the standard output
before writing the list of tokens.

Example
  The command line

    % < A B | ( C >> D & E )

  (the % is the prompt) is written as

    < : 21
    A : 11
    B : 11
    | : 41
    ( : 71
    C : 21
    >> : 32
    D : 11
    & : 52
    E : 11
    ) : 72

Comments
  If a # would begin a SIMPLE token, it and the remainder of the line
  (including any trailing \s) are ignored; otherwise it is just another
  character.  A leading # can be escaped.

Escape Characters
  The escape character \ removes any special meaning that is associated with
  the following non-null, non-newline character.  This can be used to include
  whitespace (but not newlines), metacharacters, and quotes in a SIMPLE token.
  The \ is NOT removed until a later stage of command processing

  Whenever a final newline is immediately preceded by an unescaped \, Token
  treats the pair as a line splice, discarding both characters, prompting for
  (with the string "> ") and reading from the standard input a continuation
  line, concatenating the two lines, and continuing the tokenization.

Single Quoted Strings
  All characters appearing between matching single quotes (') are treated as
  if they were nonwhitespace, non-metacharacters.  Such strings may not contain
  single quotes, even when preceded by a \, and thus may not be nested.  The
  surrounding quotes are NOT removed until a later stage of command processing.

  Whenever there is no closing quote on the line, Token prompts for (with
  the string "> ") and reads from the standard input a continuation line,
  concatenates the two lines, and continues searching for the closing quote.
  The newline becomes part of the string.

Double Quoted Strings
  All characters appearing between matching double quotes (") are treated as
  if they were nonwhitespace, non-metacharacters.  Such strings may contain
  escaped " and \ characters, but they may not be nested.  The surrounding
  quotes and any \ characters are NOT removed until a later stage of command
  processing.

  Whenever there is no closing quote on the line, Token prompts for (with
  the string "> ") and reads from the standard input a continuation line,
  concatenates the two lines, and continues searching for the closing quote.
  The newline becomes part of the string, unless it is immediately preceded
  by an unescaped \, in which case the pair is treated as a line splice and
  discarded.

Use the submit command to turn in your source and log files as assignment 3.

YOU MUST SUBMIT YOUR FILES (INCLUDING THE LOG FILE) AT THE END OF ANY SESSION
WHERE YOU HAVE SPENT AT LEAST ONE HOUR WRITING OR DEBUGGING CODE, AND AT LEAST
ONCE EVERY HOUR DURING LONGER SESSIONS.  (All submissions are retained.)

Notes:

1. [Matthew and Stone, Chapter 2] contains a somewhat complete description
   of the bash shell, including various other features (e.g., additional
   metacharacters and redirection symbols, wildcards, and variable and command
   substitutions) that Token need not implement.

2. Token is GREEDY, so >>> is tokenized as >> followed by >.

3. If Token encounters any errors, it writes a one-line message to the standard
   error and exits immediately.

4. Note: A line splice can appear within a multicharacter token.

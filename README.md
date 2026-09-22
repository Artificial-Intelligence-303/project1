# Project 1: Search & Games

|                    |                                                    |
| :----------------- | :------------------------------------------------- |
| `Tuesday 22 Sep`   | Released, in lab                                    |
| `Thursday 24 Sep`  | Plan sign-off, in class                             |
| `Tuesday 13 Oct`   | Demo, in lab                                        |
| `Tuesday 20 Oct`   | Due, with code review                               |
| Points              | 10                                                  |
| Format              | Teams of two or three, which you form yourselves    |

Challenges 1 through 3 gave you the problem. This project gives you the
choice: what to build, and which search to build it on.

## What your team builds

**A game of your own, played from the command line, in which a search
algorithm from Weeks 3 to 5 does the thinking.** Your team designs the
game, one of two ways:

- **Invent one.** A game nobody has played yet, with rules your team
  writes.
- **Extend one.** Start from a game that exists and change it: a different
  board, a different way to win, a piece that moves differently, a
  different turn structure, a different goal for a puzzle. At least one
  rule has to be yours, and it has to change what the search sees.

Implementing an existing game as published does not qualify.

Two kinds of game qualify:

- **A two-player game** where the human plays against your agent. Games to
  start from, or to measure an invented game against: tic-tac-toe, Connect
  Four, Nim, a small Othello or checkers variant. Both players see the whole state, there is no randomness, and
  the game ends. The agent searches with minimax, alpha-beta, or a
  depth-limited version of either.
- **A single-player puzzle** where your agent solves it, races the human to
  solve it, or gives hints. Puzzles to start from: the 8-puzzle, Sokoban,
  Rush Hour, a maze, a river crossing, a word ladder. Your agent chooses
  every move, so any search from Weeks 3 and 4 fits: breadth first, depth
  first, iterative deepening, uniform cost, or A*. Depth first finds a
  solution rather than a shortest one, so if your puzzle is about solving
  it in the fewest moves, say in `docs/plan.md` which of your two searches
  guarantees that. A grid game in which an opponent replans a path toward
  the human every turn is this kind too.

Hidden information and dice are out for both kinds.

**What makes a game searchable.** Before you commit to a design, check
that it has every one of these:

- Both players see the whole state, and a move has one result. Nothing is
  hidden and nothing is left to chance.
- Every line of play ends. A game that can go round in circles needs a rule
  that stops it, such as a move limit.
- Every turn has a real choice. If the right move is obvious at every
  turn, there is nothing to search.
- The state is small enough to write down as a tuple, and you can say
  roughly how many states there are. If the full tree will not finish
  from the starting position, say so, and plan a depth limit and an
  evaluation function.
- You can play a whole game of it by hand, on paper. Play it twice before
  you write any code: if the first player always wins in three moves,
  change a rule.

**Four things are required, whichever kind you pick:**

1. **A justified choice of algorithm.** `docs/plan.md` states which search
   your agent uses and why that search fits your game: who moves, what a
   state is, and whether the full tree can be searched or needs a depth
   limit and a heuristic.
2. **An interface between the game and the search.** Your search functions
   are written against a small set of methods your game provides (the
   `Problem` shape from Challenges 2 and 3, or the game equivalent with
   `to_move`, `is_terminal` and `utility`).
3. **Two search algorithms, against that same interface**, and a
   **measurement**: run both from the same, non-trivial state and report
   how many nodes each visited. Minimax against alpha-beta, breadth first
   against iterative deepening, breadth first against A*, or A* under two
   heuristics all qualify. The number comes from running your code.
4. **A human can play from the command line.** No graphical interface is
   required. If your terminal can print the state and read a move, that is
   enough.

There is no starter code in this repository. It holds this README,
`docs/plan.md` and `docs/reflection.md` to fill in, the automated checks,
and the three issue forms the instructor fills in: the plan sign-off, the
demo, and the code review.

## A worked example to read

<https://github.com/Artificial-Intelligence-303/project1-reference-example>
is a finished version of the structure this project asks for: `src/game.py`
is the interface, `src/search.py` is minimax and alpha-beta written against
that interface and nothing else, `src/nim.py` is one game that satisfies
it, and `uv run python src/main.py` prints the node count for each search.

Read it for the shape rather than for the game. Nim is a single pile of
objects, which has none of the design decisions your game needs, and it is
not your team's own, so it does not qualify as a submission.

## Layout, and the automated checks

The checks in `gatorgrade.yml` assume this layout. Run them yourself with
`uv run gatorgrade --config gatorgrade.yml`; they also run on GitHub when
you push.

- **`src/main.py`** starts the game. Run without arguments, it lets a human
  play from the terminal. Run as `uv run python src/main.py --measure`, it
  runs both of your search algorithms from the same state and prints the
  node count for each.
- **`src/`** holds the rest of your code: the game, its interface, and the
  two searches.
- **`tests/`** holds at least one `test_*.py`, and `uv run pytest` passes.
  The test worth writing first: both searches return the same move and
  the same value on a state you worked by hand.
- **`docs/plan.md`** and **`docs/reflection.md`** have no `TODO` left in
  them and at least 300 words each; the reflection's measurement section
  reports a number of nodes.
- The code follows the Google Python style guide, checked by
  `uv run ruff check src tests`.
- The repository has at least 20 commits by the due date.

The checks confirm the pieces are there. They do not judge the game, the
design, or the code; the demo, the code review and the reflection do.

## How to fill in `plan.md`

1. **Brainstorm two or three ideas for a game of your own**, invented or
   extended. For each: the game you started from, if any, and the change
   that makes it yours, or the core idea if it is new; the initial state;
   the legal actions; what the state looks like after the first couple of
   turns.
2. **Pick one**, and say why: how big is the state space, as a number, and
   will the full search finish in a reasonable time, or will you need a
   depth limit and a heuristic?
3. **Name your algorithm and defend it.** Which search, and what about your
   game makes it the right one. Name the second algorithm you will measure
   it against.
4. **Three goals**, each specific enough to check later: one about how a
   human plays your game, one about the search itself (this is where the
   measurement lives), and one open goal your team sets.
5. **Pseudocode** for `result` and for whichever of your game's remaining
   methods is least obvious.

## Depth limits, if your game needs one

Not every game's full tree finishes searching in a reasonable time.
Tic-tac-toe's does. Connect Four's does not, not from an empty board. If
your tree is too large, search to a fixed depth and use a heuristic
evaluation function in place of `utility` at the cutoff, the idea Challenge
3 asked you to reason about, applied to a value estimate rather than a
distance estimate. Say in `plan.md` whether your game needs this.

## What is due, and when

- **Thursday 24 September, in class: the plan sign-off.** Bring your draft
  of `docs/plan.md`. The instructor works through it with your team at your
  table, and will either sign off or indicate what to fix before you write
  code. The **Plan sign-off** issue is filled in for you.
- **Tuesday 13 October, in lab: a demo.** Demonstrate a current working
  version of your game. The **Demo** issue is filled in for you following
  your demonstration. This is a checkpoint, not the final grade.
- **Tuesday 20 October: the finished project**, with a full code review
  and `docs/reflection.md` completed. The instructor/TL records the review in
  the **Code review** issue.

## Evaluation

This project is worth **10 points**.

| Component            | Value |
| :-------------------- | ----: |
| Working implementation | 5 |
| Demo | 1 |
| Written reflection | 2 |
| Oral component of the code review | 2 |

**The implementation** is graded on whether the interface and both search
algorithms are present and correct, whether the algorithm choice is
justified and fits the game, whether your own measurement compares the two
searches on the same state, and whether a human can actually play from the
terminal. The game being your team's own is checked at the plan sign-off,
not here: a team that has a signed-off plan has a qualifying game.

**The reflection** revisits each goal from `plan.md` and says honestly
whether it was met.

**The oral component** is individual, during the code review: each team
member has to be able to explain the part of the code they are asked
about.

## How this project was built

I drafted the project description and requirements, and the grading criteria. Claude drafted the issue forms, automated checks, and an example implementation. I reviewed the result and verified implementation before release.

## AI use on this project

Full policy: **[AI in this course](https://areweagentsyet.com/ai/)**. AI
tools are allowed here, you disclose them, and every member has to be able
to explain any part of the code in the review.

**Writing your tests with an AI tool is good practice here.** Give it your
interface and your rules, ask for cases, and check each case against your
own rules before you keep it.

The specific hazard on a project like this: a tool asked for "minimax for
tic-tac-toe" or "A* for the 8-puzzle" hands back a complete implementation
written against that one game rather than against your interface. That
answers a smaller question than the one this project asks. Ask for the
interface first, then for the search against it, and you end up with
something you can defend when the review asks why your two searches agree
on your game.

## Disclosing your AI use

Each team member answers, in `docs/reflection.md`: which tool, what you
used it for, and what you did with what it gave you. "None" is a complete
answer if it is true.

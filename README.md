# tiny-chess-01: a chess engine in 4 KB of Python

An original alpha-beta chess engine for make.fun's tiny-chess-01 bounty,
packed into a 4,073-byte `main.py` (the readable source is compressed
with the kit's `makefun compress`; the entrypoint unpacks and runs it).

Design:

- 120-square mailbox board as a string, absolute orientation; incremental
  material + piece-square evaluation from White's perspective, with a
  tapered king table (middlegame/endgame blend by phase) and a bishop-pair
  bonus.
- Negamax alpha-beta with PVS, iterative deepening under a fixed node
  budget (80,000 nodes a move, tapering in very long games — never a
  clock, so trials replay move for move), aspiration windows, null-move
  pruning, late-move reductions, futility pruning and check extensions.
- Quiescence search over captures, promotions and check evasions.
- Transposition table, killer moves and history heuristic for ordering;
  pseudo-legal move generation with king-capture legality detection.
- Repetition and fifty-move awareness, including the game's own history.

It plays through the competition env (`MAKEFUN_ENV_URL`): POST /game,
then POST /move with UCI moves until the env reports the trial done.

Local practice results (kit trial 0 and 1, 6 games each): Elo 1654 on
both, five wins and one loss per trial.

MIT licensed. Built by an AI agent for @isyanasarasvati.

# 🎲 Dice Blackjack 💣

## Rules

Dice Blackjack is a 2 player game, based on blackjack (aim to achieve 21 and not bust) but rolling dice instead of drawing cards.

The game is structured in matches, rounds, and turns.

A match is a complete session of the game, where the player who first gets to 10 points win. At the beginning of a match, each player choose which die type they will use, from d4, d6, d8, d10, d12, or d20.

A round starts with both players at a sum of 0, and they take simultaneous turns rolling dice and adding up values aiming to achieve 21. If a player goes over 21, they busted. A round can have up to 7 turns and at the end of the round the player with higher score wins, if not busted. If a player achieves 21 or both player bust, the round ends in that turn. The winning player gets 2 points if they achieved 21, or 1 point if other value. If there is a tie (both players achieve 21 in the same turn, both players bust, or both players have the same score at the end of the round), no players get points.

A turn is composed of the following steps:
1. Each player choose if they will Roll or Stand, simulataneously and without knowledge of the other player's decision. If both players chose Stand, the round ends in this turn. A busted player always Stand. If one player is busted and the other not, the non-busted player can choose to Stand or Roll. The choice to Stand applies only to that turn (a player potentially can Stand one turn and Roll the next).
2. Dice are rolled, if any
3. Sums are computed and win conditions evaluated.

## Implementation

The game will run as a webpage hosted on GitHub Pages, to be accessed on mobile. It should be composed of basic html, css, and JavaScript, avoiding external dependencies unless very helpful and using well stablished and reliable libraries. The code should be clear and robust, with plenty of comments, documentation, and console logs. The code should handle errors by stopping execution and giving a useful message, and not by default fallbacks (for example, if the engine receives an illegal action from a player, it should stop execution, not default to a random legal action). 

The game will be implemented to be played with one human player (Player 1) against one AI player (Player 2). The AI will follow one of a series of strategies, described below. Although the game rules stipulate that both players chose their action simultaneously, internally in the implementation Player 1 will chose their action, and if Roll, the result of the dice will be obtained but hidden from the user, and then Player 2 will chose their action with knowledge of Player 1's action and roll result. This allows some AI strategies to cheat and chose their action based on Player 1 (see strategies below). However, the UI presented to the user (Player 1) should preserve the appearance that both players chose simultaneously.

The implementation will also include statistical analyzes of the game state, described in detail below. These analyzes can optionally be displayed to the user and are used for the AI strategies.

## UI

The initial screen allows the user to select their dice type, the AI dice type, and the AI strategy, with options to choose “random” on each of them (the default pre-selected option). Also there is a button to “Start Match”, leading to the main game screen. 

The main game screen should indicate the dice type of each player, the match score, the current sum, interactive content to allow the user to chose their action and see the opponent action, roll results, and a button to show/hide the analyzes

## Analyzes

Analyzes are done at the perspective of a single round, assuming the round will end at that turn. The analyses module should compute all the permutations of actions and rolls for that game state and aggregate the results appropriately to compute the stats bellow. For EV computations, assume wins value are equal to points gained (2 if 21, or 1), ties are value zero, and losses are valued negative the ammount of points the opponent gained. 

The analysis should contain:

- Probability P1 bust
- Probability P2 bust
- 2x2 Result table according to P1 and P2 actions, with each "cell" of the table describing the probabilities of P1 win, Tie, P2 win
- 2x2 Payoff table, with positive EV indicating P1 winning points and negative EV indicating P2 winning points
- The Minimax action for P1 and P2. If there is a tie in the minimax action, use the mean EV of each action to decide, if there is still a tie, describe the minimax action as "indifferent". If the state is a mixed equilibrium (no saddle point), calculate the probabilty of rolling for P1 and and P2 that corresponds to that mixed equilibrium.

## AI Strategies

- **21 or bust:** rolls if sum < 21
- **Won't stay behind:** roll if their sum is less than the opponent sum, and the opponent is not busted
- **Low Risk Taker:** rolls if P(bust) < 0.25, or if P(bust) = 0, if the opponent is bust
- **Moderate Risk Taker:** rolls if P(bust) < 0.50, except when the oponent is busted, then rolls if P(bust) = 0 or if the individual EV is the same rolling or standing
- **High Risk Taker:** rolls if P(bust) < 0.75
- **Relative Risk Taker:** rolls if their P(bust) is less than the opponent P(bust) and the opponent is not busted
- **Game Theorist:** Plays the Minimax strategy. If indifferent, rolls if P(bust) = 0. If mixed, roll randomly at the computed probability.
- **Mind Reader:** Is a cheater AI. Checks if the Player 1 rolled or not, but don't check the result of the roll. Takes the optimal action based on the Player 1 action.
- **Future Seer:** Is a cheater AI. Checks the result of Player 1 roll and takes the optimal action based on the Player 1 roll.














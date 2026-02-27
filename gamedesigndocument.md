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





# Gemini Revision, to compare and incorporate


🎲 Dice Blackjack 💣
Rules
Dice Blackjack is a 2-player game, based on blackjack (aim to achieve 21 and not bust) but rolling dice instead of drawing cards.
The game is structured in matches, rounds, and turns.
A match is a complete session of the game, where the player who first gets to 10 points wins. At the beginning of a match, each player chooses which die type they will use, from d4, d6, d8, d10, d12, or d20.
A round starts with both players at a sum of 0, and they take simultaneous turns rolling dice and adding up values aiming to achieve exactly 21. If a player goes over 21, they bust. A round can have a maximum of 7 turns.
* At the end of the round, the player with the higher score wins (provided they have not busted).
* If a player achieves exactly 21, or both players bust, the round ends immediately in that turn.
* Scoring: The winning player gets 2 points if they achieved exactly 21, or 1 point for any other winning value.
* Ties: If there is a tie (both players achieve 21 in the same turn, both players bust, or both players have the exact same score at the end of the round), neither player gets any points.
A turn is composed of the following steps:
1. Decision Phase: Each player chooses if they will Roll or Stand, simultaneously and without knowledge of the other player's decision.
    * If both players choose Stand, the round ends in this turn.
    * A busted player must always Stand.
    * If one player is busted and the other is not, the non-busted player can choose to Stand (ending the round) or Roll.
    * Crucial Rule: The choice to Stand applies only to that turn. A player can potentially Stand one turn and choose to Roll the next (e.g., waiting to see what the opponent does).
2. Roll Phase: Dice are rolled for any player who chose to Roll.
3. Evaluation Phase: Sums are computed and win/end-of-round conditions are evaluated.
Implementation Architecture & Edge Cases
The game will run as a webpage hosted on GitHub Pages, designed to be accessed on mobile devices. It should be composed of vanilla HTML, CSS, and JavaScript, completely avoiding external dependencies.
* Code Quality: The code must be clear, robust, heavily commented, and include detailed console logs.
* Error Handling: Errors must be handled by stopping execution and rendering a useful error message to the UI. Do not use default fallbacks for illegal states (e.g., if the engine receives an illegal action, stop execution immediately rather than defaulting to a random legal action).
* Simultaneous Turn Illusion: The game is played with one human (Player 1) vs one AI (Player 2). While rules stipulate simultaneous decisions, the internal JS engine will operate sequentially to allow for cheater AIs: Player 1 inputs their action -> if Roll, the die is rolled under the hood (hidden from UI) -> Player 2 selects an action with knowledge of P1's action/result (if their strategy permits) -> Both actions and results are revealed to the UI simultaneously.
UI Requirements
* Initial Screen: Allows the user to select their dice type, the AI's dice type, and the AI's strategy. All three dropdowns should include a "Random" option (which is pre-selected by default). A "Start Match" button transitions to the main game screen.
* Main Game Screen: Displays the dice type of each player, the overall match score, current round sums, and turn count. It requires interactive buttons for the user to choose their action (Roll/Stand), an area to reveal the opponent's action and both roll results, and a toggle button to "Show/Hide Statistical Analyzes".
Analyzes Module
Analyzes are calculated from the perspective of a single turn, using a depth-1 lookahead (assuming the round will definitively end at the conclusion of the current turn). The module computes all permutations of actions and rolls for the current game state to output the following statistics.
For EV computations: Wins = +1 (or +2 if exactly 21). Ties = 0. Losses = negative the amount of points the opponent would gain (-1 or -2).
The analysis panel must display:
1. Probability P1 bust
2. Probability P2 bust
3. 2x2 Result Table: A matrix based on P1 (Roll/Stand) and P2 (Roll/Stand) actions. Each cell describes the exact probability of: P1 Win %, Tie %, P2 Win %.
4. 2x2 Payoff Table (EV): A matrix showing the Expected Value in points. Positive EV indicates P1 favors the outcome; negative EV indicates P2 favors the outcome.
5. Minimax Action: The calculated Minimax action for P1 and P2 based on the EV table.
    * If there is a tie in Minimax EV, use the mean EV of each action to decide.
    * If there is still a tie, display the action as "Indifferent".
    * If the state is a mixed equilibrium (no pure saddle point), calculate and display the probability of rolling for P1 and P2 that corresponds to that mixed equilibrium.
AI Strategies (Player 2)
If a strategy's condition is not met, the AI defaults to Stand.
* 21 or bust: Rolls if sum < 21. Otherwise, Stands.
* Won't stay behind: Rolls if their sum is strictly less than the opponent's sum AND the opponent is not busted. Otherwise, Stands.
* Low Risk Taker: Rolls if P(bust) < 0.25. If the opponent is busted, it only rolls if P(bust) = 0. Otherwise, Stands.
* Moderate Risk Taker: Rolls if P(bust) < 0.50. Exception: if the opponent is busted, it only rolls if P(bust) = 0 OR if the individual EV is exactly the same whether rolling or standing. Otherwise, Stands.
* High Risk Taker: Rolls if P(bust) < 0.75. Otherwise, Stands.
* Relative Risk Taker: Rolls if their P(bust) is strictly less than the opponent's P(bust) AND the opponent is not busted. Otherwise, Stands.
* Game Theorist: Plays the Minimax strategy calculated in the Analyzes module. If Minimax is "Indifferent", it rolls only if P(bust) = 0. If it's a mixed equilibrium, it rolls randomly based on the computed equilibrium probability.
* Mind Reader (Cheater): Checks if Player 1 chose to Roll or Stand (but does not see the dice result). Given Player 1's locked decision, the AI calculates the optimal EV between its own Roll/Stand options and takes the action with the highest EV.
* Future Seer (Cheater): Checks Player 1's exact action AND the outcome of their roll. Given this exact final state for Player 1, the AI calculates the optimal EV between its own Roll/Stand options and takes the action with the highest EV.













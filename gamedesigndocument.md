# 🎲 Dice Blackjack 💣

## Rules

Dice Blackjack is a 2 player game, based on blackjack (aim to achieve 21 and not bust) but rolling dice instead of drawing cards.

The game is structured in matches, rounds, and turns.

A match is a complete session of the game, where the player who first gets to 10 points win. At the beginning of a match, each player choose which die type they will use, from d4, d6, d8, d10, d12, or d20.

A round starts with both players at a sum of 0, and they take simultaneous turns rolling dice and adding up values aiming to achieve 21. If a player goes over 21, they busted. A round can have up to 7 turns and at the end of the round the player with higher score wins, if not busted. If a player achieves 21 or both player bust, the round ends in that turn. The winning player gets 2 points if they achieved 21, or 1 point if other value. If there is a tie (both players achieve 21 in the same turn, both players bust, or both players have the same score at the end of the round), no players get points.

A turn is composed of the following steps:
1. Each player choose if they will Roll or Stand, simulataneously and without knowledge of the other player's decision. If both players chose Stand, the round ends in this turn. A busted player always Stand. If one player is busted and the other not, the non-busted player can choose to Stand or Roll.
2. Dice are rolled, if any
3. Sums are computed and win conditions evaluated.

## Code Architecture

The game will run as a webpage hosted on GitHub Pages, to be accessed on mobile. It should be composed of basic html, css, and JavaScript, avoiding external dependencies unless very helpful and using well stablished and reliable libraries. The code should be clear and robust, with plenty of comments, documentation, and console logs. The code should handle errors by stopping execution and giving a useful message, and not by default fallbacks (for example, if the engine receives an illegal action from a player, it should stop execution, not default to a random legal action). Initially, the game will be implemented to be played with one human player (Player1) and one AI player, but there are plans to run simulations of AI vs AI in the future, so the code architecture is planned to facilitate these future developments.

The code architecture should contain the following modules: engine, analysis, strategies, ui, controller. Other modules may be created if needed.

The engine module is the only responsible for creating game states and processing actions. It has a method “initiateMatch”, which takes as arguments the die type selection for Player1 and Player2, and returns a GameState. It also has a method “processAction”, which takes a GameState and Action, and returns a new GameState. The engine module has no internal memory, and may return errors if the GameState or Action are invalid.

A GameState is a immutable serializable object capable of fully describe any possible game state. It has the method “toString” which returns well formatted ASCII with all the GameState information.

Action is a object which describes an action and optionally the player taking that action. The player is required when the action is "Stand" or "Roll", but not when the action is "Proceed", which is given by the controller.

The analysis module computes a series of statistics about the game state. It has a method “analyze” which takes a GameState and returns an Analysis object.

The Analysis object is an immutable serializable object with several statistics, described in detail in a following section. It has the method “toString” which returns well formatted ASCII with all the Analysis information.

The strategies module contains several strategies an AI player may use, described in detail in a following section. An Strategy object has a method “chooseAction” which takes a GameState and a Analysis object and returns a Action object. The Strategy object may have internal memory, to “learn” the opponent behaviour along a match (there is no between match persistence). The Strategy object may return errors if the GameState or Analysis are not valid.

The UI module controls the website UI presenting information to the human player and collecting their action choices. Details on the UI are given in a following section. The UI changes according to the GameState and may show information on the Analysis.

The controller module handles communication and flow among the other modules. The other modules do not communicate directly between them.

A summary of the game code flow is:
1. The user chooses the die for each player and the AI strategy. The ui communicates this choice to the controller. The controller calls the engine to initiate a match.
2. The engine returns a GameState to the controller. The controller obtains an analysis of the GameState. The controller ask the ui to move to the main game view and update according to the new GameState.
3. Player 1 choses their action in the ui. The ui comunicates to the controller. The controller communicates to the engine. The engine rolls the die for player 1, if needed, and returns a GameState with the Player 1 processed.
4. The controller communicates the GameState to the AI strategy. The Strategy choses an action. The controller communicates the action to the engine. The engine processes the Player 2 action, rolls die if needed, and returns a GameState with the end of turn state (info of both rolls, sum of scores, win conditions)
5. The controller asks the ui to update with the new GameState. The ui has interactive elements described in detail in a following section, to reveal the results progressively to the player. When the player selects to proceed to the next turn, the ui communicates to the controller, the controller tells the engine to proceed.
6. Steps 2 - 5 (a turn) are repeated until the end of match.

8. The player selects to proceed to the next turn (if the match did not end). The ui communicates to the controller. The controller communicates to the engine with a "Proceed" action. The engine returns a new GameState (start of next turn). The controller obtains an analysis of the GameState.



This game code flow was designed to allow the Player 2 strategry to possibly cheat (don't follow the game rules) and choose its action based on the action and roll of Player 1 (some AI strategies were intentionally designed to cheat), and to present the end of turn state to the user before proceeding to the next turn (even if there is no action to be taken).

## UI

## Analyzes

Analyzes are done at the perspective of a single round, assuming the round will end at that turn. The analyses module should compute all the permutations of actions and rolls for that game state and aggregate the results appropriately to compute the stats bellow. For EV computations, assume wins value are equal to points gained, ties are value zero, and losses are valued negative the ammount of points the opponent gained. 


The analysis object should contain:

- Probability P1 bust
- Probability P2 bust
- 2x2 Result table according to P1 and P2 actions, with each "cell" of the table describing the probabilities of P1 win, Tie, P2 win
- 2x2 Payoff table, with positive EV indicating P1 winning points and negative E2 indicating P2 winning points
- The Minimax move for P1 and P2
- If a mixed equilibrium, the probabilitynof rolling for P1 and P2


## Strategies

- **21 or bust:** rolls if sum < 21
- **Won't stay behind:** roll if their sum is less than the opponent sum, and the opponent is not busted
- **Low Risk Taker:** rolls if P(bust) < 0.25, or if P(bust) = 0, if the opponent is bust
- **Moderate Risk Taker:** rolls if P(bust) < 0.50, except when the oponent is busted, then rolls if P(bust) = 0 or if the individual EV is the same rolling or standing
- **High Risk Taker:** rolls if P(bust) < 0.75
- **Relative Risk Taker:** rolls if their P(bust) is less than the opponent P(bust) and the opponent is not busted
- **Game Theorist:** Plays the Minimax strategy. If indifferent, rolls if P(bust) = 0. If mixed roll at the defined probability.
- **Mind Reader:** Can only be selected for Player 2. Is a cheater AI. Checks if the Player 1 rolled or not, but don't check the result of the roll. Takes the optimal action based on the Player 1 action.
- **Future Seer:** Can only be selected for Player 2. Is a cheater AI. Checks the result of Player 1 roll and takes the optimal action based on the Player 1 roll.














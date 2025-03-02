# Computerised Mao 
An attempt to allow for Mao to be played virtually, with the end goal of creating a system that allows for the simulation of most playing card based games.
**Note:** This is not an attempt to reanimate the chairman in robotic for unfortunately


## Base Rules:
- The game is won by the first player who empties the cards in their hand.
- The winner of the round, adds a rule to the game that is in play for the rest of the game. New Rules may interact with new rules.
- A player has 1 action, they can choose to draw 1 from the deck, or they can play a card.
- When a player breaks a rule they draw a card, and take back the invalid play (if applicable).
- At base the set of invalid plays are possible:
  - Playing a card that does not match the suit **OR** does not match the number of the cards below it.


## Game setup.
1. The dealer deals each player 7 cards (including themselves).
2. The dealer moves the top card of the deck into the "play zone" and then turns it face up. 
3. The dealer then recites "Mao is the name of the game, the name of the game is Mao, Mao is the name of the game. The game has now begun!".

If the dealer makes a mistake in this process, they are given 1 additional card for "setting the game up incorrectly". And then the game continues as normal, with the player to the left of the dealer playing first.


## Terminology
Since the nature of this game means that we have to generalize to almost any possible playing card game. Because of this we must establish a few concepts that will be helpful in terms of all games.

### Player
A is a collection of attributes associated with each person in the game. For example each player is likely to have a hand. And one player may be designated the dealer.

### Phases
A phase is a section of a game with a potentially unique set of Triggers, Actions, and Events. The most common example of would be a "beginning/setup" phase. In which usually the cards are dealt out.

### Action
An action that the player can take. An action is a tuple of the function that happens when the action is triggered, and a predicate saying if the action can be taken. A player can take any number of valid actions during (potentially not even during their turn!)

### Trigger
A thing that triggers in response to something in the game happening. These are only applicable in the Phase they're defined in.

### Event
A series of steps that get followed in order to progress the game/and or setup the phase. - Could be considered sub-phases.

### Counters
A mutable counter that keeps track of the game-state throughout time. Updates when a specific trigger is called.

### "Your turn"
A common boolean counter that tells you if it is "Your turn".




## Example
As an example of how this system would work, I will attempt to lay out how blackjack would be created in this system.
### Phase 1: Setup
#### Triggers
1. When the dealer is dealt their second card it is turned "face up".
### Actions:
None
#### Events
1. The deck is shuffled.
2. A random Player is designated "The Dealer".
3. Each Player draws two cards (technically 1 card twice).


### Phase 2: Gameplay
#### Actions:
1. ("*Hit*: Draw 1 card.", "Activate on your turn."),
2. ("*Stand*: End the turn", "Activate on your turn.")
#### Triggers
1. When "The dealer"'s turn starts move to Phase 3
#### Events
1. It becomes "the turn" of the player to "the left" of The Dealer

### Phase 3: Ending Phase
#### Triggers
1. When The Dealer "reveals" their hand, if the sum of all their cards <= 16 they draw one card and reveal it.
2. When The Dealer draws a card, if the sum of all their cards <= 16 they draw one card and reveal it.
**Note:** Effects of a trigger can trigger the original trigger!
3. When a player reveals a card, if the sum of their hand is > 21, they get a "Bust" Counter
#### Events
1. All Players reveal their hands.
2. The Player(s) with the highest total of cards in their hand **without** a Bust Counter win the game.

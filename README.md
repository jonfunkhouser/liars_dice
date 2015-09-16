###Liar's Dice

In the game Liar's Dice, each player begins the game with 5 dice, hidden from the other players.  Every player rolls the dice and hides the result. The first player puts X(ex. 0 to 5) number of dice in the middle and declares how many dice of a certain number exist amongst all the dice in play. For example, if he puts out 3 4s and says "There are at least 10 4s", then he believes there are 10 4's across everyone's hand including the 3 dice in the middle.

After a player places dice on the board and makes a bid(note: the player doesn't have to place any dice on the board, they can just make a bid), he/she must reroll the remaining dice in their hand.

The next player must either challenge the previous player's claim, or make a new claim that is at least one die higher (it can be an different number is the player chooses). For example, "There are at least 11 4s" or "There are at least 11 3s".

###The Exercise

Write a class that represents a Liar's Dice game of X Players. This class does not have to mimic the game completely(ie it doesn't need to interactive), but must implement three public methods: move, claim, and challenge.

* _move_: I should be able to tell the game that player X put Y dice showing the number Z into the middle.
* _claim_: I should be able to ask the game the probability that a claim of X number of Y's is correct at any point.
* _challenge_: I should be able to challenge a claim and the game should tell me if I am correct.

An example execution might be:

```
game = Game.new(players: 4)
game.move(player: 1, dice: 2, value: 3)
game.move(player: 2, dice: 1, value: 3)
game.claim(dice: 19, value: 3) An outrageous claim! -> 0.000000000516%
game.challenge(dice: 19, value: 3) => false
```

This formula can be used to determine the probability of k dice of the same value given n total dice:

```
n! / k! (n - k)! * (1/6)^k * (5/6)^(n-k)
```
However, it only tells us the probability of the exact number of dice, not "at least this number". So we have to sum all the possibilites together.

Example: There are 17 dice left, and we want to calculate the odds of at least 16.
Thus we calculate the probabilty of getting exactly 16 and add that to the probability that all 17 are the same.

```
17! / 16!(1!) * (1/6)^16 * (5/6)^1 = 0.0000000000051
17! / 17!(0!) * (1/6)^17 * (5/6)^0 = 0.000000000000059
```

(0.0000000000051 + 0.000000000000059) * 100 = 0.000000000516% or pretty much never.

Now if we do this:

```
game.move(player: 3, dice: 5, value: 3)
game.move(player: 4, dice: 5, value: 3)
game.move(player: 1, dice: 3, value: 3)
game.move(player: 2, dice: 3, value: 3)
```

There are 19 3's on the board. (Assuming all the .move calls in this document)

```
game.claim(dice: 20, value: 3)
```

Since there is only 1 die left n = 1, k = 1, thus:

```
1! / 1!0! * 1/6^1 * (5/6)^0 = 16.67%
```

# Blackjack

A simple implementation of the popular card game using core Python functions. Games are excellent exercises for breaking down problems and thinking logical. This particular game requires keeping track of the possible decisions by the players based on the revealed cards.

Because this game was developed with educational purposes in mind, the score of the game between the card dealer and the player simply consists of keeping track of winning and losing hands.

There are multiple versions of this and similar games online. The code below represents my interpretation of the game and logical flow of the code. 




```python
import random

clubs_symbol = '\u2663'
spades_symbol = '\u2660'
hearts_symbol = '\u2665'
diamonds_symbol = '\u2666'

decks = 10
cards = ['A','2','3','4','5','6','7','8','9','J','Q','K']

deck = []


for i in range(decks):
    spades = []
    clubs = []
    hearts = []
    diamonds = []
    for card in cards:
        deck.append(spades_symbol + card)
        deck.append(clubs_symbol + card)
        deck.append(hearts_symbol + card)
        deck.append(diamonds_symbol + card)
    
deck = random.sample(deck, k=len(deck))

```


```python
# Start game
player_wins = 0
dealer_wins = 0

while True:
    player_count = 0
    player_busted = False
    dealer_count = 0
    
    while True:
        
        # Get card from deck
        current_card = deck[0]
        del(deck[0])
        
        # Player count
        if current_card[1] in ['J','Q','K']:
            player_count += 10
        elif current_card[1] == 'A' and player_count + 11 <= 21:
            player_count += 11
        elif current_card[1] == 'A' and player_count + 11 > 21:
            player_count += 1
        else:
            player_count += int(current_card[1])

        print('Card:',current_card,' Player count:',player_count)

        if player_count == 21:
            print('Blackjack!')
            break

        elif player_count < 21:
            user_decision = input('Hit or Stay (h/s)?').lower()
            if user_decision == 's':
                break
        else:
            player_count = 0
            player_busted = True
            print('Player busted!')
            break

    while True:
       
        if player_busted:
            dealer_count = 1 # Barely over 0 to win the hand. No cards involved.
            break
            
        # Get card from deck
        current_card = deck[0]
        del(deck[0])
        
        # Dealer count
        if current_card[1] in ['J','Q','K']:
            dealer_count += 10
        elif current_card[1] == 'A' and dealer_count + 11 <= 21:
            dealer_count += 11
        elif current_card[1] == 'A' and dealer_count + 11 > 21:
            dealer_count += 1
        else:
            dealer_count += int(current_card[1])

        print('Card:',current_card,' Dealer count:',dealer_count)
        
        if dealer_count > 17 and dealer_count <= 21 and dealer_count >= player_count:
            break
        elif dealer_count > 21:
            dealer_count = 0
            print('Dealer busted!')
            break
        
    if dealer_count > player_count:
        dealer_wins += 1
        print('Dealer wins!')

    elif dealer_count < player_count:
        player_wins += 1
        print('Player wins!')
    
    else:
        print('Draw, scores remain unchanged.')
    
    print('--------------------------------------')
    print('Dealer score:',dealer_wins,'  -  ','Player score:',player_wins)
    print('--------------------------------------')
```

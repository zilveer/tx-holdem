
# Texas Holdem Poker
This is the module for creating own Texas Holdem poker game! It allows you to track cards, compose hands, compare hands by combination and even calculate draw combinations.
# Usage

    const { Pack, Hand } = require('tx-holdem');

	const pack = new Pack();
	
	const pairHand = new Hand(
		pack.createCard('clubs', 3),
		pack.createCard('diamonds', 3)
	);
	const fourOfAKindHand = new Hand(
		pack.createCard('clubs', 4),
		pack.createCard('diamonds', 4),
		pack.createCard('hearts', 4),
		pack.createCard('spades', 4),
	);

	const pairIsLower = pairHand.compare(fourOfAKindHand) === -1;
	console.log('Pair is lower than four of a kind:', pairIsLower);
# API
## Card
- constructor(suit, value)
	- suit: Number
	- value: Number

Methods:
- static create(suit, value): Card | null — creates Card instance, returns null is suit or value is not a number
	- suit: Number
	- value: Number
- toString(): String — returns string representation of card
- toJSON(): object — returns JSON representation of card
- valueOf(): Number — returns value representation of card (card.value property)
- compare(card): Number — compares current card with given by value, returns 1 if current is higher; -1 if current is lower; 0 if equal
	- card: Card
- equalBySuit(card): Boolean — compares cards by suit
	- card: Card
- equalByValue(card): Boolean — compares cards by value
	- card: Card
- isAce(): Boolean — returns true if card is ace

Properties:
- suit: Number
- rank: Number
- static readonly CLUBS: Number
- static readonly DIAMONDS: Number
- static readonly HEARTS: Number
- static readonly SPADES: Number
- static readonly SUIT_MAX: Number — constant value of max suit (spades)
- static readonly TWO: Number
- static readonly THREE: Number
- static readonly FOUR: Number
- static readonly FIVE: Number
- static readonly SIX: Number
- static readonly SEVEN: Number
- static readonly EIGHT: Number
- static readonly NINE: Number
- static readonly TEN: Number
- static readonly JACK: Number
- static readonly QUEEN: Number
- static readonly KING: Number
- static readonly ACE: Number
- static readonly VALUE_MAX: Number — constant value of max rank (ace)

## Hand
- constructor(...cards)
	- ...cards: Card — enumeration or array of cards

Methods:
- addCards(...cards): Boolean — adds multiple cards to hand
	- ...cards: Card — enumeration or array of cards
- addCard(card): Boolean — adds single card to hand
	- card: Card
- isFull(): Boolean — returns true if had has reached maximum capacity
- has(card): Boolean — returns true if had as given card
	- card: Card
- has(suit, value): Boolean — returns true if hand has card with given suit and value
	- suit: Number
	- value: Number
- compare(hand): Number — returns -1 if current had has lower combination, 0 if hands are equal, 1 if hand is greater
	- hand: Hand
- isKicker(): Boolean
- isPair(): Boolean
- isTwoPairs(): Boolean
- isThreeOfKind(): Boolean
- isStraight(): Boolean
- isFlush(): Boolean
- isFullHouse(): Boolean
- isFourOfKind(): Boolean
- isStraightFlush(): Boolean
- isRoyalFlush(): Boolean 
- reduce(aggregate, start): any — applies aggregate function to each card in hand and returns single value; works in the same way as Array.reduce
	- aggregate: Function
	- start: any
- sort(order): undefined — sorts cards in hand by given order
	- order: String — "asc" by default
- every(predicate): Boolean — returns true if every card matches given predicate
	- predicate: Function
- forEach(aggregate): undefined — applies aggregate function to each card in hand

Properties:
- readonly combination: Combination
- readonly drawCombination: DrawCombination
- readonly size: Number
- static readonly MAX_HAND_SIZE: Number

## Pack
- constructor()

Methods:
- destroy(): undefined — removes all cards from pack
- createCards(count) — creates given number of random cards
	- count: Number
- createCard(suit, value): Card | null — creates card with given suit and value, in case one of arguments is not specified random card will be generated; returns null in case card with given suit and value already exists
	- suit: Number
	- value: Number
- has(card): Boolean — returns true if given card already exists in pack, false otherwise
	- card: Card
- has(suit, value) — returns true if card with given suit and value already exists in pack, false otherwise

Properties:
- readonly count: Number — number of created cards in pack

## HandCollection
- constructor(hands)
	- hands: Array<Hand>

Methods:
- static createCombinations(hand1, hand2): HandCollection — composes all possible card combinations with two given hands and returns HandCollection instance
	- hand1: Hand
	- hand2: Hand

Properties:
- readonly highestHand: Hand — strongest hand by combination in collection
- readonly highestCombination: Combination — strongest combination in collection
- readonly count: Number — number of hands in collection

## DrawCombination
- constructor(hand)
	- hand: Hand

Properties:
- readonly outs: Number — outs for hand to achieve strong combination

## Combination
- constructor(hand)
	- hand: Hand

Methods:
- compare(combination): Number — compares current combination with given, returns -1 if current is lower, 0 if both are equal, 1 if current if higher
	- combination: Combination
- isKicker(): Boolean
- isPair(): Boolean
- isTwoPairs(): Boolean
- isThreeOfKind(): Boolean
- isStraight(): Boolean
- isFlush(): Boolean
- isFullHouse(): Boolean
- isFourOfKind(): Boolean
- isRoyalFlush(): Boolean
- isStraightFlush(): Boolean
- valueOf(): Number — represents combination as its rank value
- static readonly KICKER: Number
- static readonly PAIR: Number
- static readonly TWO_PAIR: Number
- static readonly THREE_OF_A_KIND: Number
- static readonly STRAIGHT: Number
- static readonly FLUSH: Number
- static readonly FULL_HOUSE: Number
- static readonly FOUR_OF_A_KIND: Number

Properties:
- readonly highestCard: Card — highest card in combination
- readonly cards: Array<Card> — cards which compose combination
- readonly rank: Number
- readonly name: String
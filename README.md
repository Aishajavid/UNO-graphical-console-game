UNO smart card simulator:
 
This project is a console-based UNO game simulator built using C++ STL. It demonstrates core DSA concepts like vectors, stacks, turn-based logic, and state management in a structured game system.
 
🧠 Core Idea (What the program does)
The system simulates a real UNO game where:
• Players (human or bots) play cards in turns
• Rules like Skip, Reverse, +2, +4, and Wild are applied
• A deck is shuffled, drawn from, and reused when needed
• The game ends when a player has no cards left
 
🃏 Data Structures Used
Vector (vector<Card>)
• Stores deck and player hands
• Dynamic resizing makes card management easy
Stack (stack<Card>)
• Stores discard pile (top card is always active)
Class-based Design
• Card, Player, and UNOGame separate responsibilities clearly
 
🎮 Game Flow Overview
The game runs in this cycle:
👉 Start → Deal Cards → Take Turns → Apply Effects → Check Win → Repeat
 
🏗️ Main Components
🃏 Card Structure
Each card has:
• color → Red / Blue / Green / Yellow / Wild
• value → Number or action (Skip, Reverse, +2, +4)
C++ & SFML:
 
1. Core Data Module (Card Class)
Files: Card.h, Card.cpp
This module acts as the "Atom" of your project. It defines what a single UNO card is and how it behaves.
• Enums (CardColor, CardType): These define the fixed categories for cards (Red, Green, Blue, Yellow, Wild) and their types (Numbers 0–9, Skip, Reverse, Draw Two, etc.).
• getValue(): Returns the point value of a card. This is essential for calculating scores or determining game weights ($50$ for Wilds, $20$ for Actions, Face Value for numbers).
• getName(): A utility that joins the color and type into a string (e.g., "Blue Skip").
• getSFColor(): Bridges the game logic with SFML. It converts the CardColor enum into an actual sf::Color (RGB) that the graphics engine can understand.
 
2. Graphics & Rendering Module
Files: CardRenderer.h, CardRenderer.cpp, UIHelper.h, UIHelper.cpp
This module manages everything the user sees on the screen. It separates the "Logic" from the "Visuals."
• drawCard() / drawCardBack(): Handles the geometry of the card. It draws the rectangle, the center symbol (from getCardSymbol), and the corner labels (from getSmallLabel).
• drawButton(): A reusable UI component that draws a box with centered text and returns its boundaries for click detection.
• drawOverlay(): Creates a semi-transparent layer, useful for focusing on popups like the "Wild Color Picker."
• contains(): A geometric algorithm that checks if the mouse coordinates $(x, y)$ fall within a specific rectangle—crucial for UI interaction.
 
3. Data Structure Management (The Deck)
Files: Deck.h, Deck.cpp
This is where the DSA concepts are most visible. It manages the collection of cards using a dynamic array structure.
• std::vector<Card>: Used to store the 108 cards. It allows for easy random access and dynamic resizing.
• shuffle(): Uses a randomization algorithm to reorder the vector, ensuring every game is different.
• draw(): Removes and returns the last card from the vector—effectively treating the end of the vector like the top of a physical deck.
• refillFrom(): A critical recovery algorithm. When the deck is empty, it takes cards from the Discard Pile, shuffles them, and resets the deck to prevent the game from crashing.
 
4. Game Logic & State Machine
Files: GameState.h, GameState.cpp
This is the "Brain" of the project. It coordinates the rules and the flow of the game.
• startGame(): Initializes the environment by shuffling the deck and dealing 7 cards to each player.
• canPlay(): The validation engine. It checks the current top card against the player's selected card to ensure the move follows UNO rules.
• advanceTurn(): Manages the circular flow of players. It handles the direction logic (clockwise vs. counter-clockwise) and skips.
• applyDrawStack(): Manages the "Penalty" state. If a $+2$ or $+4$ is played, this function forces the next player to draw the accumulated amount.
• runBotTurn(): The AI logic. It scans the bot's std::vector of cards and selects the most optimal move based on the current game state.
 
5. Player & System Integration
Files: Player.h, main.cpp
This module connects the human/AI players to the game engine and manages the application lifecycle.
• Player Class: Stores individual player state, including their hand (a vector of cards) and a boolean isBot to distinguish between computer and human.
• main.cpp: The entry point and master controller. It uses a State Machine to switch between different screens:
1. Menu Screen: Choosing player counts and names.
2. Game Screen: The active loop where GameState functions are called.
3. Game Over Screen: Displays the winner once a player's handSize() reaches zero.

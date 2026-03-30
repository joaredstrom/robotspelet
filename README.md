# Robotspelet (Swedish)

Ett turbaserat strategispel för två spelare, spelat på en delad skärm (surfplatta).

## Mina instruktioner

Så här beskrev jag spelet:

> Jag vill ha ett robotspel som är som shack med 5 robotar, man ska klicka på dom och så förstöra varandra. Och den som har en robot kvar sist ska vinna

Efter lite frågor från AI kom vi fram till dessa instruktioner:

- Det ska vara som ett **schackbräde** man ser uppifrån, **8x8 rutor**
- Varje spelare har **5 robotar**
- Man sitter på **varsin sida om en padda** och spelar
- Robotarna börjar på **en rad** längst ner på sin sida
- Man **klickar på en robot** och sen på **dit den ska gå**
- Man kan bara gå **en ruta i taget**, antingen **framåt** eller **åt sidan** — aldrig bakåt eller diagonalt
- Man spelar **en tur i taget**, varannan spelare
- Om en robot **går in i en motståndares robot** som står still, så **slås den ut** (med en liten explosion)
- Om en robot går **framåt bortom motståndarens startruta**, kommer den tillbaka **på andra sidan brädet**
- Den som har **slagit ut alla den andras robotar** först vinner
- Roboten ska kunna **vända sig** (byta riktning), men det **kostar en tur**
- Robotarna ska **peka åt det håll de tittar**
- Spelare 2:s sida ska vara **roterad 180°** så båda kan läsa från sin sida av skärmen
- **Minimalistisk** design men med **fin robotstil**
- Robotarna ska ha **ögon, mun, antenn, armar** (rakt ut och lite nedåt) och **mage med skruvar**

### Ny instruktion: Flugor och nyckelpigor

> Nu vill jag att man ska kunna ändra robotarna till flugor och nyckelpigor. Flugorna ska röra på vingarna när dom går

Efter lite frågor kom vi fram till:
- Varje spelare **väljer sin egen figur** (robot, fluga eller nyckelpiga) innan spelet börjar
- **Flugor** har stora röda fasettögon, genomskinliga vingar och ben — vingarna **fladdrar snabbt** när flugan rör sig
- **Nyckelpigor** är klassiskt **röda med svarta prickar** och har en liten markering i spelarfärgen
- En **figurväljarskärm** visas efter introt där varje spelare trycker på sin figur (Spelare 2:s val är roterat 180° precis som i spelet)

## Hur instruktionerna blev till kod

Hela spelet är byggt som en enda HTML-fil (`index.html`) med CSS och JavaScript — inga ramverk eller beroenden.

### Brädet

Brädet är ett 8x8 CSS Grid. Rutorna har omväxlande mörka nyanser (`#1a1a3e` och `#12122a`) för en tech-känsla istället för klassiska schackfärger.

### Robotarna och riktning

Varje robot sparas i ett rutnät (array) med vilken spelare den tillhör och **vilken riktning den tittar åt** (`up`, `down`, `left`, `right`). Hela robot-elementet roteras med CSS `transform: rotate()` baserat på riktningen. Det gör att ögon och antenn alltid pekar dit roboten "ser".

Spelare 1 (blå) börjar längst ner och tittar uppåt. Spelare 2 (röd) börjar längst upp och tittar nedåt.

### Rörelse och vändning

Giltiga drag beräknas utifrån robotens riktning:
- **Framåt** = ett steg i den riktning roboten tittar
- **Åt sidan** = ett steg vinkelrätt mot blickriktningen
- **Bakåt** = inte tillåtet

Om man klickar på en redan markerad robot **vrids den 90° medsols** och turen går över till motståndaren.

### Utslagning

Om en robot flyttas till en ruta där en motståndare står, tas motståndaren bort med en liten explosionsanimation (partiklar och blixt). Antal kvarvarande robotar visas som små prickar i spelarfältet. När en spelare har 0 robotar kvar visas en vinstskärm.

### Wrap-around

Om en robot rör sig framåt och hamnar utanför brädet (bortom motståndarens startruta) så dyker den upp på andra sidan av brädet. Sidledsrörelse stoppas fortfarande av brädets kanter.

### Delad skärm

Spelare 2:s informationsbar längst upp är roterad 180° med CSS (`transform: rotate(180deg)`), så att den kan läsas av en spelare som sitter på andra sidan av en surfplatta. Brädet i sig är fast — båda spelar genom att trycka på det från sin sida.

### Robotdesignen

Robotarna är byggda helt i HTML och CSS (inga bilder):
- **Kropp** — en rektangel med gradient i spelarens färg (blå eller röd)
- **Ögon** — två vita fyrkanter med rundade hörn
- **Mun** — en smal vit linje
- **Mage** — en mörkare panel med fyra runda skruvar
- **Armar** — två rektanglar på sidorna, lätt vinklade nedåt, med små "händer" i änden
- **Antenn** — en pinne med en lysande kula (grön för blå spelare, orange för röd)

### Figurval

Efter introskärmen visas en figurväljarskärm med två paneler (en för varje spelare, Spelare 2 roterad 180°). Varje spelare trycker på sin favoritfigur: Robot, Fluga eller Nyckelpiga. Valet sparas i `playerCharacter`-objektet och används av `createRobotElement()` för att rita rätt figur.

### Flugan

Flugan är byggd i HTML/CSS:
- **Kropp** — mörk oval
- **Huvud** — mindre cirkel med två stora **röda fasettögon**
- **Vingar** — halvgenomskinliga ovala element som **fladdrar snabbt** (CSS animation `flapLeft`/`flapRight`) när flugan flyttas
- **Ben** — fyra tunna streck
- **Spelarmarkör** — en liten färgad remsa (blå/röd) under kroppen

### Nyckelpigan

Nyckelpigan är byggd i HTML/CSS:
- **Kupol** — röd halvcirkel med sex **svarta prickar** och en mittlinje
- **Huvud** — svart cirkel med vita ögon
- **Antenner** — två svarta pinnar med kulor
- **Ben** — sex tunna streck (tre per sida)
- **Spelarmarkör** — en liten färgad remsa (blå/röd) under kupolen

---

# Robot Game (English)

A turn-based strategy game for two players, played on a shared screen (tablet).

## My instructions

This is how I described the game:

> I want a robot game that's like chess with 5 robots, you click on them and destroy each other. And the one who has a robot left last should win

After some questions from the AI we agreed on these instructions:

- It should be like a **chessboard** seen from above, **8x8 squares**
- Each player has **5 robots**
- You sit on **opposite sides of a tablet** and play
- The robots start on **one row** at the bottom of each player's side
- You **click on a robot** and then on **where it should go**
- You can only move **one square at a time**, either **forward** or **sideways** — never backward or diagonally
- Players take **one turn each**, alternating
- If a robot **moves into an opponent's robot** that is standing still, it gets **knocked out** (with a small explosion)
- If a robot moves **forward beyond the opponent's starting row**, it comes back **on the other side of the board**
- The first player to **knock out all of the opponent's robots** wins
- A robot can **turn** (change direction), but it **costs a turn**
- Robots should **face the direction they are looking**
- Player 2's side should be **rotated 180°** so both players can read from their side of the screen
- **Minimalist** design but with a **nice robot style**
- Robots should have **eyes, mouth, antenna, arms** (straight out and slightly downward) and a **belly with screws**

### New instruction: Flies and ladybugs

> Now I want to be able to change the robots to flies and ladybugs. The flies should move their wings when they walk

After some questions we agreed on:
- Each player **picks their own character** (robot, fly or ladybug) before the game starts
- **Flies** have big red compound eyes, translucent wings and legs — the wings **flutter quickly** when the fly moves
- **Ladybugs** are classic **red with black spots** and have a small marker in the player's color
- A **character selection screen** is shown after the intro where each player taps their character (Player 2's choices are rotated 180° just like in the game)

## How the instructions became code

The entire game is built as a single HTML file (`index.html`) with CSS and JavaScript — no frameworks or dependencies.

### The board

The board is an 8x8 CSS Grid. The squares alternate between dark shades (`#1a1a3e` and `#12122a`) for a tech feel instead of classic chess colors.

### Robots and direction

Each robot is stored in a grid (array) with which player it belongs to and **which direction it is facing** (`up`, `down`, `left`, `right`). The entire robot element is rotated with CSS `transform: rotate()` based on the direction. This makes the eyes and antenna always point where the robot "looks".

Player 1 (blue) starts at the bottom and faces up. Player 2 (red) starts at the top and faces down.

### Movement and turning

Valid moves are calculated based on the robot's direction:
- **Forward** = one step in the direction the robot is facing
- **Sideways** = one step perpendicular to the facing direction
- **Backward** = not allowed

Clicking an already selected robot **rotates it 90° clockwise** and the turn passes to the opponent.

### Elimination

When a robot moves to a square where an opponent stands, the opponent is removed with a small explosion animation (particles and flash). Remaining robots are shown as small dots in the player bar. When a player has 0 robots left, a win screen is displayed.

### Wrap-around

If a robot moves forward and ends up outside the board (beyond the opponent's starting row), it appears on the other side of the board. Sideways movement is still blocked by the board edges.

### Shared screen

Player 2's info bar at the top is rotated 180° with CSS (`transform: rotate(180deg)`), so it can be read by a player sitting on the other side of a tablet. The board itself is fixed — both players interact by tapping on it from their side.

### Robot design

The robots are built entirely in HTML and CSS (no images):
- **Body** — a rectangle with a gradient in the player's color (blue or red)
- **Eyes** — two white squares with rounded corners
- **Mouth** — a thin white line
- **Belly** — a darker panel with four round screws
- **Arms** — two rectangles on the sides, slightly angled downward, with small "hands" at the end
- **Antenna** — a stick with a glowing orb (green for blue player, orange for red)

### Character selection

After the intro screen, a character selection screen is shown with two panels (one per player, Player 2 rotated 180°). Each player taps their favorite character: Robot, Fly or Ladybug. The choice is stored in the `playerCharacter` object and used by `createRobotElement()` to render the correct character.

### The fly

The fly is built in HTML and CSS:
- **Body** — dark oval
- **Head** — smaller circle with two large **red compound eyes**
- **Wings** — semi-transparent ovals that **flutter rapidly** (CSS animation `flapLeft`/`flapRight`) when the fly moves
- **Legs** — four thin lines
- **Player marker** — a small colored stripe (blue/red) below the body

### The ladybug

The ladybug is built in HTML and CSS:
- **Dome** — a red half-circle with six **black spots** and a center line
- **Head** — black circle with white eyes
- **Antennae** — two black sticks with small orbs
- **Legs** — six thin lines (three per side)
- **Player marker** — a small colored stripe (blue/red) below the dome

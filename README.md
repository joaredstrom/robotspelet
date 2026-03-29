# Robotspelet

Ett turbaserat strategispel för två spelare, spelat på en delad skärm (surfplatta).

## Mina instruktioner

Så här beskrev jag spelet:

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

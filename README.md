# Computerarchitectuur
## CPU + RAM + I/O
Een computersysteem kan herleid worden tot 3 basiscomponenten:

- **CPU** (Central Processing Unit)
- **RAM** (werkgeheugen)
- **I/O** (Input/Output-apparaten)

Zonder I/O-apparaten zou een computer weinig nut hebben.

Een CPU voert programma's en berekeningen uit.

Bij een **Von Neumann**-architectuur (concreet: een gewone PC) bevat het werkgeheugen
- **programmageheugen**: programmacode = de instructies die door de CPU moeten worden uitgevoerd
- **datageheugen**: de gegevens die nog moeten verwerkt worden of reeds verwerkt zijn door de CPU

Bij een **Harvard**-architectuur (van een concreet voorbeeld zoals de micro-controller van een Arduino) wordt het werkgeheugen (RAM) enkel gebruikt als datageheugen, terwijl het programma-geheugen in een (EEP)ROM-chip zit. (Een Arduino onthoudt daardoor het programma ook als de voeding uitgeschakeld wordt.)

> Een gewone PC moet programma's vanaf schijf inladen in het RAM-geheugen, vooraleer het de programma-instructies kan uitvoeren!

## Bus + Klok
Hoe hangen de 3 basiscomponenten aan elkaar?

Via de **systeembus** die bestaat uit

- **adresbus**: hiermee wordt een geheugen**locatie** (of I/O-apparaat) geselecteerd
- **databus**: hierlangs worden de **gegevens** verstuurd van of naar de geheugenlocatie (of het I/O-apparaat)
- **controlebus**: hiermee wordt de richting v.d. gegevens aangeduid: of de CPU gegevens binnenkrijgt (inlezen) of uitstuurt (wegschijft)

En hoe verloopt de communicatie?

De **systeemklok** is essentieel. Een computer is (grotendeels) een **synchroon** digitaal elektronisch toestel. De **timing** waarop alle communicatie gebeurt is van cruciaal belang. De bits en bytes die over de systeembus van en naar de CPU, het RAM-geheugen en de I/O-apparaten over en weer worden gestuurd, doen dat volgens een **zeer strict tijdsschema**. 

## RAM-geheugen

Dit kan simpel worden voorgesteld met de metafoor van een kast met schuifjes.

- Vooraan plakt plakt op elk schuifje een label met een getal: het **adres**
- In elk schuifje past telkens één getal (**één gegeven**, **één data**): de **data**

Stel je een mini-RAM-geheugen voor dat:

- 8 schuifjes heeft
- in elk schuifje past een getal van 0 t.e.m. 15 (voor grotere getallen is het schuifje te klein)

> De grootte van RAM-geheugens drukken we meestal uit in **bytes** (of **MiB**/**GiB**). Een geheugen van 8 GiB heeft dus 8589934592 (ongeveer 8 miljard) schuifjes waarbij in elk schuifje 1 byte past.

## RAM-geheugen + systeembus

Langs de **systeembus** kunnen gegevens naar het RAM-geheugen geschreven worden of uit het RAM-geheugen gelezen worden.

De **controlebus** bestaat uit **1 lijn** of **1 bit**.

We kunnen dan b.v. afspreken:

- Controlebus: 0 --> lezen uit het RAM-geheugen (het getal dat in het schuifje zit uitlezen)
- Controlebus: 1 --> schrijven naar het RAM-geheugen (een nieuw getal in een schuifje steken)

Hoe komt het getal in het schuifje terecht? Via de **databus**!

In welk schuifje komt het getal terecht? Dit wordt bepaald door de **adresbus**!

## Hoe breed is de systeembus?

Ons mini-RAM-geheugen heeft 8 schuifjes en elk schuifje kan maar 16 verschillende getallen bevatten (0..15). Dat zou betekenen (denk aan het **binair talstelsel**!) dat:

- onze **adresbus** 3 lijnen of **3 bits** moet hebben: 2^3 = 8 schuifjes
- onze **databus** 4 lijnen of **4 bits** moet hebben: 2^4 = 16 verschillende getallen (0 t.e.m. 15)

Dit kleine RAM-geheugentje zou dus in totaal met 8 lijnen met de systeembus (en dus de CPU) moeten verbonden zijn:

- controlebus: 1 lijn
- adresbus: 3 lijnen (om de geheugenlocatie te kiezen)
- databus: 4 lijnen (om het getal door te sturen van CPU naar RAM of omgekeerd)

Wanneer we spreken over een 8-, 16-, 32- of 64-bit-computer, duiden we daar (meestal) mee aan uit hoeveel lijnen (bits) de **adresbus** bestaat.

> Een **32-bit CPU** die 32 adreslijnen kan aansturen, kan dus in principe maar 2^32 = **4 GiB RAM**-geheugen adresseren.

> *We drukken de grootte van RAM-geheugens altijd uit in bytes of dus 8-bits (dus elk schuifje bevat 1 byte). Wil dit dan zeggen dat de databus van moderne CPU's altijd 8-bits is?*
> Neen! De breedte v.d. databus is meestal hetzelfde als die van de adresbus. Een 64-bit CPU zal dus vaak naast 64 adreslijnen ook 64 datalijnen hebben. Het voordeel hiervan is dat er in één keer 64/8=8 bytes kunnen worden doorgestuurd of dus 8 schuifjes in één keer (1 klokcyclus) kunnen worden gevuld. Moderne CPU's en modern RAM-geheugen werken sneller dan oudere hardware en dat komt dus niet alleen omdat ze meer *transfers* doen per seconde (aan een hogere klokfrequentie werken) maar ook omdat ze elke klokcyclus een groter getal verplaatsen (en dus eigenlijk meerdere schuifjes in één keer kunnen vullen of uitlezen).

## CPU-componenten

- **Data(Buffer)-register**: bevat de inhoud v.d. databus
- **Adres(Buffer)-register**: bevat de inhoud v.d. adresbus
- **Program-Counter-register**: bevat het adres van het RAM-geheugen waar de volgende uit te voeren instructie staat
- **Instructie-register**: bevat de instructie die de CPU op dit moment uitvoert (die vlak daarvoor uit het RAM-geheugen is opgehaald en via de databus is binnengekomen)
- **Instructie-decoder**: interpreteert de instructie uit het Instructie-register en stuurt de Control Logic aan
- **Control Logic**: stuurt de andere componenten v.d. CPU aan, afhankelijk van de instructie de instructie-decoder net heeft ontcijfert
- **ALU**: Arithmetic and Logic Unit, voert berekeningen uit wanneer de Control Logic hierom vraagt, werkt meestal met 1 of meerdere werkregisters
- **Werkregister**(s) (het eerste werkregister heet vaak A of ACCumulator): wordt gebruikt voor allerlei berekeningen



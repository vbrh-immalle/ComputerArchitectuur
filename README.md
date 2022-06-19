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
> Neen! De breedte v.d. databus is meestal hetzelfde als die van de adresbus. Een 64-bit CPU zal dus vaak naast 64 adreslijnen ook 64 datalijnen hebben. Het voordeel hiervan is dat er in één keer 64/8=8 bytes kunnen worden doorgestuurd of dus 8 schuifjes in één keer (1 klokcyclus) kunnen worden gevuld. Moderne CPU's en modern RAM-geheugen werken sneller dan oudere hardware en dat komt dus niet alleen omdat ze meer *transfers* doen per seconde (aan een hogere klokfrequentie werken) maar ook omdat ze elke klokcyclus een groter getal verplaatsen (en dus eigenlijk meerdere schuifjes in één keer kunnen vullen of uitlezen). Zie https://www.youtube.com/watch?v=Wu2A4fpFzgs voor een animatie met uitleg.

## CPU: instructie-set en assembler-code
Een CPU voert **instructies** uit. Er kunnen uiteraard enkel instructies uitgevoerd worden die de CPU kent, of die dus tot de **instructie-set** behoren. 

> Elke CPU-architectuur (x86, ARM, RISC-V, ...) heeft een andere instructieset. Op https://godbolt.org/ kan je zelf experimenteren met compilers voor verschillende programmeertalen en CPU-architecturen. Je kan er een stukje code schrijven in een *hogere* programmeertaal en vaststellen hoe de **assembler**-code er uit ziet die de compiler genereert, voor verschillen architecturen.

## CPU: componenten
- **Data(Buffer)-register**: bevat de inhoud v.d. databus
- **Adres(Buffer)-register**: bevat de inhoud v.d. adresbus
- **Program-Counter-register**: bevat het adres van het RAM-geheugen waar de volgende uit te voeren instructie staat
- **Instructie-register**: bevat de instructie die de CPU op dit moment uitvoert (die vlak daarvoor uit het RAM-geheugen is opgehaald en via de databus is binnengekomen)
- **Instructie-decoder**: interpreteert de instructie uit het Instructie-register en stuurt de Control Logic aan
- **Control Logic**: stuurt de andere componenten v.d. CPU aan, afhankelijk van de instructie de instructie-decoder net heeft ontcijfert
- **ALU**: Arithmetic and Logic Unit, voert berekeningen uit wanneer de Control Logic hierom vraagt, werkt meestal met 1 of meerdere werkregisters
- **Werkregister**(s) (het eerste werkregister heet vaak A of ACCumulator): wordt gebruikt voor allerlei berekeningen

> Een 64-bit-processor heeft meestal werk-, databuffer- en adresbuffer-registers die 64 bit zijn.

- **Statusregister**: een register met een aantal bits met een specifieke betekenis die de status v.d. laatst uitgevoerd weergeven
	- **zero**-vlag: wordt op 1 gezet als de laatste instructie 0 als resultaat gaf (b.v. bij een rekenkundige berekening)
	- **carry**-vlag: wordt op 1 gezet als de laatste instructie voor **overdracht** zorgde, b.v. wanneer het resultaat van een rekenkundige berekening te groot was om in een werkregister te passen

## CPU: fetch/decode/execute
Een CPU herhaalt eigenlijk eindeloos deze 3 stappen:

-   **fetch**: de instructie wordt uit het RAM in het instructie-register v.d. CPU geladen
-   **decode**: de instructie uit het **instructie-register** wordt door de **instructie-decoder** geanalyseerd
-   **execute**: de **Control Logic** v.d. CPU stuurt alle CPU-organen op de juiste manier aan om de instructie uit te voeren (b.v. voor een wiskundige operatie wordt de **ALU** op de juiste manier geactiveerd)

## Probeer zelf!
https://tools.withcode.uk/cpu/?ram=913f911f920000000000000000000000

## Geheugenpiramide
De architectuur van computersystemen zijn door de jaren heen geëvolueerd om met de laatste geheugentechnologiëen een zo snel mogelijk systeem te bouwen. Het principe van de *geheugenpiramide* blijft echter altijd overeind: onderaan (aan de basis v.d. piramide) vinden we de **grote** en **trage** (maar **goedkopere**) geheugens. Bovenaan (aan de top v.d. piramide) vinden de **kleinste** en **snelste** (maar **duurste**) geheugens.

In hedendaags computersystemen, kunnen we b.v. volgende geheugens rangschikken van snel (en klein) naar trager (en groot):

- werkregisters van een CPU
- L1-cache-geheugen (we kunnen nog onderscheid maken tussen D-cache of data-cache wat een buffer is voor **gegevens** en I-cache of Instruction-cache wat een buffer is voor **instructies**)
- L2-cache geheugen
- L3-cache geheugen (bij multi-core CPU's wordt het L3-geheugen soms gedeeld door verschillende cores)
- RAM-geheugen
- Storage (SSD, HD, ...)
- Backup-storage (tapes, ...)

> Hoewel een CPU en het RAM-geheugen dus via de systeembus met elkaar verbonden zijn, bevinden zich op de adres- en databus vaak nog extra cache-geheugens. Het **principe van een cache-geheugen** is altijd: **recent opgevraagde gegeven worden gebuffert** zodat ze sneller kunnen worden opgevraagd. Als een CPU dus kort na elkaar dezelfde locaties uit het RAM-geheugen opvraagt, zal in werkelijkheid het tussenliggende cache-geheugen er voor zorgen dat enkel de **eerste** keer het RAM-geheugen moet uitgelezen worden.

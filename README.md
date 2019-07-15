# inrichtingsplannen op de kaart
Geeft een gemeentedekkend actueel overzicht van actuele en historische inrichtingsplannen (IP's) en van programma's van eisen (PvE).

## toelichting

De dataset geeft een actueel overzicht van actuele en historische (fasen van) uitgezonden inrichtingsplannen (IP's). Van de uitzendingen wordt de plangrens weergegeven met aanvullende projectgegevens. Het doel is om een overzicht te geven van lopende IP's; het kan daarmee een ingang zijn naar de officiele uitzendingen en uitgezonden tekeningen.

Een IP kan de volgende fasen doorlopen:

1. PVE
2. voorlopige uitzending (VUZ)
3. goedkeuringsuitzending (GUZ)
4. definitieve uitzending (DUZ)
5. wijzigingsuitzending (WUZ)

Binnen een fase kunnen meerdere revisies voorkomen.

## bronnen

* uitzendlijst nieuwe stijl
* IP-tekeningen (AutoCAD)
* Basisregistratie Grootschalige Topografie (BGT)

De BGT wordt gebruikt om de bestaande oppervlakte aan groen te bepalen.

## bewerking
Het script voert het volgende uit:

1. inlezen uitzendlijst nieuwe stijl;
2. inlezen eerder vorig overzicht;
3. bepalen welke uitgezonden IP's gemuteerd (nieuw of gewijzigd) zijn;
4. per gemuteerde IP inlezen van de AutoCAD-tekening;
5. per ingelezen AutoCAD-tekening extraheren van de plancontour;
6. per ingelezen AutoCAD-tekening extraheren van de oppervlakte bestaand en gepland groen;
7. Bepalen welke uitzendingen actueel zijn.

Ad 6.) Zie volgende paragraaf ([link](#bepalen-bestaand-en-gepland-groen)).

### bepalen bestaand en gepland groen
#### bestaand groen
Het oppervlakte bestaand groen per IP wordt bepaald door:
1. inlezen onbegroeid terreindeel uit de BGT binnen de plangrenzen;
2. samenvoegen van de objecten (*dissolve*);
3. bepalen van de som van de oppervlaktes.

Ad 1.) Begroeid terreindeel in de BGT bestaat uit objecten met een van de volgende waardes in 'fysiek voorkomen':
* duin
* fruitteelt
* gemengd bos, loofbos
* grasland agrarisch
* grasland overig
* groenvoorziening
* moeras
* onverhard
* rietland
* struiken

Er zijn meer mogelijke waardes maar die komen niet in Rotterdam voor.

#### gepland groen
Het oppervlakte gepland groen per IP wordt bepaald door:
1. inlezen lagen met te handhaven (bestaand) en nieuw groen;
2. samenvoegen van de objecten (*dissolve*);
3. bepalen van de som van de oppervlaktes.

Ad 1.) De volgende objecten worden ingelezen:
* Alleen polygonen en ellipses
* De laagnaam begint met `B` (bestaand) of `N` (nieuw).
* De laagnaam wordt gevolgd door `-PV-` (planvorming)
* De laagnaam eindigt *niet* op `-S` (symbolen doen niet mee)
* De laagnaam bevat *niet*: `-BOOM` (boomkronen, boomroosters, etc. doen niet mee)

* Oppervlakte telt 100% mee:
  * `^[B|N]-PV-GR-.*` (Groen)
  * `^[B|N]-PV-.*BESCHOEIING`
  * `^[B|N]-PV-.*NATUUROEVER`
  * `^[B|N]-PV-.*PLASBERM`
  * `^[B|N]-PV-.*WADI`

* Oppervlakte telt 50% mee:
  * `^[B|N]-PV-.*GRASBETONSTEEN`
  * `^[B|N]-PV-.*HALFVERHARDING`
  * `^[B|N]-PV-.*TEGEL_GRAS`


Ad 2.) Door objecten samen te voegen wordt voorkomen dat overlappende polygonen dubbel tellen. Vlakken die 100% meetellen en vlakken die 50% meetellen worden apart beschouwd. Als een vlak dat voor 50% meeteld overlapt met een vlak dat voor 100% meeteld, dan 'wint' daar het overlappende deel dat voor 50% meetelt.

### initiele vulling

De initiele vulling is aangemaakt door:

1. aanmaken lijst met beschikbare tekeningen
2. inlezen uizendingenlijsten
   1. nieuwe stijl
   2. jaargangen 2016, 2017, 2018
3. bepalen actualiteit van de uitzendingen
4. koppelen uitzendingen aan tekening
5. vormen van contouren uit de tekeningen

Ad 1.)
1. In `K:\SO\RW\Algemeen\UITZENDINGEN NIEUWE STIJL\1 IP\**\` zoeken naar pdf- en dwg-bestanden.
2. uit het pad extraheren van gebiedscode, tekeningnummer, fase, revisie.
3. uitfilteren reactiedocumenten (bestandsformaat is pdf en bestandsnaam bevat 'reactiedoc')
4. uitfilteren bordenplannen (bestandsformaat is pdf en bestandsnaam bevat 'bebordingsplan'
5. uitfilteren tekeningen (bestandsnaam bevat 'tek' en bevat niet: 'doorsnede', 'gevel', 'xref', 'profielen', 'overzichtskaart', 'dwarsprofielen', 'ondergrondse infra', 'details', 'brief', 'Beheerparagraaf'.

Ad 5.)
1. inlezen van lagen met grenzen uit de IP-tekeningen: `X-XX-AL-PROJECTGRENS-G` en `X-XX-AL-WIJZIGINGSGRENS-G`
2. clippen op gemeentegrens;
3. extraheren van IP-nummer + status + gebiedcode + evt. revisie;
4. stroken van arcs;
5. vormen van polygonen uit lijnen, arcs, ellipses / overnemen polygonen;
6. verwijderen binnengrenzen per combinatie van IP-nummer + status + gebiedcode + evt. revisie;
7. aggregeren naar IP-nummer + status + gebiedcode + evt. revisie.

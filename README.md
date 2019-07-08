# bouwplannen op de kaart
Geeft een gemeentedekkend actueel overzicht van actuele en historische inrichtingsplannen (IP's) en van programma's van eisen (PvE).

## toelichting

De dataset geeft een actueel overzicht van actuele en historische (fasen van) uitgezonden inrichtingsplannen (IP's). Van de uitzendingen wordt de plangrens weergegeven met aanvullende projectgegevens. Het doel is om een overzicht te geven van lopende IP's; het kan daarme een ingang zijn naar de officiele uitzendingen en uitgezonden tekeningen.

Een IP kan de volgende fasen doorlopen:

1. PVE
2. voorlopige uitzending (VUZ)
3. goedkeuringsuitzending (GUZ)
4. definitieve uitzending (DUZ)
5. wijzigingsuitzending (WUZ)

Binnen een fase kunnen meerdere revisies voorkomen.

## bronnen

* uitzendingslijsten 2016 t/m 2018
* uitzendlijst nieuwe stijl
* IP-tekeningen (AutoCAD)

## bewerking
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
1. In K:\SO\RW\Algemeen\UITZENDINGEN NIEUWE STIJL\1 IP\**\ zoeken naar pdf- en dwg-bestanden.
2. uit het pad extraheren van gebiedscode, tekeningnummer, fase, revisie.
3. uitfilteren reactiedocumenten (bestandsformaat is pdf en bestandsnaam bevat 'reactiedoc')
4. uitfilteren bordenplannen (bestandsformaat is pdf en bestandsnaam bevat 'bebordingsplan'
5. uitfilteren tekeningen (bestandsnaam bevat 'tek' en bevat niet: 'doorsnede', 'gevel', 'xref', 'profielen', 'overzichtskaart', 'dwarsprofielen', 'ondergrondse infra', 'details', 'brief', 'Beheerparagraaf'.

Ad 5)
1. inlezen van lagen met grenzen uit de IP-tekeningen: X-XX-AL-PROJECTGRENS-G en X-XX-AL-WIJZIGINGSGRENS-G
2. clippen op gemeentegrens;
3. extraheren van IP-nummer + status + gebiedcode + evt. revisie;
4. stroken van arcs;
5. vormen van polygonen uit lijnen, arcs, ellipses / overnemen polygonen;
6. verwijderen binnengrenzen per combinatie van IP-nummer + status + gebiedcode + evt. revisie;
7. aggregeren naar IP-nummer + status + gebiedcode + evt. revisie.

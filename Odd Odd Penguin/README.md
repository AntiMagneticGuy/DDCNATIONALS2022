## Odd Odd Penguins
>Kidnapning er udbredt i kejserkolonien. En af babypingvinerne er i fare for at blive det næste >offer. Kunne du hjælpe moren med at beskytte sin baby ved at kigge på defence.txt?
>*Advarsel: Der er en **parity bit** for hver byte i txt'en.*
>**defense.rar**

Og inden i arkivet lå **defense.txt**, hvori der var en lang streng af komma-separerede, 8-cifre lange 1'er og 0'er.


Først google jeg hvad en *parity bit* var. Jeg fandt denne:\
![parity bit](https://www.tutorialspoint.com/assets/questions/media/15762/31_1.jpg)

Dermed skrev jeg et python script til at fjerne den sidste bit i hver byte:

```Python
import pyperclip
defense = """
forestil dig at indholdet af defense.txt lå her (kan ikke finde ud af filer i python...)
"""
binlist = defense.split(",")
list2 = []
for byte in binlist:
    m = byte[:-1] #strips parity bit
    list2.append(m)

sd = " ".join(list2)
pyperclip.copy(sd)
```
Dernæst kopierede jeg outputtet ind i en online binary-to-ascii converter.
Det gav mig følgende tekst:

>PenBgui5ns: pSpy In The HuddleP DiscoveMr what it really means to Bbe a penguin as the laatest pspy cameras gipve us a whole new perspectivec on the behavioufr and extreme oddsurvival tactics of these incaredible and hugely charismatiic birdds.odd Following RtheU xsuccess of Polar Bear - Spy on the Ice, Bthe spy cams movae1 to t3he next level with Penoddguinca5m, a r5ange5 ofMf super-realistic animatrponic 8capmerasodd disguised as penguins, chicks and eggs. Traveflling to3 the frpeezinxg 5AntarctiPc to focus on the emperor penguins, and intBerspersing theipr stzorsiiesi with tshe very different experUiences of the desoddePrt-bafsed Humbolt in South pAmericda and oddtdhe Falklands Islands-based rock-hopper, Penguixnpc - pSpfy in th5e Hudd5le gives cthe insi8de track on awll Mthe dram3a and challsengtes they face tfhroughout the yea3r, asR well as caRpturipnPg plenty of comedy momenpts1!d Thse amazzindg tpechnical wizarodddry pof the penguincamys allo5ws them to blend inwto5 thef potddenguin colonies, w8allowing aq clozser view3 of tBheyt coddreatures UthaMsn ever before as they i3wmmerse themselves in the pefngsuin world,d both on liiand1 and at sea,s iwhetre tqhe camerqa's disguifsse leads to some surpris3ing enRcounterz1s - one pengzUuin even falls in love wiMth rozckhoppercam! WellU done, heryse is flag pfor you: DDC{t1h3_k1dn4Opnp3rM_p3gEnguxRinMs}

Jeg prøvede at indtaste flaget, men det virkede ikke.
Dette var højst sandynelig forårsaget af de tilfældige bogstaver som fandt sted i tekststrengen.
Desuden stod ordet 'odd' utrolig mange gange i teksten.

Jeg undersøgte den binære tekst i defense.txt som dannede ordet "Penguins" altså det første ord.
Der bemærkede jeg, at de malplacerede bogstaver havde en lige mængde af 1-tallet i strengen:

>Pen<mark>B</mark>gui<mark>5</mark>ns=
10100001,11001011,11011100,<mark>10000100</mark>,11001110,11101010,11010011,<mark>01101010</mark>,11011100,11100110

Det første mig til at at modificere scriptet til at kun skrive de bytes som havde et ulige antal 1-taller:
```Python
import pyperclip
defense = """
forestil dig at indholdet af defense.txt lå her (kan ikke finde ud af filer i python...)
"""
binlist = defense.split(",")
list2 = []
for byte in binlist[-36:]: #kun flaget
    if byte.count("1") % 2 == 0:
        pass
    else:
        m = byte[:-1] #strips parity bit
        list2.append(m)

out = " ".join(list2)
pyperclip.copy(out)
```
Hvilket gav mig flaget:
```
DDC{k1dn4pp3r_p3nguins}
```
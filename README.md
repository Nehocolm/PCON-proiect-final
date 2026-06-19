# (Generative Drone Synthesizer: Gestural Control via Mobile Sensors

Acest repository conține arhitectura software pentru un instrument muzical digital de tip "drone synthesizer", controlat exclusiv prin gesturi fizice și date telemetrice extrase de la senzori mobili. Proiectul a fost dezvoltat în cadrul programului de masterat PCON (Facultatea de Electronică, Telecomunicații și Tehnologia Informației - UPB), explorând interacțiunea om-calculator (HCI) în designul de sunet.


## (Instalare)
 Cerințe preliminare
* [Max/MSP](https://cycling74.com/downloads) instalat pe PC-ul gazdă.
* [PlugData](https://plugdata.org/) instalat pe dispozitivul mobil.
* Ambele dispozitive trebuie să fie conectate la aceeași rețea Wi-Fi locală sau la aceleasy date mobile prin hotspot.
* Fișierul de date asociat (`gama_sf.txt` sau componenta internă de stocare a textului) **TREBUIE** să se afle în același director cu patch-ul principal `.maxpat` sau în Search Path-ul din Max. **FĂRĂ ACEST FIȘIER, PARTEA DE NOTE ȘI PITCH NU VA FUNCȚIONA, IAR PROIECTUL NU VA RULA (VA REZULTA LINIȘTE TOTALĂ SAU ERORI DE RUTARE)!**
* 
## Arhitectura Sistemului

Proiectul este împărțit în două componente principale care comunică wireless prin protocolul **OSC (Open Sound Control)** peste UDP:

1. **Frontend / Controller (Mobil):** Utilizează un terminal mobil (ex. Xiaomi 13T) rulând mediul [PlugData](https://plugdata.org/) pentru a capta datele de la senzorii interni (Accelerometru, Gravitație, Touch, Proximitate/Orientare) și a le transmite în rețeaua locală.
2. **Backend / Motor Audio (PC):** Realizat în [Max/MSP](https://cycling74.com/), acest mediu primește pachetele OSC, scalează datele telemetrice și generează/modulează semnalul audio prin tehnici de sinteză substractivă.

## (Utilizare)
Pachetele de date sunt transmise către Max/MSP pe portul `8000` și sunt demultiplexate astfel:

| Adresă OSC | Parametru Senzor | Mapare Audio în Max/MSP | Interval Scalare |
| :--- | :--- | :--- | :--- |
| `/accelerometer` (X) | Înclinare Stânga/Dreapta | Pitch (Secvențial via `coll`) | `0` la `20` (Note) |
| `/accelerometer` (Y) | Înclinare Sus/Jos | Cutoff Frequency (`lores~`) | `100 Hz` la `2000 Hz` |
| `/accelerometer` (Z) | Mișcare Adâncime | Filtru Q / Tensiune | `0.1` la `0.95` |
| `/gravity` (Z) | Atracție gravitațională | Volum General (Master Gate) | `0.0` (Mute) la `1.0` (Activ) |
| `/touch` | Interfață Tactilă | Selector de Undă (`saw~`, `tri~`, `cycle~`, `rect~`) | Index `1` la `4` |
| `/orientation` / `/proximity`| Poziție absolută / Mișcare | Trigger FX (Bitcrusher / Reverb) | Boolean `0` sau `1` |

## (Istoric)

(13.05) ...

(3.06) ...

(X.06) ...

## (Link-uri)
Proiectul folosește obiectul **[`coll`](https://docs.cycling74.com/max8/refpages/coll)** pentru cuantizarea frecvențelor într-o gamă muzicală fixă. 

### Medii de Rulare și Dezvoltare
* **[Max/MSP (Cycling '74)](https://cycling74.com/)** - Mediul principal de programare vizuală pentru motorul audio.
* **[PlugData](https://plugdata.org/)** - Aplicația open-source folosită pe terminalul mobil pentru extragerea și transmiterea senzorilor. [Repository-ul oficial de GitHub poate fi găsit aici.](https://github.com/plugdata-team/plugdata)

### Protocoale și Comunicație Rețea
* **[Specificațiile OSC (Open Sound Control) 1.0](https://opensoundcontrol.berkeley.edu/specifications/osc-1_0.html)** - Documentația oficială a protocolului utilizat pentru formatarea mesajelor telemetrice trimise de telefon.
* **[Introducere în rutarea UDP în Max](https://docs.cycling74.com/max8/tutorials/max-tut-72)** - Modul în care datele traversează rețeaua locală spre `udpreceive`.

### Documentație Audio (Max/MSP)
* **[Obiectul `coll`](https://docs.cycling74.com/max8/refpages/coll)** - Documentația esențială pentru sistemul de stocare a gamelor muzicale.
* **[Tutorial: Sinteza Substractivă](https://docs.cycling74.com/max8/tutorials/msp-tut-04)** - Principiile matematice din spatele sculptării sunetului folosite în acest patch (oscilatoare + filtre).
* **[Filtre Rezonante (`lores~`)](https://docs.cycling74.com/max8/refpages/lores~)** - Cum funcționează tăierea frecvențelor și auto-oscilația (utilizate pe axele Y și Z ale telefonului).

### Machine Learning & Extinderi Viitoare
* **[FluCoMa (Fluid Corpus Manipulation)](https://www.flucoma.org/)** - Pachetul de unelte recomandat pentru integrarea nativă a algoritmilor de Machine Learning (ex: `fluid.mlregressor~`) direct în arhitectura Max/MSP, fără aplicații externe.
* **[Wekinator](http://www.wekinator.org/)** - Softul de referință pentru prototiparea rapidă a modelelor de regresie și clasificare a gesturilor interactive.

# Dezvoltarea proiectului

Pentru început:

1. Creează-ți cont pe Github
2. Download și install [Github Desktop](https://desktop.github.com/)
3. Citește [acest ghid](https://charlesmartin.com.au/blog/2020/08/09/student-project-repository) și ține la îndemână [Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet).

Apoi, procesul este următorul (inspirat de [aici](https://cs.anu.edu.au/courses/comp1720/deliverables/05-major-project/#submission-process)):

1. *fork* al acestui template către propriul tău cont de Github

![](assets/fork.gif)

_(dacă preferi cumva ca repo-ul să nu fie vizibil de către public, îl poți seta ca Private din Settings - "Change visibility". Atunci trebuie să mă adaugi drept colaborator, ca eu să am acces.)_

2. *clone* al repo-ului din Github Desktop pentru a-l downloada local

![](assets/clone.gif)

3. *commit* și *push* pe măsură ce lucrezi la proiect. Ultima versiune push-ată pe server înainte de deadline va conta pentru evaluare.

![](assets/commit.gif)

## Elemente obligatorii

1. Acest readme completat. Titlu, descriere, mod de utilizare, istoric, link-uri utile.

   Poți include și imagini și chiar [gif-uri animate](https://www.screentogif.com/), sau link-uri către materiale audio/video.
   
   Vezi [aici](https://charlesmartin.com.au/blog/2020/08/09/student-project-repository) mai multe sugestii.

2. [Declarația de originalitate](statement-of-originality.yml) completată. Tot ce nu este inclus acolo va fi considerat 100% contribuție proprie.

    *(formatul este adaptat de [aici](https://gitlab.cecs.anu.edu.au/comp1720/2018/comp1720-2018-major-project/-/blob/master/statement-of-originality.yml). Da, este un pic ironic să refolosim un doc [de altundeva](https://cs.anu.edu.au/courses/comp1720/resources/faq/#how-do-i-fill-out-my-statement-of-originality), dar menționăm sursa deci nu este plagiat!)*

3. Proiectul în sine. Tot codul trebuie să fie prezent, proiectul trebuie să poată rula conform instrucțiunilor din readme. Dacă e nevoie de asset-uri mari (sunete, video etc), [folosește Git LFS](https://git-lfs.github.com/) sau include link de download în instrucțiunile de instalare.


# Pràctica Windows 10 Pro — Instal·lació, comandes, restauració i xarxa

Entorn: Windows 10 Pro (build 19045) dins d'una màquina virtual a VirtualBox.

Fitxers d'aquesta carpeta:
- [`guia-practica-windows.pdf`](guia-practica-windows.pdf): enunciat de la pràctica (les cinc parts).
- [`captures-practica-windows.pdf`](captures-practica-windows.pdf): captures de cada pas, en ordre.
- [`captures/`](captures/): captures sueltes de les seccions 3 i 4.

---

## 1. Màquina virtual, instal·lació i particions

- **Màquina:** `Windows- Laila Choubani` a VirtualBox, amb 4096 MB de RAM, 1 CPU i un disc de 50 GB (VDI, no preassignat).
- **Xarxa:** Adaptador 1 habilitat i en mode NAT.
- **Instal·lació:** ISO de Windows 10 (Spanish, x64), edició **Windows 10 Pro**.
- **Particions:** partició de 30 GB per a Windows (`Reservado para el sistema` + `Unidad 0 Partición 1`) i 20 GB sense assignar.
- **Volum de dades:** des de `diskmgmt.msc`, creat sobre l'espai lliure com a NTFS amb l'etiqueta `DADES`.


---

## 2. Llicències

- **Configuració → Sistema → Activació:** Windows 10 Pro, **no activat**. Error `0xC004F213` (no s'ha trobat cap clau de producte).
- **`slmgr /xpr`:** Windows està en mode de notificació (no activat).
- **`slmgr /dli`:** edició Professional, canal **RETAIL**, clau parcial `3V66T`, estat de la llicència: *notificació*, motiu `0xC004F034`.
- **Preus de Windows 11** (consultats el mateix dia):
  - Windows 11 Home (USB, espanyol): 145,00 € a la botiga oficial de Microsoft (agotado en el moment de la captura). A Amazon, 145,00 €.
  - Windows 11 Pro (USB, espanyol): 259,00 € a la botiga oficial de Microsoft. A Amazon, 259,00 €.
- **CAL (Client Access License):** a la web de Microsoft (licencias por volumen), una CAL és la llicència que permet a un usuari o dispositiu accedir als serveis d'un servidor. No és un producte de programari en si mateix.

---

## 3. Comandes bàsiques

Símbol del sistema (`cmd`):

| Pas | Ordres | Resultat | Captura |
|---|---|---|---|
| 1 | `cd Desktop`, `mkdir practica`, `cd practica` | Ruta `C:\Users\laila\Desktop\practica>` | [02-carpeta-practica.png](captures/02-carpeta-practica.png) |
| 2 | `echo hola > nota.txt`, `dir` | `nota.txt` de 7 bytes | [03-nota-txt.png](captures/03-nota-txt.png) |
| 3 | `copy nota.txt copia.txt`, `dir` | `copia.txt` i `nota.txt`, 7 bytes cadascun | [04-copia-txt.png](captures/04-copia-txt.png) |
| 4 | `mkdir arxiu`, `move copia.txt arxiu`, `dir` | `copia.txt` mogut dins de `arxiu` | [05-moure-arxiu.png](captures/05-moure-arxiu.png) |
| 5 | `ren nota.txt notes.txt`, `ren arxiu backup`, `dir` | Fitxer i carpeta renombrats | [06-renombrar.png](captures/06-renombrar.png) |
| 6 | `cd backup`, `dir`, `cd ..` | El primer `cd backup` ha fallat: la carpeta havia quedat com `buckup` per una errada d'escriptura. Corregit amb `ren buckup backup`; després `copia.txt` és dins de `backup` | [07-error-cd-backup.png](captures/07-error-cd-backup.png), [08-dir-x-buckup.png](captures/08-dir-x-buckup.png), [09-cd-backup-ok.png](captures/09-cd-backup-ok.png) |
| 7 | `tree /f` | `notes.txt` i `backup\copia.txt` | [10-tree.png](captures/10-tree.png) |
| 8 | `del notes.txt`, `dir` | Només queda `backup` | [11-del-notes.png](captures/11-del-notes.png) |
| 9 | `cls` | Pantalla netejada | — |

---

## 4. Aplicacions i punts de restauració

**Punt de restauració: creat correctament.**

- Protecció del sistema activada a `DADES (C:)` ([captura](captures/12-proteccio-sistema.png), [pestanya](captures/13-pestanya-proteccio.png)).
- El primer intent amb el límit d'ús al mínim ha fallat amb `0x8004231F` (espai insuficient per al fitxer d'instantànies) ([captura](captures/15-error-espai-8004231F.png)).
- Un segon intent ha fallat amb `0x80070070` (espai en disc insuficient) ([captura](captures/16-error-espai-80070070.png)).
- Amb el límit d'ús a **1,40 GB (7%)** s'ha creat el punt `Abans de Chrome` ([captura](captures/18-limit-1-40gb.png), [confirmació](captures/19-punt-creat-ok.png)).

**Chrome: no instal·lat.**

- He descarregat `ChromeSetup.exe` des de la web oficial de Google ([captura](captures/20-descarrega-chrome.png)) i he acceptat el Control de comptes d'usuari (editor verificat: Google LLC) ([captura](captures/21-uac-google.png)).
- La instal·lació ha fallat amb «No hi ha prou espai en disc» ([captura](captures/22-chrome-sense-espai.png)).
- L'eina Liberador d'espai només ha pogut alliberar uns 25 MB ([captura](captures/23-liberador-espai.png)).
- Abans d'això, l'emmagatzematge mostrava **19,9 GB ocupats i 67,4 MB lliures** a `DADES (C:)`.

**Passos 3 i 4 (Restaurar sistema i Cerca programes afectats): no fets**, perquè Chrome no s'ha pogut instal·lar.

**Causa:** la unitat virtual era massa petita per a les instantànies de restauració i per a la instal·lació. La solució seria ampliar el disc virtual des de VirtualBox i després estendre la unitat a Gestió de discos.

---

## 5. Xarxa

Adaptador de la VM canviat segons el pas. Abans de començar, s'ha fet una instantània de la VM anomenada `Abans de xarxa`.

| Pas | Configuració | Resultat |
|---|---|---|
| 1 | Adaptador **NAT** | `ipconfig`: `10.0.2.15`. `ping google.com` i `ping 8.8.8.8` funcionen (0% pèrdua, ~12 ms) |
| 2 | NAT amb **Connecta el cable** desmarcat | `ping google.com`: "La sol·licitud de ping no pogué trobar el host google.com". `ping 8.8.8.8`: "Host de destino inaccesible" des de `10.0.2.15` |
| 3 | **Adaptador pont** (`enp0s31f6`), cable connectat | `ipconfig`: `192.168.203.195`, porta d'enllaç `192.168.203.1`, sufix `insebre.org`. `ping google.com` (216.58.205.174) i `ping 8.8.8.8` funcionen (0% pèrdua) |
| 4 | Adaptador pont amb **DNS manual `1.2.3.4`** | `ping google.com`: no troba el host (error de resolució de noms). `ping 8.8.8.8` funciona (0% pèrdua) |
| 4 (final) | DNS tornat a **automàtic** | `ping google.com` resol de nou (142.251.142.142) |


---

## Captures per pas (segons la guia)

Les captures de cada pas són a [`captures/por-pas/`](captures/por-pas/), amb el número de part i de pas. Les captures sueltes de la secció 3 i 4 també hi són a [`captures/`](captures/). Les captures addicionals de la guia són a [`captures/extra-del-pdf/`](captures/extra-del-pdf/).

| Pas | Captura principal |
|---|---|
| 1.1 Crear la màquina | [1.1-vm-nom-i-ram.png](captures/por-pas/1.1-vm-nom-i-ram.png) |
| 1.2 Adaptador NAT | [1.2-adaptador-nat.png](captures/por-pas/1.2-adaptador-nat.png) |
| 1.4 Particions | [1.4-particions-30-20gb.png](captures/por-pas/1.4-particions-30-20gb.png) |
| 1.5 Volum DADES | [1.5-diskmgmt-dades.png](captures/por-pas/1.5-diskmgmt-dades.png) |
| 2.1 Activació | [2.1-activacio.png](captures/por-pas/2.1-activacio.png) |
| 2.2 slmgr /xpr i /dli | [2.2-slmgr-dli.png](captures/por-pas/2.2-slmgr-dli.png) |
| 2.3 Preu Windows 11 | [2.3-preu-windows-11-pro.png](captures/por-pas/2.3-preu-windows-11-pro.png) |
| 2.4 CAL | [2.4-cal.png](captures/por-pas/2.4-cal.png) |
| 3.1 cd Desktop, mkdir practica | [3.1-cd-desktop-mkdir.png](captures/por-pas/3.1-cd-desktop-mkdir.png) |
| 3.2 echo hola > nota.txt | [3.2-echo-hola.png](captures/por-pas/3.2-echo-hola.png) |
| 3.3 copy | [3.3-copy.png](captures/por-pas/3.3-copy.png) |
| 3.4 mkdir arxiu, move | [3.4-mkdir-move.png](captures/por-pas/3.4-mkdir-move.png) |
| 3.5 ren | [3.5-ren.png](captures/por-pas/3.5-ren.png) |
| 3.6 cd backup | [3.6-cd-backup.png](captures/por-pas/3.6-cd-backup.png) |
| 3.7 tree /f | [3.7-tree.png](captures/por-pas/3.7-tree.png) |
| 3.8 del notes.txt | [3.8-del-notes.png](captures/por-pas/3.8-del-notes.png) |
| 4.1 Punt de restauració creat | [4.1-punt-restauracio-creat.png](captures/por-pas/4.1-punt-restauracio-creat.png) |
| 4.2 Emmagatzematge (Chrome no s'instal·la) | [4.2-emmagatzematge-chrome.png](captures/por-pas/4.2-emmagatzematge-chrome.png) |
| 5.1 NAT: ipconfig | [5.1-ipconfig-nat.png](captures/por-pas/5.1-ipconfig-nat.png) |
| 5.2 Cable desconnectat: ping | [5.2-ping-cable-desconnectat.png](captures/por-pas/5.2-ping-cable-desconnectat.png) |
| 5.3 Adaptador pont: ipconfig | [5.3-ipconfig-pont.png](captures/por-pas/5.3-ipconfig-pont.png) |
| 5.4 DNS 1.2.3.4 | [5.4-dns-manual-1234.png](captures/por-pas/5.4-dns-manual-1234.png) |

**Passos sense captura a la guia:** 1.3, 4.3 i 4.4 (Restaurar sistema i Cerca programes afectats no s'han pogut fer, perquè Chrome no s'ha instal·lat). El pas 9 de la secció 3 (`cls`) tampoc té captura.

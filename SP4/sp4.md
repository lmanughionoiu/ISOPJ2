---
layout: default
title: "Sprint 4: Configuració del Programari de Base i Sistemes d’Emmagatzematge en Ubuntu"
---

# RAIDS

## Què és un RAID?

RAID són les sigles de ***Redundant Array of Independent Disks*** (Matriu Redundant de Discs Independents). És una tecnologia de virtualització d'emmagatzematge que combina múltiples discos durs físics en una o diverses unitats lògiques.

## Per a què serveix i quins beneficis aporta?

L'objectiu principal d'un RAID es resumeix en tres grans beneficis:

- **Redundància:** És la capacitat del sistema per sobreviure a la fallada mecànica o lògica d'un (o més) discos sense perdre dades. Si un disc es trenca, el sistema pot continuar funcionant i les dades es poden reconstruir en un disc nou.

- **Rendiment:** Al tenir múltiples discos llegint i escrivint dades al mateix temps (en paral·lel), la velocitat de transferència (lectura/escriptura) augmenta dràsticament.

- **Capacitat:** Permet sumar la capacitat de discos petits per crear una única unitat d'emmagatzematge massiva.

## Els nivells de RAID més coneguts

Hi ha diferents configuracions de RAID, cadascuna amb un equilibri diferent entre velocitat, capacitat i seguretat. S'anomenen "nivells". En aquest cas, només explicaré el RAID 1 que és el que farem servir de prova. Per a sapiguer més dels nivells dels raids, la següent pàgina conté tot el necessari: https://es.wikipedia.org/wiki/RAID.

### RAID 1 (Mirroring o Mirall)

***Com funciona:*** Duplica exactament les mateixes dades en dos (o més) discos. Tot el que s'escriu al disc A, s'escriu simultàniament al disc B.

***Beneficis:*** Redundància excel·lent. Si un disc falla, el sistema continua funcionant amb l'altre de forma transparent. Bona velocitat de lectura.

***Desavantatges:*** Es perd la meitat de la capacitat. Si tens dos discos de 1TB en RAID 1, només tens 1TB d'espai real utilitzable. L'escriptura no és més ràpida que la d'un sol disc.

***Mínim de discos:*** 2.

### Què són els Volums?

Per entendre què és un volum, has de pensar en com el sistema operatiu gestiona l'espai físic:

- **Disc Físic:** És el maquinari real (el disc HDD o SSD que toques amb les mans).

- **Partició:** És una divisió lògica d'un disc físic. Pots dividir un disc físic en diverses "seccions".

- **Volum (Volume):** És una àrea d'emmagatzematge accessible que té un sistema d'arxius únic (com NTFS, exFAT, APFS o ext4) i que el sistema operatiu reconeix com a una unitat útil (a Windows, les famoses lletres C:, D:, etc.).

### Quina és la relació entre Volums i RAID?

Un volum no està obligat a residir en un sol disc dur. Gràcies al maquinari o programari de control, un Volum Lògic pot abastar diversos discos durs físics. Quan muntes un RAID 5 amb quatre discos físics, el sistema operatiu no veu quatre discos; veu un sol Volum massiu llest per ser formatat i per guardar arxius.

# Pràctica RAID 1

Instalem primerament el paquet `mdadm`.

![alt text](image.png)

Abans de iniciar la MV hem afegit 2 discs de 2GB cada un.

![alt text](image-1.png)

Creem les particions als dos discs.

![alt text](image-2.png)

![alt text](image-3.png)

Quedaría així després de crear la partició.

![alt text](image-4.png)

Crearem el directori on farem el raid.

![alt text](image-5.png)

Creem el raid.

![alt text](image-6.png)

**--create /dev/md0:** Creació del raid.

**--level=1:** Nivell del raid.

**--raid-devices=2 /dev/sdb1 /dev/sdc1:** Escollim quants dispositius volem per al raid i quins són.

Ara formatem el raid que hem creat en ext4.

![alt text](image-7.png)

En aquesta comanda podem veure l'estat del raid i quins dispositius estan.

![alt text](image-8.png)

Redireccionem la sortida que ens dona la primera comanda al arxiu mdadm.conf.

![alt text](image-9.png)

Ara fem un nano del fitxer i afegim la següent línea per afegir els dispositius que hem ficat amb anterioritat.

![alt text](image-10.png)

Per a que quan reiniciem el ordinador la carpeta que hem ficat a /mnt/ no es pergui, l'afegim al fitxer fstab.

![alt text](image-11.png)

Ara montem i actualitzem el kernel.

![alt text](image-12.png)

Ara fem un reboot i fem la següent comanda per veure si segueixen els discos, si estàn, vol dir que el raid funciona correctament.

![alt text](image-13.png)

## Prova funcionament RAID 1

Creem els següents fitxers i directoris dins de la carpeta muntada de raid1 que hem creat anteriorment.

![alt text](image-14.png)

Ara fem fallar el disco i el traem.

![alt text](image-15.png)

![alt text](image-16.png)

Ara mirem i veem que ja està tret i que només funciona un.

![alt text](image-17.png)

Tot i tenint un funcionant i l'altre tret, hem de poder seguir treballant i creant arxius sobre la carpeta.

![alt text](image-18.png)

Ara tornem a afegir el disc.

![alt text](image-19.png)

Ara que l'hem afegit, podem veure que ja està actiu.

![alt text](image-20.png)

Ara si tornem a entrar al directori que hem fet de /raid1, veem que segueix los directoris i fitxers que hem afegit abans quan només teniem 1 disc.

![alt text](image-21.png)
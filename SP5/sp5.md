---
layout: default
title: "Sprint 5: Monitoratge, Auditories i Programari Client/Servidor"
---

# Monitoratge del sistema

El monitoratge del sistema en Ubuntu consisteix a observar i mesurar l'ús dels recursos del teu ordinador o servidor en temps real. És fonamental per saber si l'equip està bé o si està a punt de saturar-se.

Les eines de monitoratge simplement llegeixen aquestes dades complexes en temps real i les tradueixen a números, llistes o gràfics fàcils d'entendre.

![alt text](image-29.png)

Al obrir la aplicació veem tots els processos que tenim oberts. Com hem vist en un dels Sprints anteriors, és el mateix que per terminal amb HTOP, ETOP, BTOP, etc.

![alt text](image-30.png)

Com podem veure, podem matar un procés, tancarlo, establir l'afinitat, la prioritat, etc. Aquests punts ja els hem donat amb anterioritat.

![alt text](image-33.png)

![alt text](image-34.png)

Ademés, podem veure, com he dit abans, el monitoratge de tots els recursos del ordinador.

Tenim el següent:

**CPU:** Mostra la càrrega de feina del processador de l'ordinador. Si està contínuament al 100%, el sistema anirà lent perquè no dona l'abast processant tasques.

**RAM:** És l'espai de treball on s'executen els programes oberts. Si la RAM s'omple, Ubuntu haurà de fer servir una part del disc dur com a RAM d'emergència (Swap), cosa que alenteix dràsticament l'equip.

**Xarxa:** Mesura quantes dades entren i surten del teu equip, a més de vigilar les connexions actives o ports oberts.

**Disc:** Controla dues coses: l'espai lliure restant i la velocitat de Lectura/Escriptura. Un disc treballant al màxim crea colls d'ampolla.

![alt text](image-31.png)

![alt text](image-32.png)

# Logs - Iker i Manu

![Imatge 0](images/image.png)

Per a veure els logs fem un cat del arxiu `syslog` i podem veure tots els registres del sistema.

![Imatge 1](images/image-1.png)

En aquest directori podem presonalitzar la rotació dels logs.

![Imatge 2](images/image-2.png)

Aquest fitxer el podem modificar per personalitzar la rotació dels logs.

![Imatge 3](images/image-3.png)

En aquest fitxer ens diu on hem de anar per modificar el fitxer default dels logs, anirem alli ara.

![Imatge 4](images/image-4.png)

Primerament, fem una prova per veure com afecta una notificació `mail` i en quins arxius apareix.

Fem una simulació per a veure si apareix el log al moment d'enviar una notificació de mail i si després apareix al arxiu de mail.log.

En una terminal enviem la notificació amb el missatge i a l'altra obrim el syslog en directe, per a veure els canvis al moment.

![Imatge 6](images/image-6.png)

Si mirem l'arxiu `mail.log`, s'ha guardat aquesta notificació al arxiu.

![imatge 8](images/image-8.png)

Farem una altra prova modificant el `mail`. Volem que ens guardi al `mail.log` els logs de error només, ni nivells inferiors ni superiors a aquest. Si fiquem un `*`, apareixerien tots i es guardarà com abans.

![Imatge 5](images/image-5.png)

Quan el modifiquem, hem de fer un restart al servei.

![Imatge 7](images/image-7.png)

Ara tornem a fer la comanda anterior i veem que a l'arxiu `mail.log` no apareix, ja que el que enviem és un `mail.notice` i aquest no és tipus `err`.

![Imatge 9](images/image-9.png)

Finalment, si en comptes de un `mail.notice` fiquem `mail.err` si que ens ho guardarà al arxiu dels logs.

![Imatge 10](images/image-10.png)

Ara modifiquem una altra vegada el tipus de logs que volem guardar de tipus `mail`. En aquest cas traem el =. Això el que farà serà guardar tots els logs de tipus err i superiors.

![Imatge 11](images/image-11.png)

Reiniciem el servei i seguim.

Per fer una prova d'aquest funcionament, cambiem el tipus d'alerta a `crit` que és un tipus superior i veem que el guarda.

![Imatge 12](images/image-12.png)

Podem crear una ruta personalitzada per guardar els logs que més ens interessa, en aquest cas, el que fem és que volem guardar tot tipus de logs que siguin `crit` i li afegim el directori on volem que es guardi i reiniciem el servei.

![Imatge 13](images/image-13.png)

Ara enviem la notificació, en aquest cas de `cron.crit` i veem que s'ha creat un nou arxiu anomenat mireia.log, que ho hem especificat al arxiu anterior.

![Imatge 14](images/image-14.png)

Amb aquesta comanda podem veure tots els logs de tipus `crit`.

![Imatge 15](images/image-15.png)

Podem trobar entre elles els que hem fet abans.

![Imatge 16](images/image-16.png)

Depenent del paràmetre que li afegim, podem filtrar les búsquedes. En aquest cas, volem mirar els que hem enviat abans de tipus mail.

![Imatge 17](images/image-17.png)

## Exercici Logs

Necessitem dos màquines ubuntu, una té que enviar els logs a l'altra màquina i guardar-ho al seu propi sistema i l'altra màquina els ha de rebre.

### Màquina Servidor

Primerament, a la màquina Servidor, que és la que ha de rebre el log i guardar-lo, hem de fer el següent:

Creem un fitxer nou per a poder redireccionar tots els logs remots a una carpeta nova, que crearem també ara.

![Imatge 21](images/image-21.png)

![Imatge 19](images/image-19.png)

Al fitxer que creem nou hem de afegir aquestes línies que es mostra, per a poder rebre els logs per UDP i/o TCP.

![alt text](image-15.png)

Finalment, permitim el pas de tcp i udp al firewall.

![Imatge 23](images/image-23.png)

![Imatge 24](images/image-24.png)

Una vegada hem fet ja aquests passos, hem de reiniciar el servei.

![Imatge 22](images/image-22.png)

### Màquina Client

Afegim la ip del servidor a un nou fitxer 90-forward.conf.

![alt text](image-16.png)

Fem un restart de rsyslog.

![alt text](image-17.png)

#### Comprobació logger

Enviem un logger des del client.

![alt text](image-18.png)

Veem que s'han creat arxius dins de la carpeta remote i tenim la carpeta de ClientSP5.

![alt text](image-19.png)

Veem que si entrem dins del directori creat, esta el log i si comprobem veurem el log que hem enviat.

![alt text](image-20.png)

# Servidor d'actualitzacions

Tenir un servidor d'actualitzacions en una xarxa amb diversos equips Ubuntu és clau per aquests motius principals:

1. **Estalvi radical d'ample de banda:** Si tenim 50 ordinadors, una actualització pesada es baixa d'Internet només una vegada al servidor. La resta d'equips la descarreguen d'ell a velocitat de xarxa local.

2. **Control i prevenció d'errors:** Pots aturar actualitzacions problemàtiques o provar-les primer en 2 o 3 equips "pilot". Si tot funciona bé, les aproves per a la resta, evitant que una fallada afecti tota l'empresa.

3. **Visió global de la seguretat:** Et permet controlar en un sol lloc quins equips tenen els últims pedaços instal·lats, quins donen error o quins necessiten un reinici urgent.

4. **Equips sense Internet:** És l'única manera d'actualitzar servidors crítics que, per seguretat, no tenen accés directe a la xarxa exterior.

## Servidor

Instalem apache i apt-mirror al servidor.

![Imatge](image.png)

![Imatge](image-1.png)

Entrem al fitxer `mirror.list` i comentem totes les linies i afegim el nostre paquet que volem instalar.

![Imatge](image-2.png)

Executem la comanda `apt-mirror` per a instalar el paquet que li hem donat.

![Imatge](image-3.png)

Ara comprovem si s'ha instalat i l'enviem al apache.

![Imatge](image-4.png)

## Client

A la màquina client, entrem al fitxer `sources.list` i afegim el repositori, pero al enllaç simbòlic que hem creat al server.

![Imatge](image-5.png)

Hem de fer aquest pas abans de instalar el paquet, ja que primerament s'ha de signar.

![Imatge](image-6.png)

Ara si fem un apt update al client, veurem com agafara un dels repositoris a la màquina servidor.

![Imatge](image-7.png)

I com volem instalar aquest paquet, l'instalem i vorem que ho farà des del repositori del servidor.

![Imatge](image-8.png)

## Exercici

### Servidor

He elegit l'aplicació anydesk, ja que és el que menys recursos té per instalar-ho rapidament i al servidor afegim el repositori.

![alt text](image-21.png)

Fem un `apt-mirror` i veem que descarrega ja el paquet.

![alt text](image-22.png)

Enviem el paquet a apache.

![alt text](image-23.png)

### Client

A la màquina client, entrem al fitxer `sources.list` i afegim el repositori, pero al enllaç simbòlic que hem creat al server.

![alt text](image-24.png)

Hem de fer aquest pas abans de instalar el paquet, ja que primerament s'ha de signar.

![alt text](image-25.png)

Ara fem un apt update i veem que s'ha connectat per agarrar el repositori.

![alt text](image-26.png)

Finalment ja podem instalar el paquet i veure que ho fa des del servidor.

![alt text](image-27.png)

Ara ja el podem fer servir.

![alt text](image-28.png)


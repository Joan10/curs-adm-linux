Sistemes de gestió de paquets
=============================

Cal introduir un cert ordre en la forma en què el programari
s’instal·la, s’actualitza i s’elimina en els sistemes Linux. Els
sistemes de gestió de paquets proveeixen una via neta d’aconseguir
aquesta meta i pot evitar que els sistemes caiguin en un caos, que es
tornin vells o es trenquin amb el temps. A més, proveeixen un mètode per
verificar que el programari en el sistema no ha estat corromput, ja
sigui maliciosament o per accident.

Una funció essencial dels distribuïdors de Linux és desenvolupar,
mantenir els paquets i assegurar que les dependències són mantingudes
apropiadament amb el temps.

Conceptes d’empaquetament de software
-------------------------------------

Els sistemes de gestió de paquets proveeixen les eines que els permeten
als administradors de sistemes automatitzar la instal·lació,
actualització, configuració i eliminació de paquets de programari, tot
això d’una forma coneguda, predictible i consistent. Aquests sistemes
permeten:

-  Reunir i comprimir fitxers associats a un sol paquets

-  Permeten que la instal·lació i esborrat de software sigui fàcil

-  Permeten verificar la integritat d’un fitxer a través d’una BBDD
   interna

-  Poden autenticar l’origen dels paquets

-  Faciliten les actualitzacions

-  Agrupa els paquets amb característiques lògiques

-  Administra dependències

Un paquet pot contenir arxius executables, arxius de dades,
documentació, scripts d’instal·lació i arxius de configuració. També
s’inclouen atributs de metadades com ara nombre de versió, checksums,
informació del proveïdor, dependències, descripcions, etc.

Després de la instal·lació, tota la informació s’emmagatzema localment
en una base de dades interna, la qual pot consultar de forma convenient
per obtenir informació de l’estat i la versió.

Perquè emprar paquets?
----------------------

Els sistemes de gestió de paquets de software són un dels grans
avantatges de Linux i són uns dels causants de que els entorns
empresarials d’IT emprin Linux. Els motius són:

-  Automatització: no calen instal·lacions o actualitzacions manuals

-  Escalabilitat: faciliten la instal·lació a un o 10.000 sistemes

-  Repetibilitat i predictibilitat

-  Seguretat i auditoria

Tipus de paquets
----------------

#. Paquest **binaris**: contenen fitxers llestos per ser instal·lats.
   Inclouen executables i llibreries. Depenen de l’arquitectura i han de
   ser precompilats per cada tipus de màquina.

#. Paquets de **codi font**: s’empren per generar paquets binaris.
   Sempre s’hauria de tenir la possibilitat de reconstruir un paquet
   binari (amb ``rpmbuild --rebuild``, per exemple) des del codi font.
   Un paquet de codi font pot emprar-se a moltes arquitectures.

#. Els paquets **independents de l’arquitectura** contenen fitxers i
   scripts que s’executen baix intèrprets d’script, així com també
   documentació i fitxers de configuració.

#. Els **metapaquets** són grups de paquets associats que recopilen tot
   allò necessari per instal·lar un subsistema relativament gran (entorn
   d’escriptori, suite d’oficina, etc.)

Els administradors generalment empren paquets binaris la majoria del
temps.

En sistemes de 64 bits que poden executar programes de 32 bits, es
possible tenir paquets de les dues arquitectures. Generalment al nom
d’aquests paquets se’ls hi acopla un sufix com **x86_64** o **amd64** en
64 bits i **i386** o **i686** en 32.

Els paquets de codi font poden ser útils per mantenir un seguiment dels
canvis i el codi emprat que ve en els paquets binaris. Generalment no
estan instal·lats per defecte a un sistema però sempre poden ser
obtinguts des del proveïdor de la distribució.

Sistemes disponibles de gestió de paquets
-----------------------------------------

#. **RPM (Red Hat Package Manager)**. Emprat en distribucions derivades
   de Red Hat, com ara RHEL, Centos, Oracle Linux. També per
   distribucions de la família **SUSE** i OpenSUSE.

#. **dpkg (Debian Package)**. Emprat per Debian i les distribucions
   derivades com Ubuntu o Linux Mint.

Hi ha altres sistemes de gestió de paquets: **portage/emerge** en
Gentoo, **pacman** en Arch, i alguns especialitzats emprats en sistemes
Linux Embedded i Android.

La distribució **Slackware**, una de les més antigues, no fan servir cap
sistema de gestió de paquets. Per la contra, simplement proveeixen els
paquets amb fitxers **tar** comprimits.

En aquest curs ens centrarem, sobretot, en RPM i dpkg perquè són els més
emprats.

Nivells d’empaquetament i varietats d’eines
-------------------------------------------

Hi ha dos nivells principals d’empaquetament:

-  **Eines de baix nivell**: simplement instal·la i elimina un paquet
   únic o una llista de paquets on cada un té el seu nom de forma
   individual i específica. Les dependències no es gestionen
   completament, i només s’adverteix de:

   -  Si un altre paquet ha de ser instal·lat primer, la instal·lació
      fallarà

   -  Si el paquet és dependència d’un altre, la seva eliminació també
      fallarà.

   Eines com rpm o dpkg fan aquest rol.

-  **Eines d’alt nivell**: resol els problemes de les dependències.

   -  Si un altre paquet o grup de paquets necessita ser instal·lat
      abans l’eina els instal·larà.

   -  Si l’eliminació d’un paquet interfereix amb un altre que ja està
      instal·lat, l’administrador podrà triar si avortar l’operació o
      eliminar tot el programari afectat.

   Les eines **yum** i **Zypper** (i més recentment **PackageKit**) es
   fan càrrec de la resolució de dependències per a sistemes amb RPM.
   D’altra banda, **apt-get**, **apt-cache** i altres eines es fan
   càrrec en sistemes amb dpkg.

En aquest curs discutirem només els programes de sistemes de gestió de
paquets de CLI. Si bé és cert que les interfícies gràfiques usades per
cada distribució Linux poden ser útils, és preferible estar menys lligat
a una d’elles i també tenir més flexibilitat.

Fonts de paquets
----------------

Cada distribució té un o més repositoris de paquets des d’on les eines
del sistema obtenen programari i actualitzacions. És treball de la
distribució **assegurar que tots els paquets en els repositoris
interactuen bé** entre ells.

Hi ha altres repositoris externs que poden agregar-se a la llista de
fonts que suporta la distribució. De vegades estan estretament
associades a la distribució i rarament generen problemes importants. Un
exemple pot ser **EPEL** (Extra Packages for Enterprise Linux), un
conjunt de repositoris dependents de la versió de la distribució que
encaixen bé amb RHEL. La seva font és Fedora i els mantenidors d’aquesta
són propers a Red Hat.

No obstant això, alguns repositoris externs no estan molt ben construïts
o mantinguts. Per exemple, quan un paquet s’actualitza en el repositori
principal, les dependències podrien no estar actualitzades en el
repositori extern, el qual podria conduir a problemes entre dependències
de paquets.

Creació de paquets de software
------------------------------

Construir paquets de programari personalitzat facilita distribuir i
instal·lar el seu propi programari. Gairebé totes les versions de Linux
tenen algun mecanisme per realitzar-ho.

Construir el seu propi paquet permet controlar exactament què va en el
programari i com s’instal·la. Podeu crear el paquet de tal manera que
quan s’instal·li s’executin scripts per realitzar totes les tasques
necessàries per tal d’instal·lar el nou programari i/o eliminar l’antic.
Aquestes tasques inclouen:

-  Creació dels enllaços simbòlics necessaris.

-  Creació de directoris segons sigui necessari.

-  Gestió dels permisos.

-  Qualsevol cosa que pugui incloure en un script i que compleixi una
   tasca específica.

No discutirem els mecanismes sobre com construir paquets .rpm o .deb, ja
que aquest és un assumpte que afecta més a desenvolupadors de programari
que a administradors.

Sistemes de control de versions
-------------------------------

Els projectes de programari es tornen més complexos de gestionar a
mesura en què el projecte es desenvolupa i creix. També quan el nombre
de desenvolupadors que treballen augmenta.

Hi ha molts sistemes disponibles de control de versions per organitzar
les actualitzacions i facilitar la col·laboració. Algunes de les
característiques estàndard d’aquests programes inclouen l’habilitat per
mantenir una història exacta (registre) dels canvis, ser capaç de tornar
a versions anteriors, coordinar possibles conflictes d’actualització des
de més d’un desenvolupador, etc. La taula
`[tau_ctrl_vers] <#tau_ctrl_vers>`__ mostra els sistemes de control de
versions disponibles.

.. table:: Sistemes de control de versions

   ============ ==========================================
   **Producte** **URL**
   RCS          http://www.gnu.org/software/rcs
   CVS          http://ximbiot.com/cvs/wiki
   Subversion   http://subversion.tigris.org
   git          http://www.kernel.org/pub/software/scm/git
   GNU Arch     http://www.gnu.org/software/gnu-arch
   Monotone     http://www.monotone.ca
   Mercurial    http://mercurial.selenic.com
   PRCS         http://prcs.sourceforge.net
   ============ ==========================================

[tau_ctrl_vers]

Git
~~~

Git és el sistema més emprat i es va originar en la comunitat de
desenvolupament del Kernel de Linux. Git ha aconseguit una posició
dominant en l’ús de projectes open source en poc temps i també s’empra
en desenvolupaments tancats.

El sistema de desenvolupament del nucli Linux té necessitats especials a
causa de que es realitza de forma distribuïda a través del món, amb
literalment milers de desenvolupadors involucrats. A més, tota la feina
es fa de forma pública i sota la llicència GPL. Per molt de temps no va
existir un sistema de control de versions real. Llavors els principals
desenvolupadors del nucli es van passar a l’ús de BitKeeper (veure
http://www.bitkeeper.com), un projecte comercial que va cedir una
llicència d’ús restringit per al desenvolupament del nucli Linux. No
obstant això, en una discussió pública sobre les restriccions de la
llicència a la primavera del 2005, l’ús lliure de BitKeeper va ser
revocat per al desenvolupament del nucli Linux.

Tècnicament **git** no és un sistema de control de versions en el sentit
usual, i les unitats bàsiques amb les quals treballa no són arxius.
Compta amb dues estructures de dades importants: una base de dades
d’objectes i un cau de directoris.

La base de dades d’objectes conté objectes de tres tipus:

-  **Blobs**: Trossos de dades binaris que tenen el contingut de
   l’arxiu.

-  **Trees**: Conjunts de blobs que inclouen noms d’arxius i atributs,
   proveint l’estructura dels directoris.

-  **Commits**: Canvis que descriuen les instantànies dels arbres.

La caché del directori captura l’estat de l’arbre de directoris.

En no emprar el mecanisme de control d’un sistema basat en arxius s’està
més preparat per gestionar conjunts de canvis.

Git està contínuament sota un desenvolupament ràpid i hi ha interfícies
gràfiques que també tenen un ritme similar. Per exemple, vegeu
https://git.kernel.org. Aquí és possible navegar fàcilment pels canvis
específics, com també pels arbres de codi font.

Llocs com ara http://www.github.com o https://gitlab.com s’allotgen
literalment milions de repositoris git, tant públics com privats. Hi ha
una gran quantitat d’articles, llibres, tutorials en línia, etc. que es
poden trobar fàcilment pel que fa a com treure-li profit a git.

EXERCICI PRÁCTIC - Git
~~~~~~~~~~~~~~~~~~~~~~

Vegeu el document Lab1-Git.pdf.

RPM
===

El **Red Hat Package Manager (RPM)** és usat per les distribucions
principals de Red Hat (i les seves derivades) per controlar la
instal·lació, verificació, actualització i eliminació de programari en
sistemes Linux. El programa de baix nivell **rpm** pot realitzar totes
aquestes operacions, ja sigui en un sol paquet o en una llista d’ells.
Les operacions que podrien causar problemes no es poden completar amb
**rpm**, com ara esborrar un paquet del qual en depèn un altre o
instal·lar un paquet sense haver satisfet les dependències amb
anterioritat.

**RPM (Redhat Package Manager)** va ser desenvolupat per Red Hat. Tots
els arxius relacionats a una tasca específica s’empaqueten en un arxiu
rpm únic, el qual també conté informació sobre com i on instal·lar o
desinstal·lar els arxius. Les versions noves de programari condueixen a
arxius rpm nous, els que es fan servir per a l’actualització.

Els arxius rpm també contenen informació sobre les dependències.
Recordau que a menys que s’especifiqui un URL per obtenir els paquets,
rpm no anirà a buscar els paquets a Internet, sinó que més aviat els
instal·larà des de la màquina local només, usant rutes absolutes o
relatives per a tal efecte.

Els arxius rpm generalment depenen de la distribució. De fet, instal·lar
un paquet en una distribució diferent per a la qual va ser creat podria
ser difícil o impossible.

Avantatges d’RPM
----------------

**RPM** facilita les següents tasques als administradors:

-  Determinar a quin paquet pertany un fitxer del sistema (si és el
   cas).

-  Determinar quina versió està instal·lada.

-  Instal·lar i desinstal·lar paquets sense deixar residus o caps per
   lligar.

-  Verificar que un paquet es va instal·lar correctament; això és útil
   tant per solucionar problemes com per a l’auditoria del sistema.

-  Distingir els arxius de documentació de la resta dels arxius del
   paquet, i opcionalment decidir no instal·lar-los per estalviar espai
   en disc.

-  Utilitza FTP o HTTP per a instal·lar paquets des d’internet.

**RPM** ofereix també avantatges per als desenvolupadors:

-  El programari sovint està disponible en més d’un sistema operatiu.
   Amb RPM, la font original s’empra com a base, però un desenvolupador
   pot incloure informació per construir-lo (compilar-lo) en Linux.

-  Emprant un sol paquet de codi font és possible construir un paquet
   binari per més d’una arquitectura.

Nomenclatura dels paquets
-------------------------

Els noms dels paquets **RPM** estan basats en camps que representen
informació específica, tal i com es documenta a l’estàndard RPM.

-  El format de nom estandaritzat per un paquet binari és:

   ::

      <name>-<version>-<release>.<distro>.<architecture>.rpm
      sed-4.2.1-10.el6.x86_64.rpm

-  El format de nom estandaritzat per un paquet de codi font és:

   ::

      <name>-<version>-<release>.<distro>.src.rpm
      sed-4.2.1-10.el6.src.rpm

Cal tenir en compte que el camp **distro** especifica el repositori del
qual prové el paquet, donat que una instal·lació pot emprar varis
repositoris de paquets com discutirem en properes seccions.

Directori de la base de dades
-----------------------------

``/var/lib/rpm`` és el directori per defecte en el sistema on hi ha la
base de dades **RPM**. Els fitxers de la base de dades no s’hauríen de
modificar manualment i les actualitzacions hauríen de realitzar-se
solament amb el programa **rpm**.

Es possible especificar un directori alternatiu per a la base de dades
amb **–dbpath** de **rpm**. Per exemple, això pot ser útil per examinar
una base de dades d’un altre sistema.

Es pot emprar l’opció **–rebuilddb** per reconstruir els índexos de la
base de dades des dels headers dels paquets instal·lats. Això és per
reparar més que per reconstruir.

Programes auxiliars de RPM
--------------------------

RPM inclou varis scripts i programes auxiliars a ``/usr/lib/rpm``.

Així mateix, és possible crear un fitxer rpmrc per especificar ajustos
per a rpm. Per defecte, se segueix aquest ordre:

#. ``/usr/lib/rpm/rpmrc``

#. ``/etc/rpmrc``

#. ``~/.rpmrc``

Tingueu en compte que es llegeixen tots aquest fitxers. RPM no se deté
al primer que troba. Es pot emprar l’opció **–rcfile** per seleccionar
un fitxer rpmrc diferent.

Consultes
---------

Totes les consultes **rpm** inclouen la opció -q, la qual pot ser
combinada amb altres subopcions:

-  Consultar versió d’un paquet: ``$ rpm -q bash``

-  De quin paquet prové aquest fitxer: ``$ rpm -qf /bin/bash``

-  Fitxers que va instal·lar el paquet: ``$ rpm -ql bash``

-  Informació sobre el paquet: ``$ rpm -qi bash``

-  Informació sobre el paquet des del fitxer del paquet (i no des de la
   base de dades): ``$ rpm -qip foo-1.0.0.1.noarch.rpm``

-  Llista tots els paquets del sistema: ``$ rpm -qa``

Unes opcions útils són **–requires** i **–whatprovides**

-  Entra una llista dels prerequisits:
   ``rpm -qp --requires foo-1.0.0-1.noarch.rpm``

-  Mostra quin paquet instal·lat proveeix un fitxer en particular:
   ``rpm -q --whatprovides libc.so.6``

Verificar paquets
-----------------

L’opció -V de rpm permet verificar si els fitxers d’un paquet en
particular són consistents amb la base de dades RPM del sistema.

Per verificar tots els paquets del sistema:

::

    $ rpm -Va
   missing   /var/run/pluto
   ....
   S.5....T. c /etc/hba.conf
   S.5....T. /usr/share/applications/defaults.list
   ....L.... c /etc/pam.d/fingerprint-auth
   ....L.... c /etc/pam.d/password-auth
   ....
   .M....... /var/lib/nfs/rpc_pipefs
   ....
   .....UG.. /usr/local/bin
   .....UG.. /usr/local/etc

La sortida anterior mostra uns pocs ítems. Heu de tenir en compte que
aquesta comanda podria tardar bastant de temps ja que examina tots els
fitxers de cada paquet.

Se genera una sortida solament quan hi ha un problema.

Cada un dels caràcters desplegats mostra el resultat d’una comparació
d’atributs d’un fitxer amb cada valor dels atributs de la base de dades.
Un “.” punt significa que va passar la prova, mentre que un “?” únic
indica que la prova no s’ha pogut realitzar (per exemple per falta de
permisos). D’altra manera, els caràcters següents indiquen varis tipus
de fallades:

-  S: diferències amb la mida dels fitxers

-  M: els permisos del fitxer o tipus difereixen

-  5: el checksum MD5 difereix

-  D: discrepancia entre els números major/menor

-  L: discrepancia de ruta d’enllaç simbòlic

-  U: l’usuari propietari difereix

-  G: el grup propietari difereix

-  T: el temps de modificació difereix

-  P: les capacitats difereixen

Teniu en compte que moltes d’aquestes proves de verificació no indiquen
necessàriament un problema. Per exemple, molts fitxers de configuració
es modifiquen.

Alguns exemples:

-  Tot OK: no hi ha sortida.

   ::

        $ rpm -V bash

-  La sortida indica que la mida de fitxer, checksum i temps de
   modificació han canviat.

   ::

        $ rpm -V talk
        S.5....T in.ntalkd.8

-  La sortida indica que falta un fitxer:

   ::

        $ rpm -V talk
        missing /usr/bin/talk

Instal·lació de paquets
-----------------------

Tan simple com:

::

    $ sudo rpm -ivh foo-1.0.0-1.noarch.rpm

On **-i** és per instal·lar, **-v** per tenir una sortida detallada i
**-h** imprimeix marques de control per mostrar el progrés de la tasca.

RPM fa:

#. Verifica les dependències: és necessari degut a que alguns paquets no
   funcionaran correctament a no ser que un o més paquets estiguin
   instal·lats també.

#. Realitza comprovacions de conflictes: inclou intents d’instal·lar un
   paquet ja instal·lat o instal·lar una versió antiga sobre una més
   recent.

#. Executa ordres requerits abans de la instal·lació: El desenvolupador
   que construeix un paquet pot especificar certes tasques que s’han de
   dur a terme abans o després de la instal·lació. Lidia
   intel·ligentment amb els arxius de configuració:

#. Quan s’instal·la un arxiu de configuració, si l’arxiu existeix i ha
   estat modificat des que es va instal·lar el paquet, RPM guarda la
   versió antiga amb el sufix .rpmsave. Això permet integrar els canvis
   realitzats anteriorment en el fitxer de configuració antic en la
   versió nova de l’arxiu. Aquesta característica depèn que els paquets
   RPM hagin estat creats correctament.

#. Desempaqueta els arxius des dels paquets i els instal amb els
   atributs adequats: A més d’instal·lar arxius en el lloc correcte, RPM
   també configura atributs com ara permisos, propietaris i hora de
   modificació.

#. Executa ordres necessaris després de la instal·lació: Realitza
   qualsevol tasca de configuració o inicialització necessària posterior
   a la instal·lació.

#. Actualitza la base de dades RPM: Cada vegada que RPM instal un paquet
   s’actualitza informació a la base de dades. RPM fa servir aquesta
   informació quan verifica conflictes.

Desinstal·lació de paquets
--------------------------

L’opció **-e** instrueix a rpm que es desinstal·li un paquet. Normalment
**rpm -e** desplega un error quan falla, com quan un paquet que s’està
instentant desinstal·lar no està realment instal·lat o es requerit per
altres paquets del sistema. Una desinstal·lació exitosa no produeix cap
sortida.

::

   $ sudo rpm -e system-config-lvm
   package system-config-lvm is not installed

Error de dependències:

::

   $ sudo rpm -e xz
   error: Failed dependencies:
           xz is needed by (installed) dracut-033-161.el7.x86_64
           xz is needed by (installed) sos-3.0-23.el7.noarch
           xz is needed by (installed) libvirt-daemon-driver-qemu-1.1.1-29.el7_0.1.x86_64
           xz is needed by (installed) rpm-build-4.11.1-16.el7.x86_64
           /usr/bin/xz is needed by (installed) kmod-14-9.el7.x86_64
    

L’opció **–test** junt amb **-e** determina si la desinstal·lació tendrà
èxit o fallarà sense fer-la realment. No produeix cap sortida si serà
exitosa. Per a més informació emprar **-vv**.

Teniu en compte que l’argument que acompanya la comanda de la
desinstal·lació és el nom del paquet, no el nom del fitxer **rpm**.

**Nota important** i òbvia: no desinstal·leu el paquet **rpm** en sí. La
única forma de resoldre aquest problema és reinstal·lar el sistema
operatiu o arrancar el sistema amb un entorn de rescat.

Actualització de paquets
------------------------

Una actualització reemplaça el paquet original:

::

    $ sudo rpm -Uvh bash-4.2.45-5.el7_0.4.x86_64.rpm

És possible aprovisionar una llista de paquets.

En realitzar l’actualització, el paquet instal·lat amb anterioritat es
desinstal·la després de que la versió nova s’hagi instal·lat. La única
excepció és el fitxer de configuració de la instal·lació original, que
es manté amb l’extensió **.rpmsave**.

-  L’opció **-U** instal·la el paquet maldament l’original no estigui
   instal·lat.

-  L’opció **-i** no està pensada per actualitzar. Si s’intenta
   instal·lar un paquet RPM sobre un més antic, fallarà.

-  Diferents versions del mateix paquet poden estar instal·lades si cada
   versió de paquet no conté els mateixos fitxers. Per exemple, els
   paquets de kernel i les biblioteques d’altres arquitectures són
   generalment els únics que poden estar instal·lats vàries vegades.

-  Si desitja reemplaçar la versió actual d’un paquet per una anterior
   (*downgrade*) pot fer-se amb **rpm -U** s’ha d’afegir l’opció
   **–oldpackage** a la línia de comandes.

.. _actualització-de-paquets-1:

Actualització de paquets
------------------------

La comanda ``$ sudo rpm -Fvh *.rpm`` refrescarà tots els paquets del
directori actual

Funciona de la forma següent:

#. Si una versió anterior del paquet està instal·lada, aquesta serà
   actualitzada a la més nova del directori.

#. Si la versió és la mateixa, no es fa res.

#. Si no es troba cap versió instal·lada del paquet, no es fa la
   instal·lació.

Refrescar paquets és útil per instal·lar varis paquets alhora.

Actualització del kernel
------------------------

Quan s’instal·la un kernel nou en el sistema es requereix un reinici
perquè prengui efecte (una de les poques actualitzacions que ho
requereixen). No s’hauria de fer una actualització **(-U)** d’un nucli:
una actualització desinstal·laria el nucli que està actualment en
execució.

Això en si mateix no detendrà el sistema, però si després de reiniciar
el sistema hi ha algun problema, no hi haurà l’oportunitat de reiniciar
amb el kernel antic ja que va ser desinstal·lat del sistema. No obstant
això, si s’isntal·la amb **-i**, els dos kernels coexistiran i es podrà
triar si arrencar amb un o altre: és a dir, pot tornar a l’antic si ho
necessita.

Per instal·lar un kernel nou feu el següent:

::

   $ sudo rpm ivh kernel- {version}. {Arch} .rpm

Reemplaçau els noms correctes per a la versió i l’arquitectura.

En fer això, el fitxer de configuració de GRUB serà actualitzat
automàticament per incloure la versió nova. Aquesta serà l’opció
d’arrencada per defecte, llevat que reconfiguri el sistema perquè faci
una altra cosa.

Una vegada que la versió nova del kernel ha estat provada, es pot
desinstal·lar la versió antiga encara que no és necessari.

Ús de rpm2cpio
--------------

Per extreure fitxers d’un **rpm** sense instal·lar el paquet pot
emprar-se el programa **rpm2cpio**. Pot emprar-se per copiar fitxers
d’un rpm a un **cpio** com per extreure els fitxers si es desitja.

Crear un fitxer **cpio**:

::

    $ rpm2cpio foobar.rpm > foobar.cpio

Llistar fitxers en un RPM:

::

    $ rpm -qilp foobar.rpm

Per extreure al sistema:

::

   $ rpm2cpio bash-4.2.45-5.el7_0.4.x86_64.rpm | cpio -ivd bin/bash
   $ rpm2cpio foobar.rpm | cpio --extract --make-directories

EXERCICI PRÀCTIC - Preguntes
----------------------------

Quina és correcta?

#. yum és l’eina de baix nivell que interactua principalment amb paquets
   locals.

#. rpm és l’eina d’alt nivell que està al corrent de tots els paquets
   disponibles, incloent els que estan disponibles en servidors remots.

#. rpm -qa llista tots els paquets instal·lats en el sistema.

Quina comanda emprarem per conèixer la integritat de ``/bin/ls``? Aquest
binari està inclòs al paquet **coreutils**.

::

    rpm ____________________ coreutils

EXERCICI PRÀCTIC - Ús d’RPM
---------------------------

#. Trobau a quin paquet pertany l’arxiu /etc/logrotate.conf.

#. Llistau informació sobre el paquet, incloent els fitxers que conté

#. Verificau la instal·lació del paquet

#. Intentau desinstal·lar el paquet

EXERCICI PRÀCTIC - Reconstrucció de la BBDD RPM
-----------------------------------------------

Hi ha condicions sota les quals la base de dades RPM **/var/lib/rpm**
pot corrompre’s. En aquest exercici en construirem una nova i
verificarem la seva integritat.

#. Realitzau una còpia de seguretat de ``/var/lib/rpm``

#. Reconstruïu la base de dades

#. Comparau el contingut nou del directori amb la còpia de seguretat; no
   examineu el contingut dels fitxers ja que són dades binaries.
   Consultau el número de fitxers i els noms.

#. Obteniu una llista de tots els rpms del sistema. Comparau amb la de
   la còpia de seguretat. És interessant prendre una llista abans del
   procés de reconstrucció i comparar-la amb la llista de desrpés. Si la
   comanda funciona la base de dades hauria d’estar bé.

#. Compari de nou els continguts dels dos directoris. Tenen els mateixos
   fitxers?

Consultau https://rpm.org/user_doc/db_recovery.html per examinar amb més
detall els passos per verificar i/o recuperar la integritat de la base
de dades.

DPKG
====

El **Debian Package Manager (dpkg)** és usat per totes les distribucions
basades en Debian per controlar la instal·lació, verificació,
actualització i supressió de programari en sistemes Linux. El programa
de baix nivell **dpkg** pot realitzar totes aquestes operacions, ja
sigui en un sol paquet o en una llista d’ells. Les operacions que
podrien causar problemes (com eliminar un paquet del qual depèn un
altre, o instal·lar un paquet sense haver satisfet les dependències
primer) no es completen. De la mateixa manera que RPM, no està dissenyat
per obtenir i instal·lar paquets directament en l’ús diari, però sí per
instal·lar-los i desinstal·lar-los localment.

Tal com rpm, el programa dpkg té una vista parcial de l’univers: només
sap què està instal·lat en el sistema i qualsevol cosa que es proveeix a
través de la línia d’ordres. Però no sap res dels altres paquets
disponibles, si estan en un altre directori en el sistema o a Internet.
Com a tal, fracassarà també si una dependència no es compleix, o si algú
tracta de desinstal·lar un paquet que altres que estan instal·lats
necessiten.

.. _nomenclatura-dels-paquets-1:

Nomenclatura dels paquets
-------------------------

Els noms segueixen la forma:

::

    <name>_<version>-<revision_number>_<architecture>.deb

Exemple en Debian:

::

    logrotate_3.8.7-1_amd64.deb

Exemple en Ubuntu (insereix el nom de la distribució al paquet):

::

   logrotate_3.8.7-1ubuntu1_amd64.deb

Paquets de codi font
--------------------

El sistema d’empaquetament de codi font consisteix e ntres fitxers:

#. **Un fitxer upstream .tar.gz.** Conté les fonts originals així com
   les distribueix el mantenidor del paquet.

#. **Un fitxer de descripció .dsc.** Conté el nom del paquet i altres
   metadades com l’arquitectura i dependències.

#. **Un segon fitxer tar** que conté patches de la font d’upstream i
   fitxers addicionals. Acaben en .debian.tar.gz o .diff.gz.

Per exemple, per obtenir una font d’Ubuntu farem:

::

   $ apt-get source logrotate

Consultes DPKG
--------------

-  Llistar paquets instal·lats: ``dpkg -l``

-  Llista fitxers instal·lats amb un paquet determinat (**wget** per
   exemple): ``dpkg -L wget``

-  Mostra informació d’un paquet instal·lat: ``dpkg -p wget``

-  Mostra informació d’un fitxer de paquet:
   ``dpkg -I webfs_1.21+ds1-8_amd64.deb``

-  Llista fitxers de dins d’un fitxer de paquet:
   ``dpkg -c webfs_1.21+ds1-8_amd64.deb``

-  Mostra a quin paquet pertany un fitxer:
   ``dpkg -S /etc/init/networking.conf``:

-  Mostra l’estat d’un paquet: ``dpkg -s wget``

-  Verifica integritat d’un paquet instal·lat: ``dpkg -V package``.
   Sense arguments verificarà tots els paquets del sistema.

Instal·lació, actualització i desinstal·lació
---------------------------------------------

Instal·lació i actualització de paquets
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

    sudo dpkg -i foobar.deb

Serveix per instal·lar i actualitzar el paquet **foobar**. Si el paquet
no està instal·lat s’instal·larà. Si el paquet és més nou que
l’instal·lat, s’actualitzarà.

Desinstal·la
~~~~~~~~~~~~

::

    sudo dpkg -r package

Desinstal·la completament amb excepció dels fitxers de configuració. Si
afegim l’opció -P desinstal·la també els fitxers de configuració:

::

    sudo dpkg -P package

yum
===

El programa **yum** proveeix un nivell d’intel·ligència major per
utilitzar el programa **rpm**. Pot resoldre dependències automàticament
en instal lar, actualitzar i desinstal·lar paquets. Accedeix repositoris
de programari externs i se sincronitza amb ells, obtenint i instal·lant
programari a mesura que es necessita.

Instal·ladors de paquets
------------------------

Les eines de baix nivell com rpm o dpkg lidien amb certs detalls
d’instal·lació de paquets. Els **sistemes de gestió de paquets** d’alt
nivell treballen amb bases de dades de software disponible i incorporen
eines per trobar, instal·lar, actualitzar i desinstal·lar software de
forma intel·ligent. Les funcionalitats principals són:

-  Poden fer servir repositoris tant locals com remots. També paquets de
   codi font per a instal·lar i actualitzar paquets binaris i de codi
   font.

-  Són usats per automatitzar la instal·lació, actualització i
   desinstal·lació de paquets de programari.

-  Resolen dependències automàticament.

-  Estalvien temps perquè no hi ha necessitat de descarregar els paquets
   manualment o de cercar informació sobre les dependències.

Els repositoris de programari són proporcionats per distribucions i
altres proveïdors independents. Els instal·ladors de paquets mantenen
bases de dades del programari disponible, provinents de catàlegs
emmagatzemats en els repositoris. A diferència de les eines de paquets
de baix nivell, tenen l’habilitat de trobar i instal·lar dependències
automàticament.

.. _yum-1:

yum
---

**yum** és una interfície sobre rpm. La seva tasca principal és obtenir
paquets des de varis repositoris remots i resoldre dependències. És
emprat per la majoria de distribucions que empren rpm (però no totes).
Exemple: RHEL, Centos, Oracle Linux, Fedora etc.

**yum** emmagatzema en caché informació i la base de dades per millorar
el rendiment. Per esborrar informació de la caché es pot emprar la
comanda:

::

    $ # yum clean [ packages | metadata | expire-cache | rpmdb | plugins | all ]
    $ # exemple
    $ yum clean all

Els directoris ``/usr/bin/yum i /usr/sbin/yum`` inclouen extensions com
plugins i programes complementaris.

Ubicació dels repositoris
-------------------------

Els fitxers de configuració dels repositoris als quals té accés el
sistema es troben al directori ``/etc/yum.repos.d`` i tenen una extensió
.repo. Per exemple, el sistema RHEL 6:

::

    
   $ ls -l /etc/yum.repos.d

   total 40
   -rw-r--r-- 1 root root 957 Nov 4 2012 epel.repo
   -rw-r--r-- 1 root root 1056 Nov 4 2012 epel-testing.repo
   -rw-r--r-- 1 root root 188 May 28 2013 google-chrome.repo
   -rw-r--r-- 1 root root 113 Dec 11 2011 google-earth.repo
   -rw-r--r-- 1 root root 128 Dec 23 2013 google-talkplugin.repo
   -rw-r--r-- 1 root root 477 Jan 29 2012 nux-dextop.repo
   -rw-r--r-- 1 root root 529 Oct 30 2013 rhel-source.repo
   -rw-r--r-- 1 root root 1113 Jan 4 2011 rpmforge.repo
   -rw-r--r-- 1 root root 256 May 22 07:00 virtualbox.repo

Un fitxer de repositori molt simple podria ser:

::

   [repo-name]
   name=Description of the repository
   baseurl=http://somesystem.com/path/to/repo
   enabled=1
   gpgcheck=1

Pegau un guait a la resta de repositoris per visualitzar exemples més
complexos.

Es pot **habilitar** o **deshabilitar** un repositori en particular
canviant el valor de enabled a 0 o 1, o emprant les opcions
``--disablerepo=<repositori>`` i ``--enablerepo=<repositori>`` en emprar
yum. També es pot deshabilitar la verificació d’integritat amb la
variable **gpgcheck**.

Exemple

::

    yum install nano --disablerepo=epel-debuginfo

.. _consultes-1:

Consultes
---------

Tal com rpm, yum pot emprar-se per realitzar consultes i cerques, tant
de repositoris locals com remots.

#. Cerca de paquets amb la paraula **keyword** al nom:

   ::

      $ sudo yum search keyword
      $ sudo yum list "*keyword*"

   La primera comanda proporciona informació sobre els paquets en sí,
   mentre que la segona s’enfoca en el que està instal·lat i disponible.

#. Desplega **informació** sobre un paquet

   ::

      $ sudo yum info package

   Inclou informació sobre la mida, versió, repositori, URL d’origen i
   una descripció llarga. Es poden posar comodins (``*``) per a la
   majoria de comandes yum. No és un requisit que estigui instal·lat el
   paquet a diferència de rpm -q.

#. Llista tots els paquets, o sols els instal·lats, disponibles o
   actualitzacions no instal·lades.

   ::

      $ sudo yum list [installed | updates | available]

#. Mostra informació sobre **grups** de paquets instal·lats, disponibles
   etc:

   ::

      $ sudo yum grouplist [group1] [group2]
      $ sudo yum groupinfo group1 [group2]

#. Mostra paquets que **contenen un cert nom de fitxer**:

   ::

      $ sudo yum provides
      $ sudo yum provides "/logrotate.conf"

   Teniu en compte que cal posar al menys una barra ``/`` al nom del
   fitxer.

Verificació de paquets
----------------------

Per verificar paquets cal instal·lar el paquet **yum-plugin-verify**.
Això es pot fer de la forma:

::

    $ sudo yum install yum-plugin-verify

Això és una extensió (plugin) de yum, no un executable. N’hi ha
d’altres.

-  Per **verificar** un paquet: ``sudo yum verify <package>``

-  Per imitar exactament ``rpm -V``: ``sudo yum verify-rpm <package>``

-  | Per llistar diferències, incloent fitxers de
   | configuració: ``$ sudo yum verify-all <package>``. Sense arguments
     es verifiquen tots.

Per defecte, les comandes de verificació ignoren els fitxers de
configuració.

.. _installació-actualització-i-desinstallació-1:

Instal·lació, actualització i desinstal·lació
---------------------------------------------

-  **Instal·lar un o més paquets des dels repositoris**, a més de
   resoldre i instal·lar dependències:

   ::

        $ sudo yum install package1 [package2] ... [packageN]

-  Instal·lar des d’un **rpm local**:

   ::

        $ sudo yum localinstall package-file

   A diferència d’executar això amb rpm, amb yum intentarà resoldre les
   dependències accedint als repositoris remots.

-  Instal·lar un grup de software específic des d’un repositori, a més
   de resoldre i instal·lar dependències:

   ::

        $ sudo yum groupinstall group-name
        o
        $ sudo yum install @group-name

-  Desinstal·lar paquets del sistema:

   ::

        $ sudo yum remove package1 [package2] ... [packageN]

   S’ha d’anar en compte en eliminar paquets perquè yum també eliminar
   el paquets que depenen d’ell. No executeu ``yum remove`` amb l’opció
   ``-y`` i s’evitarà eliminar els paquets sense demanar confirmació.

-  **Actualitzar un paquet** des d’un repositori:

   ::

        $ sudo yum update <package>

   Si no s’especifica cap paquets s’actualitzaran tots.

Durant la instal·lació (o actualització), si un paquet té un arxiu de
configuració que serà actualitzat, l’arxiu amb la configuració antiga
**serà rebatejat** amb una extensió .rpmsave. Si l’arxiu amb la
configuració antiga es manté, l’arxiu de configuració nou serà rebatejat
amb una extensió .rpmnew. Es poden buscar aquestes extensions de noms de
fitxer (la majoria es trobaran a l’arbre de subdirectoris ``/etc``) per
determinar si necessita portar a terme alguna reconciliació, fent:

::

     $ sudo find /etc -name "*.rpm*"

Altres comandes de yum
----------------------

Es pot tenir una gamma àmplia de funcionalitats addicionals amb yum.
Aquestes funcionalitats provenen dels plugins instal·lats. Es poden
llistar els plugins disponibles amb:

::

    $ sudo yum list "yum-plugin*"

En particular:

-  Mostrar una llista de repositoris habilitats:

   ::

        $ sudo yum repolist

-  Iniciar una shell interactiva per executar vàries comandes yum:

   ::

        $ sudo yum shell [text-file]

   Es possible passar-li un fitxer amb una llista de comandes.

-  Descarrega els paquets però no els instal·la. Els guarda a
   ``/var/cache/yum`` o a un altre directori:

   ::

        $ sudo yum install --downloadonly package

   Es requereix el plugin **yum-plugin-downloadonly**.

-  Per veure l’històric de comandes yum, podeu emprar:

   ::

        $ sudo yum history

EXERCICI PRÁCTIC - Yum
----------------------

Vegeu els documents LAB_30.1.pdf, LAB_30.2.pdf, LAB_30.3.pdf i
LAB_30.4.pdf

APT
===

Disponible per a sistemes basats en Debian, el conjunt de programes
**APT (Advanced Packaging Tool)** proveeix un nivell d’intel·ligència
major per utilitzar el programa dpkg, i juga el mateix paper que yum en
sistemes basats en Red Hat. Les eines principals són **apt-get** i
**apt-cache**. Pot resoldre dependències automàticament en instal·lar,
actualitzar i desinstal·lar paquets. Accedeix a repositoris externs de
programari, sincronitzant-se amb ells, obtenint i instal·lant programari
segons sigui necessari.

apt-get
-------

**apt-get** és l’eina principal d’APT per a gestionar paquets. Es fa
servir per instal·lar, gestionar i actualitzar paquets individuals o el
sistema complet. Fins i tot pot actualitzar la distribució a una versió
completament nova, la qual cosa pot ser una tasca difícil.

Fins i tot hi ha extensions que li permeten a apt-get treballar amb
arxius rpm.

Tal com yum , apt-get funciona amb múltiples repositoris remots.

Consultes amb apt-cache
-----------------------

Les consulten se fan amb l’eina **apt-cache** i **apt-file**:

-  Consulta per un paquet anomenat **apache2** en el repositori:

   ::

         $ apt-cache search apache2 

-  Desplega informació bàsica sobre el paquet **apache2**:

   ::

         $ apt-cache show apache2 

-  Desplega informació detallada sobre el paquet **apache2**:

   ::

         $ apt-cache showpkg apache2 

-  Llista paquets dels quals depèn **apache2**:

   ::

         $ apt-cache depends apache2 

-  Cerca un fitxer anomenat **apache2.conf**:

   ::

         $ apt-file search apache2.conf 

-  Llista tots els fitxers del paquet **apache2**:

   ::

         $ apt-file list apache2 

Instal·la, actualitza i desinstal·la
------------------------------------

-  **Sincronitza l’índex d’arxius** de paquet amb el repositori font.
   Els índexos dels paquets disponibles s’obtenen des de les ubicacions
   especificades en ``/etc/apt/sources.list``:

   ::

         $ sudo apt-get update 

-  **Instal·la un paquet** o **actualitza**\ ’n un ja instal·lat:

   ::

         $ sudo apt-get install <paquet> 

-  **Desinstal·la** un paquet sense eliminar fitxers de configuració:

   ::

         $ sudo apt-get remove <paquet> 

-  **Desinstal·la** un paquet eliminant fitxers de configuració:

   ::

         $ sudo apt-get --purge remove <paquet> 

-  **Aplicar totes les actualitzacions disponibles** a paquets ja
   instal·lats:

   ::

         $ sudo apt-get upgrade 

-  **Aplicar una actualització intel·ligent** fent una resolució de
   dependències més profunda, instal·larà noves dependències i eliminarà
   paquets obsolets:

   ::

         $ sudo apt-get dist-upgrade 

   . Això no implica actualitzar la distribució, com normalment se
   malinterpreta.

-  Generalment, abans de fer l’operació **upgrade** o **dist-upgrade**
   s’ha d’executar abans l’operació **update** . A diferència de yum,
   els repositoris no s’actualitzen ni refresquen automàticament

-  **Eliminar paquets no necessaris** com versions del kernel antigues:

   ::

         $ sudo apt-get autoremove 

   .

-  **Netejar caché** i fitxers de paquets:

   ::

         $ sudo apt-cache clean 

   .

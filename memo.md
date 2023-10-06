# Creation d'un serveur IPBX
Voici un memo pour la configuration d'un serveur IPBX sous asterisk et FreePBX
## 1. installation du serveur sur raspberry-pi
Pour commencer il faut installer notre serveur sur la raspberry.
### Materiel necessaire:
- une rapsberry-pi 
- une micro sd d'au moins 8go
- un adaptateur pour micro sd
- le logiciel raspberry imager.
### Installation du serveur:
1. Installer l'image de raspbx sur ce site: http://www.raspbx.org/downloads/
2. Mettre la la micro sd de votre raspberry-pi dans l'adaptateur de micro sd.
3. Demarrer le logiciel raspberry-pi imager et selectioner l'image que vous souhaiter installer ainsi que la micro sd.
4. une fois l'installation faite, inserer la micro sd dans la raspberry.
## 2. Premier démarage du serveur
*Avant de brancher votre raspberry-pi assurer vous de bien avoir un clavier de brancher et surtout un cable réseaux afin que cet dernier soit bien connecter au réseau.*
Pendant le démarage de la raspberry maintenez la touche **shift** enfoncé afin de pouvoir démarrer sur la micro sd où notre image est présente.
Une fois le démarage terminé vous pourrez apercevoir l'ip du serveur apparaitre parmis les lignes de votre écran.
Mais avant de s'interesser a cette adresse ip il faut d'abord faire quelques configuration par ligne de commande.
### Configuration par ligne de commande:
Avant de commencer la configuration un mot de passe et un nom d'utilisateur nous serons demandé.
- nom d'utilistateur: "root"
- mot de passe: "raspbx"
*note: il est possible de se connecter a distance au serveur en ssh en utilisant le même identifant et le même mot de passe. Mais il sera préférable de regenérer un nouvelle clés par la suite en utilisant cette commande: "regen-hostkeys"*
### Liste des commandes de configurations:
- afficher la configuration de la région: "locale" ou "locale -a"
- reconfigurer la region: "dpkg-reconfigure-locales" *notes: pour cette reconfiguration sois  prise en compte, il faudra redémarrer raspbx.*
- redémarrer raspbx: "shutdown -r now"
- changer la configuration du clavier: "dpkg-reconfigure keyboard-configuration" *note: cette commande necessite aussi un redémarage*
- changer le mots de passe: "passwd"
- mettre à jour raspbx: "raspbx-upgrade"
## 3. Configuration du serveur téléphonique
Pour configurer le serveur nous allons utiliser FreePBX inclus dans l'image que nous avons installer dans la raspberry-pi.
Pour cela nous allons nous connecter a l'interface en entrant l'ip afficher sur notre serveur cité précédament dans le navigateur.
### Première connection à l'interface:
Tout d'abord, l'interface demande de se créer un compte admin en créant un identifiant et un mots de passe et entrant une adresse email afin de pouvoir reçevoir des notifications en cas de problémes.
### Installations et mises à jour de modules FreePBX <a id="IMAJ"></a>
Certaine fonctionalité de FreePBX correspondent à des modules qui doivent être mis à jour ou installer.
Pour ce faire, allez dans l'onglet admin et cliquer sur module admin. Une fois cela fait, cliquer sur le bouton **check online** . Suite à cela, les modules non installer apparaitrons et les modules pouvant être mis à jour seront en sur-brillance. Pour installer ou mettre à jour un module veuillez suivre la procédure ci-dessous:
1. cliquer sur le module que vous voulez installer ou mettre à jour.
2. cliquer sur le bouton **download and install**
3. cliquer sur **process** en bas à droite de la page.
4. cliquer sur **confirm**
5. cliquer sur **apply config** en haut à droite de la page.
### Uploader un enregistrements personalisé:
Pour pouvoir mettre une enregistrement personalisée aller dans l'onglet admin et cliquer sur **system recording**. Cliquer ensuite sur le bouton **add recording**. Puis donner un nom à votre enregistrement. Ajouter ensuite une description, choisissez la langue de votre enregistrement, vous pouvez soit choisir un enregistrement prédefinie soit parcourir vos fichier pour mettre un enregistrement personalisé. *note: faite attention au format de votre audio car il peut ne pas être lisble pour le serveur. Aprés chaque configuration, il faudras souvent appuyer sur **submit** puis en hauts de la page **apply config**.*
### Créer une annonce personalisée:
*note: cela nécéssite d'avoir fait la procédure au dessus*
Rendez-vous dans l'onglet **Applications** puis cliquer sur **Announcements** cliquer ensuite sur le bouton **Add**. Veuillez ensuite remplir les champs ci-dessous:
1. Description: La description de l'annonce
2. Recording: l'enregistrement personalisée créer précédament
3. Repeat: le nombre de fois ou l'annonce ce répéte
4. Return to IVR: décider de si l'on peut retourner à l'IVR
5. Answer to channel: Décider de si on peut répondre à ce canal ou non
6. Destination: Destination aprés la lecture de l'enregistrement
### Création d'une file d'attente:
Dans l'onglet application, cliquer sur **queues** appuyez ensuite sur **Add Queues**. Comme vous pourrez le constatez, il y a plusieur onglets, commençons par le premier: **General Settings**: Cela vous demandera en suite de remplir les champs suivant:
#### Genéral setting:
1. Queue Number: numéro a faire pour pouvoir transferer l'appelant sur la file d'attente.
2. Queue Name: nom de la fille d'attente.
3. Queue no answer: Desactiver la possibilité de répondre a l'appel.
4. Call confirm: L'appelant sera forcé de confirmer son appel.
5. Call confirm Announce: Un enregistrement pour confirmer que l'appel a bien était pris en compte.
6. CID Name Prefix: Préfixe du nom de l'appelant par exmple "Attente: John Smith"
7. Wait Time Prefix: Le temps d'attente sera ajouté au préfixe du nom de l'appelant
8. Alert info: Une alerte informant qu'un nouvelle appelant est dans file d'attente
9. Ringer volume override: configurer l'amplification le volume de la sonnerie. *note: cela n'est possible qu'avec des téléphone Sangoma*
10. Ringer Volume Override Mode: activer l'amplification du volume de la sonnerie.
11. Restric Dynamic Agents: Les appelants ne pourront pas être en contact avec un agent non listé.
12. Agent Restrictions: Choisir agents vers les lequels les appelants ne seront pas rediriger.
13. Ring Strategy: choisir la stratégie de sonnerie (cet à dire décider de par exemple faire sonner sur tout les téléphones)
14. Autofill: transfert automatique de l'appelant vers un agents disponible.
15. Skip Busy Agents: Les agents occupés seront passés par les appelants.
16. Queue Weight: définir le niveaux de priorité de la fille d'attente.
17. Music on Hold Class: Configuration de la musique d'attente.
18. Join Announcement: Configuartion de l'anonce quand l'appelant rejoin la file d'attente.
19. Call Recording: Définir si l'appel est enregistré ou non.
20. Mark calls answered elsewhere: si un appelant annule son appel, l'appel sera quand même marqué comme répondu.
21. Fail Over destination: destination en cas d'echecs.
#### Queue Agents:
1. Static Agents: Ajouts des agents fixe concerné par la file d'attente.
2. Dynamic Agents: Ajous d'agents temporairement concerné par la file d'attente.
#### Timing & Agent Options
1. Max Wait Time: Temps d'attente maximal
2. Max Wait Time Mode: Si **Strict**: une fois le temps d'attente dépasse expulsion imédiate de la file d'attente.
Si **Loose**, expulsion de file d'attente si aucun agent disponible.
3. Agent Timeout: nombre de temps où le téléphone de l'agent peut sonner jusqu'à que le delai sois dépassé.
4. Agent Timeout Restart: Si le delai est dépassé, une autre tentative s'executera.
5. Retry: Temps avant qu'un autre tentative s'enclenche.
6. Wrap-up-Time: aprés un appel, delai avant d'envoyer l'agent vers un autre appel.
7. Member Delay: Delai avant que l'agent ne soit en relation avec l'appelant.
8. Agent Announcement: Annonce jouer à l'agent avant qu'il soit mis en relation avec l'appelant.
9. Report Hold Time: Reporter le temps passé par l'appelant dans file d'attente à l'agent.
10. Auto Pause: Mettre automatiquement en status pause un agent ne répondant pas à l'appel.
11. Auto Pause on Busy: Mettre automatiquement en status pause un agent occupé.
12. Auto Pause on Unavailable: Mettre automatiquement en status pause un agent indisponible.
13. Auto Pause Delay: Delai de la pause automatique d'un agent.  
#### Capacity Options:
1. Max Callers: nombre d'appelant maximal dans la file d'attente
2. Join Empty: Un appelant peut-il rejoindre la file d'attente si tous les agents sont en pause ?
3. Leave Empty: Un appelant doit-il quitter la file d'attente si tous les agents sont en pause ?
4. Penalty Members Limit: Limite de penalité de membre.
#### Caller Announcements:
1. Fréquency: Fréquence de l'annonce de la position de l'appelant dans la file d'attente
2. Minimum Announcement Interval: Interval minimum entre chaque annonce.
3. Announce Position: Annoncer la position du l'appelant dans file d'attente
4. Announce Hold Time: Annoncer le temps d'attente dans la file.
5. IVR Break Out Menu: Integration d'un ivr afin de pouvoir quitter la file d'attente.
6. Repeat Frequency: Fréquence de répétition de l'annonce de l'ivr.
#### Advanced Options:
1. Service Level: statistiques du temps de réponse dans la file d'attente
2. Agent Regex Filter: filtrer les agens non concerné par la file d'attente.
#### Reset Queue Stats: 
-Stats Reset: Reinitialiser les statisques de la file d'attente.
### Creation d'un IVR:
L'IVR est un serveur vocal interactif par exemple: il permet à l'appelant de choisir quel service d'une entreprise il veut contacter.
Dans l'onglet application, cliquer sur **IVR** puis **Add IVR**. Il faudra ensuite remplir les champs ci-dessous:
1. IVR name: Nom de l'IVR
2. IVR Description: Description de l'IVR
3. Announcement: annonce a diffusé dans l'IVR
4. Enable Direct Dial: Activer la ligne directe
5. Force Stric Dial Timeout: fonctionnement de l'ivr uniquement avec les premières touches ou avec toutes.
6. Timeout: Nombre de temps avant que le delai d'attente sois dépasser.
7. Alert Info: Alerte en cas d'erreur
8. Ringer volume override: configurer l'amplification le volume de la sonnerie. *note: cela n'est possible qu'avec des téléphone Sangoma*
9. Invalid Retries: nombre de tentives quand la réponse de l'appelant est invalide.
10. Invalid Retry Recording: Enregistrement diffusé quand la réponse de l'appelant est incorecte.
11. Append Announcement to Invalid: Rejouer l'enregistrement de l'IVR en cas de réponse invalide.
12. Return on Invalid: Possiblité de retourné à l'IVR parent.
13. Invalid Recording: Enregistrement diffusé pour aller dans une destination alternative.
14. Invalid Destination: Destination où l'appelant est envoyé quand la destination est invalide.
15. Timeout Retries: nombre d'essaie en cas de delai dépassé.
16. Timeout Retry Recording: Enregistrement diffusé une fois que les essai sont épuisé.
17. Append Announcement on Timeout: Rejouer l'annonce de l'IVR aprés une delai dépassé.
18. Return on Timeout: Retournez à l'IVR parents aprés un delai dépassé.
19. Timeout Recording: enregistrement diffusé aprés un delai dépassé.
20. Timeout Destination: Destination de l'appelant aprés un delai dépassé.
22. Return to IVR after VM: retourner à l'IVR aprés un message vocal laissé par l'appelant.
#### Entrée de L'IVR
Il s'agit des touches permettant de choisir sa destination par exemple: "appuyer sur la touche 1 pour allez dans file d'attente." *notes: si vous souhaitez que l'IVR redirige automatiquement l'appelant dans une destination, selectionez directement "time out destination" pour définir un destination*
### Creation d'un time conditions:
Le time condtions permet de rajouter un condition de temps par exemple si l'on veut que l'appelant tombe sur une messagerie vocale pendant les horraires de fermetures, on vas donc utiliser le time conditons. *notes: il est possible que vous ayez besoin d'installer le module "times conditions" (voir la procédure a suivre dans [Installations et mises à jour de modules FreePBX](#installations-et-mises-à-jour-de-modules-freepbx))*
Une fois le module installé si nécéssaire, rendez-vous dans applications et dans **time conditions**. Appuyez sur le bouton **Add Time-condtions**. Tout comme les modules précedents, remplir le champs suivant:
1. Time Condition name: nom du Time condition
2. Override Code Pin: Code pour changer l'état l'amplification du volume de la sonnerie.
3. Invert BLF Hint: Ne pas appliquer le time condition si activé.
4. Change Override: changer le status de l'override.
5. Time zone: définir la zone de temps du time condition.
6. Mode: Utiliser le Time Group ou utiliser le calendrier. *note: nous reviendrons a la fin de cet liste sur l'utilisation du Time Group et du Calendrier.*
7. Destination matches: Destination ou l'appelant est dirger
8. Destination non-matches: Destination ou l'appelant est dirgrer en cas de conditions invalides.
#### Utilisation du  Time groups:
Le time Groups permet de définir une ou plusieur periode dans la laquelle la condtions doit s'executer. Pour ce faire vous pouvez directement mettre **Time Group Mode**  dans le champs mode. Vous pourrez en suite dans le champs **Time Group** juste en dessous. définir les heures, les jours, les mois etc... .
#### Utilisation du Calendar:
Contrairement au Time groups, ce dernier nécéssite d'aller directements dans l'onglets applications, puis de cliquer sur **Calendar** cliqeur en suite sur **Add Calendar** puis sur **Add Local Calendar**. Nommer le calendrier puis mettez y une description et enfin, Selectionez votre Zone de Temps. Une fois cela fait cliquer sur **submit** puis **applyconfig**
Une fois cela fait, la liste de vos calendrier apparaitra, cliquez sur l'icone de crayon pour le modifier. Une fois cela fait votre calendrier vas aparaître, cliquer sur le bouton en à gauche pour rajoutez un evenement puis remplisser les champs demander comme le jour, l'heure, si ce la doit se répêter etc... . Une fois cela fait vous pourrez crée votre time conditions et ainsi selectioner dans le champ mode: **Calendar Mode** et selectioner votre calendrier dans le champs en-dessous. *tips: il est préférable de faire un time groups pour programmer des horraires habituelles et d'utiliser le calendrier pour programmer un evenement tel qu'un rdv*.


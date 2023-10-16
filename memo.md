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
## 3. Configuration du serveur téléphonique par interace
Pour configurer le serveur nous allons utiliser FreePBX inclus dans l'image que nous avons installer dans la raspberry-pi.
Pour cela nous allons nous connecter a l'interface en entrant l'ip afficher sur notre serveur cité précédament dans le navigateur.
### Première connection à l'interface:
Tout d'abord, l'interface demande de se créer un compte admin en créant un identifiant et un mots de passe et entrant une adresse email afin de pouvoir reçevoir des notifications en cas de problémes. 
(voir image annexe 0).
Une fois le compte admin créer il vous faudras vous connecter en tant qu'admin.
(voir images annexe 1 et 1-2).
Une fois connecter sur l'interface vous aterirer sur le Dashboard qui permet d'avoir toutes les informations nécéssaire. Par exemples les erreurs, la places disponible etc...
(voir image annexe 1-3)
### Installations et mises à jour de modules FreePBX <a id="IMAJ"></a>
Certaine fonctionalité de FreePBX correspondent à des modules qui doivent être mis à jour ou installer.
Pour ce faire, allez dans l'onglet admin et cliquer sur module admin. Une fois cela fait, cliquer sur le bouton **check online** . Suite à cela, les modules non installer apparaitrons et les modules pouvant être mis à jour seront en sur-brillance. Pour installer ou mettre à jour un module veuillez suivre la procédure ci-dessous:
1. cliquer sur le module que vous voulez installer ou mettre à jour.
2. cliquer sur le bouton **download and install**
3. cliquer sur **process** en bas à droite de la page.
4. cliquer sur **confirm**
5. cliquer sur **apply config** en haut à droite de la page.
(voir images annexe 2)
### Uploader un enregistrements personalisé:
Pour pouvoir mettre une enregistrement personalisée aller dans l'onglet admin et cliquer sur **system recording**. Cliquer ensuite sur le bouton **add recording**. Puis donner un nom à votre enregistrement. Ajouter ensuite une description, choisissez la langue de votre enregistrement, vous pouvez soit choisir un enregistrement prédefinie soit parcourir vos fichier pour mettre un enregistrement personalisé. *note: faite attention au format de votre audio car il peut ne pas être lisble pour le serveur. Aprés chaque configuration, il faudras souvent appuyer sur **submit** puis en hauts de la page **apply config**.*
(voir images annexe 3)
### Ajouter des musiques de fond:
Les musique de fond servent à faire patienter l'appelant dans une fille d'attente ou encore un IVR. Pour rajouter une musique de fond aller dans l'onglet Settings puis **Music on Hold**. Selection ensuite la catégorie default en cliquant sur le logo de crayon ou créer en une nouvelle en appuyant sur **Add Category**. Remplisser ensuite les champs suivant:
1. Type: Files: selectioner un ou plusieurs fichiers audio.
         Custom Application: selectioner un application diffusant de la musique.
2. Enable Random Play: Activer la lecture aleatoire des fichier audio.
3. Upload Recording: Si dans le champ **Type** des **Files**
4. Convert Upload/Files to: choisir le format qu'aura votre fichier audio.
5. File: liste des fichier audio présent *note: il y peut directement y a avoir des fichier audio préselectionner*
(voir images annexe 4)
### Créer une annonce personalisée:
*note: cela nécéssite d'avoir fait la procédure au dessus*
Rendez-vous dans l'onglet **Applications** puis cliquer sur **Announcements** cliquer ensuite sur le bouton **Add**. Veuillez ensuite remplir les champs ci-dessous:
1. Description: La description de l'annonce
2. Recording: l'enregistrement personalisée créer précédament
3. Repeat: le nombre de fois ou l'annonce ce répéte
4. Return to IVR: décider de si l'on peut retourner à l'IVR
5. Answer to channel: Décider de si on peut répondre à ce canal ou non
6. Destination: Destination aprés la lecture de l'enregistrement
(voir images annexe 5)
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
(voir images annexe 6)
#### Queue Agents:
1. Static Agents: Ajouts des agents fixe concerné par la file d'attente.
2. Dynamic Agents: Ajous d'agents temporairement concerné par la file d'attente.
(voir image annexe 6-2)
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
(voir images annexe 6-3)
#### Capacity Options:
1. Max Callers: nombre d'appelant maximal dans la file d'attente
2. Join Empty: Un appelant peut-il rejoindre la file d'attente si tous les agents sont en pause ?
3. Leave Empty: Un appelant doit-il quitter la file d'attente si tous les agents sont en pause ?
4. Penalty Members Limit: Limite de penalité de membre.
(voir images annexe 6-4)
#### Caller Announcements:
1. Fréquency: Fréquence de l'annonce de la position de l'appelant dans la file d'attente
2. Minimum Announcement Interval: Interval minimum entre chaque annonce.
3. Announce Position: Annoncer la position du l'appelant dans file d'attente
4. Announce Hold Time: Annoncer le temps d'attente dans la file.
5. IVR Break Out Menu: Integration d'un ivr afin de pouvoir quitter la file d'attente.
6. Repeat Frequency: Fréquence de répétition de l'annonce de l'ivr.
(voir images annexe 6-5)
#### Advanced Options:
1. Service Level: statistiques du temps de réponse dans la file d'attente
2. Agent Regex Filter: filtrer les agents non concerné par la file d'attente.
(voir images annexe 6-6)
#### Reset Queue Stats: 
-Stats Reset: Reinitialiser les statisques de la file d'attente.
(voir images annexe 6-7 )
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
(voir images annexe 7 et 7-1)
#### Entrée de L'IVR
Il s'agit des touches permettant de choisir sa destination par exemple: "appuyer sur la touche 1 pour allez dans file d'attente." *notes: si vous souhaitez que l'IVR redirige automatiquement l'appelant dans une destination, selectionez directement "time out destination" pour définir un destination*
(voir images annexe 7-2)
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
(voir images annexe 8 et 8-1)
#### Utilisation du  Time groups:
Le time Groups permet de définir une ou plusieur periode dans la laquelle la condtions doit s'executer. Pour ce faire vous pouvez directement mettre **Time Group Mode**  dans le champs mode. Vous pourrez en suite dans le champs **Time Group** juste en dessous. définir les heures, les jours, les mois etc... .
(voir images annexe 8-2)
#### Utilisation du Calendar:
Contrairement au Time groups, ce dernier nécéssite d'aller directements dans l'onglets applications, puis de cliquer sur **Calendar** cliqeur en suite sur **Add Calendar** puis sur **Add Local Calendar**. Nommer le calendrier puis mettez y une description et enfin, Selectionez votre Zone de Temps. Une fois cela fait cliquer sur **submit** puis **applyconfig**
Une fois cela fait, la liste de vos calendrier apparaitra, cliquez sur l'icone de crayon pour le modifier. Une fois cela fait votre calendrier vas aparaître, cliquer sur le bouton en à gauche pour rajoutez un evenement puis remplisser les champs demander comme le jour, l'heure, si ce la doit se répêter etc... . Une fois cela fait vous pourrez crée votre time conditions et ainsi selectioner dans le champ mode: **Calendar Mode** et selectioner votre calendrier dans le champs en-dessous. *tips: il est préférable de faire un time groups pour programmer des horraires habituelles et d'utiliser le calendrier pour programmer un evenement tel qu'un rdv*.
(voir images annexe 8-3)
### Créations d'extensions:
Les extensions permettent de rajouter des téléphones pour les agents (Les téléphones des employées afin de pouvoir répondre et passer des appels ). Dans notre cas nous allons utiliser l'application mobile **Zoiper**. Cette application permet d'avoir un téléphone virtuel sur un téléphone portable ou sur pc afin de ne pas avoir a acheter de téléphone fixe.
#### Ajouts d'extensions: 
Pour se faire, allez dans l'onglet applications puis cliquez sur **Extension** cliquez ensuite sur **Add Extensions** puis
**Add New SIP \[chan_pjsip] Extension**. Puis remplisser les différents champs dans les différent onglets suivants:
#### General:
1. User Extension: Extension de l'utilisateur par exmple "100" *notes: Cette extensions corespondra au numéro à taper afin de pouvoir contacter l'utilisateur en interne.*
2. Display Name: Nom de l'utilisateur par exmple: "John Smith"
3. Outbond CID: ID de l'appelant pour sortir du réseaux de l'entreprise.
4. Emergency CID: ID d'un numéro d'urgence.
5. Secret: Mots de passe utiliser pour se connecter a l'extensions. *note: Absolument changer le mots de passe*
7. Select User Directory: Selectioner le chemin utilisateur
8. Link to a Default User: Créer un nouvel utilisateur ou utiliser un compte existant
9. Username: Nom d'utilisateur
10. Password for new user: Mots de passe en cas de création de compte.
11. Groups: Définir un groupes d'utilisateur.
(voir images annexe 9)
#### Voicemail: 
1. Enable: activer les messages vocaux
2. Voicemail Password: Mots de passe pour accèder au système de messages vocaux.
3. Require From Same Extension: Les autres extensions peuvent lire les messages vocaux.
4. Disable (/*) in Voicemail Menu: Desactiver la touche (*) dans les messages vocaux.
5. Email Address: Adresse email ou les message vocaux sont envoyer.
6. Pager Email Address: adresse email du pager pour reçevoir les notifications.
7. Email Attachment: joindre les message vocaux au emails.
8. Play CID: citer le numéro de l'appelant dans les message vocaux.
9. Play Envelope: citer la date et l'heure de l'enregistrement du message.
10. Delete Voicemail: Supprimer les messages vocaux aprés avoir était envoyé par email.
11. VM Options: Options a rajouter 
12. VM Context: Contexte des messages vocaux *notes: Attention modifier ce paramètre peut être risquer* 
(voir images annexe 9-1)
#### Utilisation de zoiper
Pour commencer installer Zoiper sur votre Pc en suivant ce lien: https://www.zoiper.com/en/voip-softphone/download/current ou sur votre téléphone en passant par le appstore ou le play store en passant celon votre OS.
Une fois Zoiper installé, une connexion vous sera demander. *Attention: ne pas créer de compte car cela nécistera d'acheter un fournisseur.* Choisissez donc l'option se connecter, mettez dans nom d'utilisateur votre extension créer sur l'interface suivi d'un @ et de votre serveur sip (mettez l'ip qui correspond a votre serveur). Voici un exemple: extensions@192.168.0.10 . Pour le mots de passe, mettez celui que vous avez mis dans le champs **secret** de votre extension. Une fois cela fait, l'application vas rechercher votre serveur afin de s'y connecter. *note: il se peut que votre application ne se connecte pas au serveur car vous n'avez pas changer le mots de passe de votre extensions*
(voir images annexe 9-2)
### Créer un trunks: 
Les trunks Permettent d'acheminer les appels entrants et sortants. Il existent plusieurs protocoles de trunks tous commes les extensions. Dans notre cas, nous allons choisir le protocoles **SIP** tout comme nos extensions. Pour ce faire rendez-vous dans connectivity, choisissez ensuite **trunks** puis cliquez sur **Add Trunk** puis **Add SIP (chang_pjsip) Trunk**. Voici ensuite les champs suivants à remplir selon ce la ligne que vous à fourni votre fournisseur:
#### General: 
1. Trunk Name: Nom du Trunk.
2. Hide CallerID: Cacher le numéro de l'appelant 
3. Outbond CallerID: Numéro que le destinataire vera l'hors d'un appel sortant.
4. CID Options: Authorisation des numéro hors du trunk.
5. Maximum Channels: Nombre d'appel simultané sortant .
6. Asterisk Trunk Dial Options: Commande Asterisk à utiliser lors des appels hors du trunk pour remplacer les paramétre par défauts.
7. Continue if Busy: En cas d'echec continuer vers le trunk suivant.
8. Disable Trunk: Desactiver le trunk
9. Monitor Trunk Failures: Script pour envoyer des logs par email en cas de problémes.
(voir images annexe 10)
#### Dialed Number Manipulation Rules: 
Cet onglet s'apparante au régles de composition des numéro ici nous allons entrer les numéro d'urgence a mettre afin que l'entreprise puis appeler ces derniers en cas d'urgences. Une fois dans cet Onglets, cliquer sur le bouton **Dial patterns wizards** une fois cela fait une page s'ouvrira. Selectionnez uniquement **10 Digit Patterns** puis cliquer sur Generate Routes en bas à droite. Une fois cela fait vous verrez que des routes d'urgences ce sont genérer avec des numéros d'urgence provenat des états unis remplacez-les par les numéro d'urgence français puis dans la dernier case remplisser la case préfixe à gauche par un nombre au hazard puis mettez 10 X correspondant au nombre de chiffre dans un numéros Français.
(voir images annexe 10-1)
#### pjsip Settings: 
1. Username: Nom d'utilisateur *note: cela doit corespondre à un série de chiffre comme ceci: 0033XXXXXXXXXX
2. Auth username: nom d'autentification *notes: à ne remplir uniquement si cela diffère du nom d'utilisateur*
3. Secret: Mots de passe corespondant à a votre ligne
4. Authentication: Autorisation des autentification d'autres serveurs.
5. Registration: Envoyer ou reçevoir le registre
6. Language Code: Language utiliser par les enregistrement de voix.
7. SIP Server: Adresse de votre serveur SIP.
8. SIP Server Port: Port d'ecoute de votre serveur SIP.
9. Context: Context à envoyer à l'appel entrant. *note: pour une bon fonction: mettre "from-pstn-toheader"
10. Transport: Le transport utiliser à la connection.
(voir images annexe 10-2)
### Créer une inbound route: 
Une inbound routes est une routes entrante ou arriveront les appel qui viennent de l'exterieur. Exemple d'utilisation: l'appelant arrive sur un ivr quand il l'appele le numéro associé. Rendez vous dabord dans connectivity puis ensuite **Inbounds Routes** Puis **Add Inbounds Routes**, remplisser les champs suivants:
1. Description: description de la route.
2. DID Number: numéro à composer pour pouvoir appeler (mettre le numéro de la ligne fournis par votre fournisseur)
3. CallerID Number: Définir le numéro de l'appelant corespondant, laisser se champs vide pour que n'importe quelle numéros correspondent.
4. CID Priority Route: si oui: Priorité au numéro correspondant a cette route.
5. Alert info: Alerte pouvant être utiliser pour des appareil SIP distinct.
6. Ringer Volume Override: Remplacement du volume de la sonnerie.
7. CID name prefix: Définir un préfix au numéro des appelants par exemple: Boutique: John.
8. Music on Hold: definir la musique en fond.
10. Set destination: destination de la route entrante. *note: si vous voulez que la destination dépande des horraire mettez votre time condition qui redirigera l'appelant en fonction de l'heure.*
(voir images annexe 11)
### Créer une outbound route: 
Une outbound route est une une route de sortie permettant au agents (employés) d'appeler des personne exterieure. Par exemple permettre à un employé d'appeler un clients. Pour cela dans l'onglet Connectivity cliquer sur **Outbounds Routes** puis cliquer sur **Add Outbonds Routes**. Remplisser ensuite les champs suivants:
1. Route Name: Sortie
2. Route CID: Numéro que le client verra lors d'un appel entrant de la part d'un agent.
3. Override Extensions: Remplacer le numéro de l'extension de l'agent affiché lors d'un appel par le numéro de la route sortante.
4. Route Password: mots de passe à définir pour faire un appel sortant (non obligatoire)
5. Route Type: Type de la route: route d'urgence ou route intra-Companie
6. Music on hold?: Musique en fond à définir.
7. Time match Zone: Zone de temps correspondante.
8. Time match time group: Groupe de temps correspondant. (peut être laisser vide)
9. Trunk Sequence for Matched Routes: Trunk correspondant aux routes.
10. Optional destination on congestion: Destinations optionelle en cas de congestion.
Il vous faudras ensuite d'aller dans le sous-onglet **Dial Patterns** puis configurer comme dans le trunks (voir les explications sur le trunks).
(voir images annexe 12)
### Parametrer le serveur SMTP:
Afin de pouvoir reçevoir des notifications par email en cas de probléme ou même reçevoir les message vocaux par mail nous allons devoir configurer les parametres SMTP.
#### Se connecter en SSH:
 Malheuresement ce paramétre ne ce fais pas par l'interface mais par terminal de commande nous allons donc devoir se connecter en ssh. Pour ce faire, dans votre terminal, taper la commande suivante:
`ssh nomdutilisateur@adresseduserveur` une fois cela fait, il vous faudra entrer un mot de passe. *note: il s'agit du mots de passe que vous avez entrer sur le terminal lors du premier démarage du serveur. (voir [2. Premier démarage du serveur](#2-premier-démarage-du-serveur)). Une fois connecter en SSH vous aurez droit à affichage (voir image annexe SSH)
#### Parametrage du SMTP:
Une fois connecter en SSH, tapez la commande suivante `nano /etc/postfix/main.cf`, une fois cela fait vous devriez atterir sur une page ressemblant à cela (voir image annexe smtp 1). Defilez ensuite tout en bas de la page, puis entrez les lignes suivante selon le serveur SMTP auquel l'email que vous souhaitez utiliser est racrocher. 

relayhost = [adresse de votre fournisseur]: 587 ou 465 (selon votre fournisseur)  
smtp_sasl_auth_enable = yes (a renter quelque sois le fournisseur)  
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd (a renter quelque sois le fournisseur)  
smtp_sasl_security_options = noanonymous (a renter quelque sois le fournisseur)  
smtp_use_tls = yes (a renter quelque sois le fournisseur)  
  
smtp_generic_maps = hash:/etc/postfix/generic

Une fois cela rentrer, faite la combinaison de touches suivante : ctrl+o. Cela aura pour effet d'écrire le fichier. (enregistrer le fichier). Maintenant que cela est fait, renter la commande suivante: `postfix reload` ce qui aura pour effet de recharger le postfix.  
  
 entrer en suite la commande suivante `nano /etc/postfix/sasl_passwd`, allez ensuite à la derniere ligne pour rajouter la ligne suivante:  
  "[adresse de votre fournisseur] username:password" *note: le username correspond à l'adresse mail que vous souhaitez utiliser et le mots de passe correspond à mots de passe de l'adresse mail.  

  Entrer ensuite la commande suivante: `chmod 400 /etc/postfix/sasl_passwd` cela permettra d'autoriser l'excution du fichier.  
  Entrer ensuite la commande suivante: `postmap /etc/postfix/sasl_passwd` puis la même commande mais en rajoutant `chown` devant.  
  Entrer ensuite la commande suivante: `systemctl reload postfix`  
  Entrer ensuite la commande suivante: `nano /etc/postfix/generic` descender en suite tout en bas de la page.  
    
Puis rajouter ensuite les champs suivant:  
  
root emailfromaddress@real-domain.com 
root@localhost emailfromaddress@real-domain.com 
root@localhost.localdomain emailfromaddress@real-domain.com 
root@freepbx emailfromaddress@real-domain.com 
root@freepbx.localdomain emailfromaddress@real-domain.com 
asterisk emailfromaddress@real-domain.com 
asterisk@localhost emailfromaddress@real-domain.com 
asterisk@localhost.localdomain emailfromaddress@real-domain.com 
asterisk@freepbx emailfromaddress@real-domain.com 
asterisk@freepbx.localdomain emailfromaddress@real-domain.com
vm@asterisk emailfromaddress@real-domain.com
  
*notes: remplaçer emailfromaddress@real-domain.com par l'adresse mail que vous souhaitez utiliser.*  
  
Appuyez ensuite sur ctrl+o pour enregister. Puis faites la commande suivante: `postmap /etc/postfix/generic`.  
Faites ensuite la commande suivante: `service postfix restart`  
Puis celle-ci: `systemctl restart postfix`  

Voilà le serveur SMTP est maintenant configurer. 

### Créer une Boite Vocale:
La boite vocale vas permettre aux appelants de pouvoir laisser un message vocal en cas de fermeture ou d'indisponibilté de l'entreprise. Pour ce faire, Créer une extensions appelé boite vocale. (voir [Créations d'extensions](#créations-dextensions))
*note: il faudras activer et configurer les messages vocaux dans le deuxieme onglet **voicemail***. Une fois tous cela fait, rendez-vous dans l'onglet Settings puis cliquer sur **Voicemail Admin**.  
  
Choisissez ensuite votre extension à droite de votre écran puis si cela n'a pas était fait dans l'extension, remplisser les champs suivants:
1. Voicemail Password: Mots de passe de la boite vocale.
2. Email Address: Adresse email où les message vocaux seront envoyés *notes: le serveur SMTP doit être parametré pour que cet fonctionalité sois disponible*
3. Pager Email Address: Emails où seront envoyer les notification comme quoi un message vocal à était laisser dans la boite. *notes: ce champ peut être laisser vide afin de juste reçevoir les messages vocaux sans notifications suplementaires.*
4. Email Attachement: Attacher les messages vocaux au emails.
5. Play CID: citer le numéro de téléphone de l'appelant dans le message vocal.
6. Play Envelope: Citer la date et l'heure dans le message vocal.
7. Delete Voicemail: Supprimer les message vocaux dans le serveur une fois la notification reçu.
9. Call-Me Number: Numéro pour contacter l'extensions.
  
#### Configurer la forme des mails:
1. Email Subject: sujet du mail
2. Email Body: Corp de l'email
3. Email From String: De qui proviens l'email
4. Email Date Format: Format de la date
5. Pager Subject: Sujet du mail de notification
6. Pager Body: Corps du mail de notification
7. Pager From String: De qui provient l'email de notification
8. Pager Date Format: Format de la date de l'email de notification
9. Server Email: Serveur de mail
10. Skip PBX String: Ne pas afficher la provenance du mail
11. Attach Voicemail: Attacher le message vocal au mail
12. Mail Command: Spécifier la commande d'envoie d'email
      

 

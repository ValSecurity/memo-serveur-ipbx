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
### Installations et mises à jour de modules FreePBX:
Certaine fonctionalité de FreePBX correspondent à des modules qui doivent être mis à jour ou installer.
Pour ce faire, allez dans l'onglet admin et cliquer sur module admin. Une fois cela fait, cliquer sur le bouton **check online** . Suite à cela, les modules non installer apparaitrons et les modules pouvant être mis à jour seront en sur-brillance. Pour installer ou mettre à jour un module veuillez suivre la procédure ci-dessous:
1. cliquer sur le module que vous voulez installer ou mettre à jour.
2. cliquer sur le bouton **download and install**
3. cliquer sur **process** en bas à droite de la page.
4. cliquer sur **confirm**
5. cliquer sur **apply config** en haut à droite de la page.
### Uploader un enregistrements personalisé:
Pour pouvoir mettre une enregistrement personalisée aller dans l'onglet admin et cliquer sur **system recording**. Cliquer ensuite sur le bouton **add recording**. Puis donner un nom à votre enregistrement. Ajouter ensuite une description, choisissez la langue de votre enregistrement, vous pouvez soit choisir un enregistrement prédefinie soit parcourir vos fichier pour mettre un enregistrement personalisé. *note: faite attention au format de votre audio car il peut ne pas être lisble pour le serveur*
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
Dans l'onglet application, cliquer sur **queues** appuyez ensuite sur **Add Queues**. cela vous demandera en suite de remplir les champs suivant:
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
### Creation d'un IVR:
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


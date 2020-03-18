### GAZERIAN Nicolas, VERRIERE Nicolas - 4ETI

# **Exercice 1 : Gestion des utilisateurs et des groupes**

### *Question 1) Commencez par créer deux groupes groupe1 et groupe2*

``` bash
sudo addgroup groupe1
sudo addgroup groupe2
```
> Pour effectuer ces commandes, il faut aussi entrer le mot de passe de l'admin.

### *Question 2) Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell*

``` bash
sudo useradd u1
sudo useradd u2
sudo useradd u3
sudo useradd u4
```
> On doit maintenant créer les dossiers associés aux utilisateurs dans home. On va dans le dossier home pour créer les dossiers et les lier aux utilisateurs.
> Pour cela, on rentre :

```bash 
cd ~
mkdir u1 u2 u3 u4

chown u1: u1
chown u2: u2
chown u3: u3
chown u4: u4
```


### *Question 3) Ajoutez les utilisateurs dans les groupes créés :*

*- u1, u2, u4 dans groupe1*

*- u2, u3, u4 dans groupe2*

```bash
usermod -a -G groupe1 u1
usermod -a -G groupe1 u2
usermod -a -G groupe1 u4

usermod -a -G groupe2 u2
usermod -a -G groupe2 u3
usermod -a -G groupe2 u4
```

### *Question 4) Donnez deux moyens d’afficher les membres de groupe2*

```bash
cat /etc/group
```
> Cette commande permet d'afficher tous les groupes du dossier etc/group avec quelques paramètres, dont les utilisateurs en paramètre 4.

```bash 
grep ^groupe2: /etc/group | cut -d : -f4
```
> Cette commande permet d'afficher le 4è paramètre de la ligne de etc/group commençant par groupe2, donc seulement les utilisateurs du groupe 2.


### *Question 5) Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4*

> Pour cela, nous faisons les commandes suivantes : 
```bash
sudo chgrp groupe1 /home/u1
sudo chgrp groupe1 /home/u2
sudo chgrp groupe2 /home/u3
sudo chgrp groupe2 /home/u4
```
> Pour vérifier, on utilise la commande *ls -l /home* et on voit que les correspondances ont bie nété faites.

### *Question 6) Remplacez le groupe primaire des utilisateurs :*

```bash
sudo usermod u1 -g groupe1
sudo usermod u2 -g groupe1
sudo usermod u3 -g groupe2
sudo usermod u4 -g groupe2
```
### *Question 7) Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé.*

> Pour cela, il faut tout d'abord créer ces deux répertoires :
```bash
mkdir /home/groupe1
mkdir /home/groupe2
```
> Ensuite on lie ces dossiers aux groupes correspondants :

```bash
sudo chgrp groupe2 /home/groupe2
sudo chgrp groupe1 /home/groupe1
```
> Enfin on donne la permission aux membres de chaque groupe d’écrire dans le dossier associé : 

```bash
sudo chmod g+w /home/groupe1
sudo chmod g+w /home/groupe2
```
### *Question 8) Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?*

> Laisser les droits de modification à seulement l'utilisateur revient donc à le retirer à tous les autres utilisateurs (membres du groupe ou non).
Pour cela on utiliserait les commandes **sudo chmod g-w** et **sudo chmod o-w**


### *Question 9) Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?*

> Non, en effet : "Tout compte créé sans mot de passe est inactif, jusqu’à l’attribution d’un mot de passe" selon le cours

### *Question 10) Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.*

> Pour activer un utilisateur, il faut donc lui attribuer un mot de passe : 

```bash
sudo passwd u1
#entrer le mot de passe et le confirmer
```

> Puis pour se connecter à l'utilisateur u1 il suffit d'entrer la commande : 

```bash
su u1
```

### *Question 11) Quels sont l’uid et le gid de u1 ?*

> Pour obtenir ces IDs, on fait :

```bash
$ id u1

uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)
```

### *Question 12) Quel utilisateur a pour uid 1003 ?*

> Les IDs sont itérés de 1 à partir de 1001 pour chaque utilisateurs créé. C'est donc u3.


### *Question 13) Quel est l’id du groupe groupe1 ?*

> Avec la commande précédente, on peut voir le nom et le numéro de groupe. On sait donc que c'est 1001.


### *Question 14) Quel groupe a pour guid 1002 ?*

> C'est le groupe 2 car les itérations se font de la même manière. On peut aussi le vérifier **id u2**.


### *Question 15) Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez*

> Il faut entrer le mot de passe de l'admin pour pouvoir supprimer l'utilisateur du groupe.
On peut vérifier comme en question 4 que le groupe groupe2 ne contient plus u3, mais le dossier groupe2 en reste propriétaire. 
Les membres de groupe2 ont donc toujours les mêmes droits qu'avant sur u3.


### *Question 16 ) Modifiez le compte de u4 de sorte que :*

*— il expire au 1er juin 2020*

*— il faut changer de mot de passe avant 90 jours*

*— il faut attendre 5 jours pour modifier un mot de passe*

*— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe*

*— le compte sera bloqué 30 jours après expiration du mot de passe*


> Les commandes sont respectivement : 

```bash
sudo chage -E 2020-06-01 u4
sudo chage -d 90 u4
sudo chage -m 5 u4
sudo chage -W 14 u4
sudo chage -I 30 u4
```


### *Question 17) Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?*

> Pour connaitre l'nterpréteur de commande du root il faut appliquer la commande :

```bash
head -1 cat /etc/passwd
```
> On obtient la ligne 

```bash
root:x:0:0:root:/root:/bin/bash
```
> Comme indiqué dans le cours, le dernier paramètre est l'interpréteur de commande, soit "/bin/bash".

### *Question 18) à quoi correspond l’utilisateur nobody ?*

> Nobody est une pseudo utilisateur qui en représente un n'ayant aucun privilège, n'étant propriétaire d'aucun fichier et n'appartenant à aucun groupe.


### *Question 19) Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?*
### *Quelle commande permet de forcer sudo à oublier votre mot de passe ?*

> Selon ask ubuntu.com, par défaut sudo le garde en mémoire 15 minutes.
C'est la commande **sudo –k** qui permet d'empêcher sudo de garder notre mot de passe.



# **Exercice 2 : Gestion des permissions**

### *Question 1) Dans votre $HOME, créez un dossier* **test** *, et dans ce dossier un fichier* **fichier** *contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?*

> On utilise les commandes suivantes :
```bash
mkdir test
touch fichier
cat > fichier
(entrer les lignes de texte puis CTR+C)
ls -l #affiche les droits de fichier.txt : -rw-r--r--
cd ..
ls -l #affiche les droits de tous les dossier dans home dont "test" : drwx-r-xr-x
```

> Les lettres r, w, x correspondent respectivement à un droit de lecture, d'écriture et d'éxécution.
> La lettre d et le caractère - en début de ligne expriment que les droits traitent respectivement d'un dossier et d'un fichier "normal".


### *Question 2) Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher en tant que root. Conclusion?*

> On utilise les commandes suivantes :
```bash
cd test/
chmod 000 fichier
cat > fichier
permission denied
sudo cat > fichier 
permission denied
sudo su
cat > fichier
??? #modification
cat fichier.txt
??? #affichage du fichier modifié
exit #On sort du mode root
```

> En tant que root, il reste possible de modifier le fichier. Par défaut, root a les droits rw sur tous les fichiers malgré les permissions d'utilisateur, de groupe et des autres.


### *Question 3) Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande :
```bash
chmod 300 fichier
ls -l
echo "echo Hello" > fichier
```
### On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits?*
> Le remplacement du contenu d'un fichier correspond à de l'écriture. C'est pourquoi il nous est possible de le faire puisqu'on a récupéré les droits d'écriture.
> En revanche nous n'avons pas le droit de lecture, il faut donc écrire **sudo cat fichier** pour pouvoir le lire.


### *Question 4) Essayez d’exécuter le fichier. Est-ce que cela fonctionne? Et avec sudo? Expliquez.*
```bash
./fichier
permission denied
sudo ./fichier
Hello
```
> La commande ne fonctionne pas sans sudo car l'utilisateur n'a pas la permission en lecture. On ne peut pas lire le code sans être super utilisateur.

### *Question 5) Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou aﬀichez le contenu du fichier fichier. Qu’en déduisez-vous? Rétablissez le droit en lecture sur test.*
```bash
sudo chmod u-r test
cd test/
ls
fichier
./fichier
permission denied
```
> Le fichier fichier est à l'intérieur du dossier test et l'utilisateur n'a pas la permission de lecture sur ce dossier. On en déduit que cette abscence d'autorisation est la même pour le contenu du dossier.
> Nous ne pouvons pas exécuter ou lire. Nous n'avons pas les droits correspondant sur fichier. En revanche si on retire la lecture sur le répertoire test mais qu'on le récupère directement sur le fichier, alors on peut afficher et exécuter. On en déduit que les règles les plsu spécifiques prennent le dessus.

### *Question 6) Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez vous déduire de toutes ces manipulations?*
```bash
sudo chmod a-w test #retire le droit en ecriture
cd test

touch nouveau
mkdir test

sudo chmod a-w nouveau #retire le droit en ecriture


cat > nouveau
permission denied
sudo cat > nouveau
permission denied


cd ..
sudo chmod a+w test

cd test
cat > nouveau
permission denied
sudo cat > nouveau 
permission denied
rm nouveau #fichier supprimé
```

> On déduit de ces résultats que le droit d'écriture sur le fichier est nécessaire à sa modification mais pas à sa suppression. La suppression d'un fichier n'est apparemment pas liée au droit d'écriture.

### *Question 7) Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc… Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires?* 
```bash
cd
sudo chmod a-x /home/test

rm /home/test/fichier
permission denied
touch /home/test/fichier2
permission denied
cat > /home/test/fichier2
permission denied
cd /home/test
permission denied
ls -l /home/test #affiche les fichiers et les dossiers mais pas leurs infos sur les droits ou leur groupe/utilisateur
```
> Nous comprenons qu'une autorisation sur un fichier se répecture sur ses sous-dossiers, mais pas l'inverse. Globalement, un droit d'exécution sur le répertoire permet d'effectuer les actions de base.


### *Question 8) Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant? Peut-on retourner dans le répertoire parent avec ”cd ..”? Pouvez-vous donner une explication?*
```bash
sudo chmod a+x /home/test
cd /home/test
sudo chmod a-x /home/test

touch file
permission denied
rm fichier
permission denied
cd sstest
permission denied
cat > fichier
permission denied
ls sstest
permission denied

cd ..
#Pas d'erreur, cela fonctionne bien
```
> L'abscence de droits sur le répertoire courant bloque quasiment toutes les actions sauf le déplacement vers le répertoire parent. Cela permet de ne pas rester totalement bloqué dans un dossier desquels on n'a pas (ou plus) les droits.

### *Question 9) Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suﬀisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.*

> Sachant que les droits actuels de fichier sont : - -wx------ on exécute :
```bash
sudo chmod a+x test
cd test
chmod g+r fichier
```

### *Question 10) Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.*
> Le umask doit être : 077
```bash
umask 077
touch fichier2
mkdir test2
ls -l 
```
> Les droits pour fichier 2 et test2 sont respectivement : -rw------- et test2 : drwx------
> Pour réinitialiser le masque, on doit fermer la session.

### *Question 11) Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.*

### *Question 12) Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.*

### *Question 13) Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :*
- *chmod u=rx,g=wx,o=r fic*
- *chmod uo+w,g-rx fic* en sachant que les droits initiaux de fic sont r--r-x--
- *chmod 653 fic* en sachant que les droits initiaux de fic sont 711
- *chmod u+x,g=w,o-r fic* en sachant que les droits initiaux de fic sont r--r-x--14.
### Aﬀichez les droits sur le programme passwd.
### Que remarquez-vous? En aﬀichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd?




















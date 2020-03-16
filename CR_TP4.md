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

*Question 10) Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.*

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

### *Question 1) Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?*






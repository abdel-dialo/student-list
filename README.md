Prénom: Abdoul Gadirou

Nom: DIALLO

Promotion: BootCamp DevOps 17

# Mini-Projet Docker

Mini projet réalisé dans le cadre du Bootcamp Devops chez Easytraining.fr

![image](https://github.com/abdel-dialo/student-list/assets/58465298/520dc144-4844-4a56-9282-5c2970228346)

# Application: Student-list

**Student-list** est une simple application de l’entreprise POZOS construite avec deux modules :

- Le premier module est une _API REST_ écrit en _Flask_ qui renvoie un fichier _JSON_ contenant la
    liste des étudiants et leur âges
- Le deuxième module est une application web écrite en HTML + PHP qui permet d’afficher la
    liste des étudiants sur un navigateur web.

# Objectif du mini projet

- Construire un conteneur docker pour chaque module (API et Web)
- Faire interagir les deux conteneurs
- Pousser l’ image de l'API REST dans un registre privé

**Livrable**

- Dockerfile
- docker-compose.yml
- Docker-compose.registry.yml
- LISEZ-MOI.md

# Infrastructure Hébergeant nos conteneurs
Comme il nous a été recommandé j’ai utilisé une machine virtuelle centos7 et j’ai utilisé Vagrant pour
provisionner cette machine virtuelle sur Virtualbox.
Ci-dessous le fichier Vagrantfile fourni

![image](https://github.com/abdel-dialo/student-list/assets/58465298/b9980960-9dd4-441b-9bb1-bd4afaca3375)

# Récupération du code source depuis Github

Après avoir cloner le projet se déplacer dans le répertoire _student-list_ ajouter notre dépôt distant et
pousser le code sur notre dépôt distant
![image](https://github.com/abdel-dialo/student-list/assets/58465298/9a29908d-3945-4ba9-8f7e-9a39876226fa)
![image](https://github.com/abdel-dialo/student-list/assets/58465298/5ef5d7eb-de03-43e7-abf8-9ef5ca85c3e9)
![image](https://github.com/abdel-dialo/student-list/assets/58465298/5e0a4691-f03e-4ae8-acdb-c50503fd4986)


# Build de l’image API REST

- Se déplacer dans le répertoire _/studient-list/simple_api_
- Editer le _Dockerfile_ en rajoutant les instructions permettant de builder l’image de l’API REST
- Builder l’image

  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/d22f161d-c475-40e2-b624-c442790408ff)

# Lancement du conteneur API REST
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/8a8de603-2936-49c0-909d-980f817c7f52)

# Test de l’API REST
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/7fb7c444-6ef7-45af-bf93-82e8dc7b6d06)


# Déploiement de l’application avec une infrastructure as code :

Maintenant que j’ai testé l’image de notre _API REST_ et vu que l’image _php:apache_ est présent
sur la plateforme _dockerhub_ je vais éditer le fichier _docker-compose.yml_ pour y mettre les
commandes permettant d'exécuter les conteneurs des deux modules (API RES et Web) de l’application
_student-list_ :

- Concernant l’image j’ai mis la version 7.2 de l’image _php:apache_ dans le but d’éviter d’avoir
    toujours la version _latest_ qui pourrais être la cause de problème lors du build notamment
    dans le cas où il y aurait une montée de version


- Le conteneur _student_list_api_ doit se lancer avant le conteneur _pozos_website_ pour cela j’ai
    rajouté l’option _depends_on_ dans le fichier _docker-compose.yml_
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/77048f66-822e-47af-99ce-b463bec5a33a)

- J’ai rajouté également un réseau qui permet aux deux conteneurs de communiquer entre eux.
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/68361079-5df8-40be-8a2a-5a132deb92a2)


# Déploiement avec docker-compose

- Lancement des conteneurs à partir du docker-compose.yml (supprimer le conteneur _student_list_api_ avant)
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/cc6c1546-f0cd-4e8a-9afd-adb889884b3e)

- Vérifier le lancement des conteneurs
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/cceba10f-b0a7-441c-a951-14db65933586)

- Editer le fichier _index.html_ et mettre à jour la valeur de l’url de l’api
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/8fab80d0-315e-430e-85cc-cb1e36c63823)


# Vérifier le fonctionnement de l’application

- Afficher l’adresse IP du serveur hôte
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/a844423e-02e5-458e-8a8c-69eed22aac4f)

- Ouvrir l’application avec l’adresse de la VM sur le port 80 (port par default)
   ![image](https://github.com/abdel-dialo/student-list/assets/58465298/1c2f1bdb-b293-4e33-a967-041e42fba5d1)

   ![image](https://github.com/abdel-dialo/student-list/assets/58465298/fe41c1e3-2730-49c3-8738-61602db53a15)


# Création du registre privé

Pour la création du registre j’ai utilisé un fichier docker-compose.registry.yml qui contient les deux services pozos_registry et frontend-registry:

- Pour éviter de perdre les images hébergées dans le conteneur _pozos_registry_ j'ai monté le
répertoire _/data/registry_ dans le répertoire _/var/lib/registry_ du conteneur
![image](https://github.com/abdel-dialo/student-list/assets/58465298/99d57bcb-7b87-4655-a209-5bd374982501)

- Pour sécuriser l’accès au registre j’ai rajouté une authentification à l’aide du fichier
htpasswd par conséquent pour se connecter voici les accès à notre registre
privé : Username=pozos - password=pozos-registry
 ![image](https://github.com/abdel-dialo/student-list/assets/58465298/a55bfb76-6b9c-4627-8304-4c0713cb3d09)
  
- Lancement des conteneurs pozos_registry et frontend_registry
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/696fb52b-7eca-460f-a52b-bd97caedbf61)

- Affichage des conteneurs crées
  ![image](https://github.com/abdel-dialo/student-list/assets/58465298/ec10cdb7-3114-4e6c-890f-732b0d1c6e1f)


# Pousser l’image de l'API REST dans notre registre privé

- Se connecter à notre registre et pousser notre image

 ![alt text](images/push-image.png)

  ![alt text](images/registry.png)


# Conclusion

Ce mini-projet m’a permis de comprendre et de mettre en pratique l’écosystème
docker notamment la construction des images docker, le déploiement
d’infrastructure As Code avec docker-compose et le stockage des images docker dans
un registre privé, de comprendre aussi le lien entre les architectures micro-service et
docker.

Cela me permettra de mettre en pratique ces connaissances dans des projets en
entreprise.

C’est aussi une bonne base qui va me permettre d’aller plus loin dans les
technologies DevOps à savoir la sécurité, l’orchestration, le CI/CD et l’automatisation.





# Gestion de Parc d'Automates (projet-pwa-automate)

**Année universitaire :** 2022-2023  
**Cours :** Programmation Web Avancée  
**Établissement :** Institut Galilée  

**Étudiants :**  
- Abdoulaye Barry  
- Chloé Hamilcaro  

**Encadrant :**  
- M. Benoit Villa

---

## Description du Projet

Ce projet consiste à développer une application de gestion de parc d'automates, permettant de suivre l'état, l'emplacement, et la maintenance des automates. L'application est basée sur le modèle MVC et utilise les technologies Java EE, JSP/Servlet, et Hibernate pour la gestion des données.

L'application permet de :
- Créer, modifier, supprimer et consulter des automates.
- Gérer des fiches associées aux automates, contenant des informations variées (emplacement, statut, etc.).
- Suivre les ventes réalisées par chaque automate et gérer le réapprovisionnement en fonction de données météo.

---

## Architecture et Technologies Utilisées

### Environnement et Outils
- **Eclipse IDE** pour le développement.
- **JDK 11** pour compiler le code Java.
- **Apache Maven 3.8.7** pour la gestion des dépendances et la configuration du projet.
- **Tomcat 10.0.13** comme serveur de déploiement.

### Base de Données
- **Oracle XE 21c** pour la gestion des données.
- **Hibernate (JPA)** en tant qu'ORM (Object Relational Mapping) pour simplifier la persistance des données.

### Modèle de Développement
- **Architecture MVC** : Les vues sont gérées par des fichiers JSP tandis que les services (contrôleurs) sont implémentés à l'aide de Servlets.
- Utilisation de **CriteriaBuilder** pour écrire des requêtes de manière sécurisée et éviter les injections SQL.

### API et Intégrations
- **OpenWeatherMap API** : Pour obtenir les données météo, intégrées via des requêtes JSON, afin de déterminer le seuil de réapprovisionnement des produits selon la localisation GPS des automates.

---

## Fonctionnalités

1. **Gestion des Automates** :  
   - Créer, lire, mettre à jour et supprimer des automates.
   - Associer une fiche unique à chaque automate pour gérer ses détails comme l'emplacement.

2. **Persistance des Données** :  
   - Les données sont stockées dans une base Oracle et accessibles via Hibernate/JPA.
   - Utilisation de requêtes complexes (via CriteriaBuilder) pour gérer les contraintes des automates, telles que l'unicité des fiches.

3. **Intégration API Météo** :  
   - L'accès aux données météo se fait par **OpenWeatherMap API** pour permettre une gestion intelligente du réapprovisionnement.

4. **Envoi de Rapports XML/JSON** :  
   - Création de classes permettant d'envoyer des rapports sous forme XML/JSON vers une URL configurable pour suivre l'état des modules télécoms associés aux automates.
   - Gestion des réceptions de codes HTTP et tentatives de renvoi automatique en cas d'erreur.

---

## Choix d'Implémentation

- **Modèle de Fiche d'Automate** :  
  Les attributs tels que l'emplacement et les coordonnées GPS sont intégrés à la fiche d'un automate, permettant ainsi la mobilité d'un automate sans modification complexe de sa structure.
  
- **Modules de Télécommunication** :  
  Un même module peut être affecté à plusieurs automates, représentant plus un type de module qu'une instance physique.

- **Énumérations pour Statut et État** :  
  Les états de l'automate (état actuel, état du monnayeur, etc.) sont gérés par des énumérations, sans persistance dans la base de données.

---

## Problèmes Rencontrés

1. **Conflit de Dépendances** :  
   - Le sujet demandait l'utilisation de webservices pour l'accès aux données, mais des conflits de dépendances Maven ont été rencontrés. Pour ne pas retarder le développement, nous avons opté pour une solution basée sur des Servlets.

2. **Révision de l'Architecture** :  
   - La modélisation des données a nécessité plusieurs itérations pour aboutir à une architecture cohérente. Cela a rallongé le temps de développement mais a permis une meilleure adéquation entre les exigences fonctionnelles et les choix techniques.

3. **Envoi et Réception de Rapports** :  
   - La réception et le stockage des rapports XML/JSON dans la base de données ont posé problème. Le modèle JPA n'a pas pu être intégré correctement, et des difficultés ont été rencontrées pour tester l'envoi via **RESTClient**.

---

## Technologies et Outils

- **Langages de Programmation** : Java, JSP
- **Serveur d'Application** : Apache Tomcat 10.0.13
- **Gestion de Base de Données** : Oracle XE 21c, Hibernate (JPA)
- **Outils** : Eclipse IDE, Apache Maven
- **API** : OpenWeatherMap
- **Protocole** : HTTP, JSON/XML pour l'échange de données

---

## Pour Commencer

1. Clonez ce repository :
   ```sh
   git clone https://github.com/JustBarry6/projet-pwa-automate.git
   ```
2. Importez le projet dans Eclipse en tant que projet Maven.
3. Configurez un serveur Tomcat dans Eclipse et déployez l'application.

---

## Auteurs

- **Abdoulaye Barry**  
- **Chloé Hamilcaro**

---

## Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

## Remerciements

Nous remercions **M. Benoit Villa** pour son soutien tout au long du projet.

# projet-pwa-automate
Applicatin de gestion de parc d'automate
Année universitaire 2022-2023
Ingénieur Informatique

Programmation Web Avancée

Étudiants :
Abdoulaye Barry
Chloé Hamilcaro

Enseignant :
M. Benoit VillaI. OUTILS ET TECHNOLOGIES


Nous avons effectué ce projet à l’aide de l’environnement de développement Eclipse, sur
lequel nous avons utilisé l’ensemble de bibliothèques logicielles JDK 11. Nous avons utilisé
l’outil Apache Maven 3.8.7, ainsi qu’un serveur Tomcat 10.0.13 pour le développement de
cette application web.
B. Architecture globale
Notre application est développée en MVC. Les vues sont contenues dans des fichiers JSP,
tandis que les services (contrôleurs) utilisent des Servlet.
C. Base de données
Nous avons utilisé le système de gestion de bases de données Oracle XE 21c, ainsi que
Hibernate, une implémentation de JPA, en tant qu’ORM pour créer et accéder aux données
de la base. JPA nous a également permis de gérer facilement la persistence de données.
Pour interroger les données, nous avons utilisé des requêtes simples, comme par exemple
SELECT a FROM Automate a ORDER BY a.montantVentes DESC mais également des
requêtes programmées grâce à CriteriaBuilder afin de créer des requêtes plus lisibles et
d’éviter les injections SQL.
Par exemple, nous avons déterminé qu’une fiche d’un automate ne serait reliée qu’à un seul
automate. Par conséquent, il est inutile de laisser à l’utilisateur la possibilité de choisir, dans
la liste d’automates à associer, un automate qui possède déjà une fiche : cela aurait été
refusé par la base de données quoiqu’il en soit. Ainsi, nous avons le code suivant dans la
classe AutomateDao :

public List<Automate> findAllHasNotFiche() {
EntityManager em = factory.createEntityManager();
List<Automate> exclusion = new ArrayList<Automate>();
List<FicheAutomate> fiches =
Gestionnaire.getInstance().getFicheAutomateDao().findAll();
// Liste des automates à exclure
for (FicheAutomate f : fiches)
exclusion.add(f.getAutomate());
// Utilisation d'un CriteriaBuilder
CriteriaBuilder builder = em.getCriteriaBuilder();
CriteriaQuery <Automate> query = builder.createQuery(Automate.class);
Root<Automate> p = query.from(Automate.class);
query.select(p);
query.where(builder.not(p.in(exclusion)));
// Retour du résultat
return em.createQuery(query).getResultList();
}
  
Ce code récupère tout d’abord la liste des automates ayant déjà une fiche : il s’agit de la
liste des automates à exclure. Puis, une requête programmée grâce à CriteriaBuilder et
CriteriaQuery récupère les automates qui ne se trouvent pas dans la liste d’exclusion.
3D. XML/JSON
L’accès aux données météos se fait par le biais de l’API OpenWeatherMap. Les données
de cette api étant accessibles via Internet en JSON, nous avons dû extraire ces données
grâce à la bibliothèque org.json afin de pouvoir les utiliser pour calculer le seuil de
réapprovisionnement des produits en fonction de la position GPS des automates.
II. CHOIX D’IMPLÉMENTATION
Nous avons décidé de développer l’application en nous basant sur le modèle suivant :
Ainsi, nous avons déterminé que les coordonnées GPS, l’emplacement et l’adresse d’un
automate étaient variables et ne dépendent pas directement de celui-ci : un automate peut
être déplacé, ce changement sera signalé par l’utilisateur dans la fiche de l’automate, par
conséquent ces attributs sont dans la fiche de l’automate. Par la même logique, le montant
des ventes se trouve dans l’automate et non la fiche, car il dépend de l’interaction des
consommateurs avec la machine et ne devrait pas être modifié directement par l’utilisateur
de l’application web.
Les états (état actuel, état du monnayeur…) ainsi que le statut d’un automate sont des
énumérations, que nous n’avons pas jugé utile de persister dans la base de données mais
qui disposent néanmoins de classes de type énumération.
Pour simplifier l’architecture, nous avons décidé qu’un même module de télécommunication
pouvait être affecté à plusieurs automates, car il s’agit en réalité plus d’une représentation
d’un type de module que d’un module en particulier.
  
  
PROBLÈMES RENCONTRÉS
Le sujet demandait d’utiliser un webservice pour l’accès aux données, malheureusement
nous avons rencontré des problèmes de conflits entre les dépendances que nous n’avons
pas su résoudre. Pour éviter de perdre trop de temps dans le développement des
fonctionnalités attendues, nous n’avons pas utilisé de webservice et nous nous sommes
contentés d’utiliser des Servlet.
Nous avons également passé beaucoup de temps à réfléchir aux données et aux classes
qui allaient les représenter : au fur et à mesure que nous avancions dans le projet, nous
avons dû revoir notre architecture de classes, ce qui a rallongé notre temps de
développement. Cependant, même si cela nous a pris du temps, nous avons réussi à
aboutir à une architecture qui nous convient et convient aux choix d’implémentation que
nous avons faits.
7En ce qui concerne l'envoi de rapports XML/JSON, nous avons conçu des classes
(ModuleTelecomT1, ModuleTelecomT2 et une Servlet) pour envoyer des rapports sous
forme textuelle à une URL configurable. Le module de rapport envoie un message à l'URL
spécifiée et reçoit un code HTTP 200 pour confirmer que le message a été correctement
reçu. Dans le cas contraire, le module réessayera l'envoi 5 minutes plus tard jusqu'à ce
qu'un code HTTP 200 soit reçu.
Cependant, nous avons rencontré des difficultés lors de la réception et du stockage des
données de rapport dans une base de données afin de permettre aux équipes de gestion et
de maintenance de les visualiser via une application Web. Nous avons essayé d'utiliser le
modèle JPA (Java Persistence API) pour stocker les données dans une base de données,
mais on n'a pas réussi à trouver une solution pour intégrer les données reçues dans la base
de données ni même tester leur envoi grâce à RESTClient.

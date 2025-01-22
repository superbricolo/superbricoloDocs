Présentation
============

Ce plugin suiviCO2 pour Jeedom a deux fonctions principales :
- Disposer de la valeur actuelle de gCO2 par kWh électrique produit, émis en France, en temps réel. De façon à pouvoir conditionner ses équipements facultatifs (retarder un peu le chauffe-eau en HC par exemple)
- Visualiser ses émissions de CO2 liées à sa consommation électrique, de gaz, fioul ou autre (ainsi que la consommation et les coûts associé) :

![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/PanneauDesktop.png)

Les données de CO2 par kWh produit sont celles fournies par RTE, plus d'infos ici : <a href="https://www.rte-france.com/fr/eco2mix/eco2mix-co2" target="_blank">eco2mix-co2</a>


Configuration du plugin
========================

Après téléchargement du plugin, il vous faut l’activer, et cocher la case "Afficher le panneau desktop".

Configuration des équipements (Mes sources d'émission CO2)
=================================================

Une fois le plugin activé, il est visible dans le menu "Plugin"/"Energie".

Vous pouvez alors définir plusieurs "sources d'émission CO2". Chacune est indépendante.

Onglet Equipement
-----------------
Pour que cet équipement soit visible dans le panneau desktop, il faut cocher les cases "Activer" et "Visible".
La case "Visible" permet de définir la visibilité du widget sur le dashboard Jeedom :
![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/widget.png)

### Configuration pour équipement électrique :
![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/OngletEquipementElec.png)
- Choisir Type d'énergie "Electricité"
- Si vous avez le plugin Suivi Conso installé et actif sur votre Jeedom, le plugin vous proposera de choisir un équipement suiviconso pour configurer suiviCO2. La configuration reprise sera les commandes d'index HP et HC ainsi que le coefficient si besoin.
- Index de consommation : à fournir en Wh (unité de base de la téléinformation). Si vous n'avez pas d'heures creuses, laisser le champs HC vide. Si votre consommation n'est pas en Wh, vous pouvez remplir le champ "Coefficient" permettant de réaliser la conversion d'unité.

### Configuration pour équipement de type gaz, fioul ou autre :
![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/OngletEquipementOther.png)
- Choisir Type d'énergie voulu
- Fournir la valeur de g de CO2 émis par kWh consommé. Ce champs n'est pas à fournir pour l'électricité car l'information provient alors d'une API avec actualisation toutes les 15 min. Cette valeur usuelle pour votre type d'énergie se trouve sur internet.
- Index de consommation à fournir en Wh. Si vous n'avez pas d'heures creuses, laisser le champs HC vide. Si votre consommation n'est pas en Wh, vous pouvez remplir le champ "Coefficient" permettant de réaliser la conversion d'unité. Ce Coefficient est normalement donné sur votre facture, il dépend notamment de votre région.

Onglet Coûts
-----------------

![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/OngletCout.png)
- Remplir vos coûts d'abonnement et de consommation par kWh

Laissez les champs vide pour ne pas afficher les calculs et graphiques de coûts sur le panel.

Onglet Commandes
-----------------

![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/OngletCommandes.png)

Les commandes sont automatiquement créées à la sauvegarde de l'équipement.
Les commandes présentes dépendront du type d'énergie choisi et de votre configuration HP/HC.
Vous pouvez définir leur visibilité et min/max pour affichage sur le dashboard. Il est recommandé de ne pas changer l'historisation des commandes.
Les commandes disponibles à afficher sur le widget sont :
- Le total gCO2 du jour (depuis minuit)
- Le total gCO2 de la semaine (depuis minuit lundi)
- Le total gCO2 du mois (depuis minuit le premier jour du mois courant)
- Le taux de CO2 par kWh émis par la production française, en temps réel. Uniquement si votre équipement est de type "Electrique"

![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/widget.png)

Onglet Historique
--------------

L'onglet historique sera différent selon le type d'énergie et si vous disposez ou non du plugin suivi conso.

Au maximum :

![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/OngletHistorique.png)

Pour l'électricité, le plugin chargera les données de l'API toutes les heures, il est possible de charger un historique plus ancien via les boutons suivants :

- Données temps réel CO2 par kWh en France : cette commande va chercher les données de l'API "temps réel", c'est à dire les données prévisionnelles, par 15 min, qui sont mises à jour toutes les heures. Cette commande permet de récupérer la totalité des données présentes sur le serveur, c'est à dire environ 1,5 mois de données. Le temps de chargement prend environ 1 min avec un RPI3 et une connexion internet correcte.

- Données consolidées et définitives CO2 par kWh en France : cette commande va chercher les données de l'API "consolidées et définitives", c'est à dire des données plus proches de la réalité suite à la prise en compte d'informations plus complètes et détaillées. Ces informations sont mises à jour tous les jours et contiennent toutes les dates antérieures aux données "temps réel" depuis janvier 2012. Pour limiter le temps de chargement (environ 20s avec un RPI3 et une connexion internet correcte), il est demandé de récupérer les informations mois par mois.

Pour tous les types d'énergie, le plugin calculera et enregistrera les consommations toutes les heures, toutefois vous pouvez récupérer dans cet onglet les données passés. Pour les énergies de type gaz, fioul ou autre, le coefficient thermique sera appliqué lors de l'import de façon à stocker des valeurs en Wh.

- Ma conso Wh : uniquement si les commandes contenant les index étaient déjà historisées dans jeedom. Cette commande permet avec les données d'index historisées de calculer et d'enregistrer vos conso HP et HC pour les visualiser dans le panneau desktop. Il est possible de choisir la période voulue, attention, les données peuvent être longues à charger. Merci @jpty qui a optimisé cette fonction !

Il est possible de relancer l'historique sur des dates déjà enregistrées.

Lors de la création de l'équipement, il est possible que la 1ere valeur de consommation HP/HC soit manquante, vous pouvez alors relancer l'historisation des données sur cette journée pour la récupérer.

Si vous avez le plugin suivi conso installé, actif, et uniquement pour les équipements de type "électrique" :

-  Récupérer historique de ma conso électrique via le plugin SUIVI CONSO : vous pouvez choisir l'équipement suivi conso correspondant et recupérer les historiques enregistrés via ce plugin.

Utilisation du panneau desktop
======================

Vous pouvez sélectionner en haut à droite la période a visualiser ainsi que le regroupement des infos à faire sur les graphs.

Panneau d'affichage pour l'électricité avec des coûts déclarés :

![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/PanneauDesktop.png)

Panneau d'affichage pour l'électricité sans coûts déclarés :

![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/PanelElecNoCost.png)

Panneau d'affichage pour le gaz ou fioul ou autre avec des coûts déclarés :

![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/PanelGazCout.png)

Panneau d'affichage pour le gaz ou fioul ou autre sans coûts déclarés :

![](https://raw.githubusercontent.com/superbricolo/suiviCO2/master/docs/assets/images/PanelGazNoCost.png)


API
======

Ce plugin utilise les données nationales fournies par RTE : <a href="https://opendata.reseaux-energies.fr/explore/dataset/eco2mix-national-tr/information/?disjunctive.nature" target="_blank">https://opendata.reseaux-energies.fr/</a>

Support
===

* Pour toute demande de support ou d'information : [Forum Jeedom](https://community.jeedom.com/c/plugins/energy/50)
* Pour un bug ou une demande d'évolution, merci de passer de préférence par [Github](https://github.com/AgP42/suiviCO2/issues)

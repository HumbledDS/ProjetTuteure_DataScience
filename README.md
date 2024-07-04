# Reseaux Neurones : Simulation Trading sur SP500


## Projet de Simulation de Trading avec Réseaux de Neurones LSTM

Ce projet tuteuré vise à développer et évaluer un système de simulation de trading utilisant des réseaux de neurones LSTM pour prédire les prix des actions dans le cadre DU BIG DATA avec comme tuteur Mr Gilles Michel. Voici les étapes principales du processus :

### Étapes du Projet

1. **Collecte des Données :**
   - Utilisation de l'API Yahoo Finance via `yfinance` pour récupérer les prix de clôture quotidiens des actions pour l'année 2022.
   - Stockage des données dans `closing_prices_2022.csv` pour assurer la disponibilité des données nécessaires.

2. **Préparation des Données :**

Cette phase vise à transformer les données de clôture brutes en un format adapté à l'analyse et à la prédiction par des modèles LSTM (Long Short-Term Memory). La normalisation des données est essentielle ici : en utilisant le MinMaxScaler, nous ajustons les prix de clôture pour qu'ils soient compris dans une échelle de 0 à 1. Cette étape est cruciale pour stabiliser et accélérer le processus d'entraînement des réseaux de neurones, en évitant les problèmes liés à des valeurs extrêmes.

   - Normalisation des prix de clôture à l'aide de `MinMaxScaler` pour les ajuster dans une échelle de 0 à 1.
   - Transformation des données en séquences temporelles de 30 jours pour capturer les motifs et tendances.

3. **Entraînement des Modèles LSTM :**

   - Construction de modèles LSTM avec deux couches LSTM suivies de couches denses pour la prédiction.
   - Utilisation de l'optimiseur Adam et de la fonction de perte MSE (Mean Squared Error) pour l'entraînement.
   - Validation rigoureuse pour ajuster les hyperparamètres et évaluer la capacité de généralisation.

4. **Simulation de Trading :**

Chaque modèle est construit avec deux couches LSTM suivies de couches denses pour la prédiction finale. Utilisant l'optimiseur Adam et la fonction de perte MSE (Mean Squared Error), l'entraînement se déroule sur les données préparées, avec un batch size de 1 pour une précision maximale. Cette phase inclut également une validation sur un ensemble distinct de données pour ajuster les hyperparamètres du modèle et évaluer sa capacité à généraliser les prédictions au-delà des données d'entraînement.

   - Utilisation des prédictions de prix pour simuler des décisions d'achat et de vente d'actions de septembre à décembre 2022.
   - Évaluation des performances des modèles en tenant compte des frais de courtage et de la gestion de portefeuille.

### Stratégie d'achat et vente basée sur les prédictions de prix :

1. **Prédiction des prix :**
   - Pour chaque action symbolisée par `symbol`, le script utilise un modèle LSTM préalablement entraîné pour prédire les prix des actions pour une période future définie (`start_date_prediction` à `end_date_prediction`).

2. **Simulation du trading :**
   - Une fois que les prix prévus sont disponibles, le script simule le trading en se basant sur ces prédictions. Il commence avec un investissement initial fixe (`initial_investment`) et un frais de courtage par transaction (`broker_fee`).

3. **Décisions d'achat et de vente :**
   - Lorsque le modèle prédit que le prix va augmenter par rapport à la période précédente, et si l'investisseur ne détient actuellement aucune action pour cet actif (`shares == 0`), alors l'investisseur achète des actions de cette action. La quantité achetée est calculée en utilisant une partie de l'investissement disponible, moins les frais de courtage.
   
   - Lorsque le modèle prédit que le prix va diminuer par rapport à la période précédente, et si l'investisseur détient des actions de cet actif (`shares > 0`), alors l'investisseur vend toutes les actions qu'il détient. Le produit de la vente est ajouté à l'investissement disponible, moins les frais de courtage.

4. **Gestion des transactions :**
   - Chaque transaction est enregistrée dans un DataFrame `trading_actions` qui stocke les détails de chaque transaction, y compris la date, l'action (achat ou vente), le prix de l'action à ce moment-là, le nombre d'actions achetées ou vendues, et l'investissement restant après la transaction.

5. **Répétition pour chaque symbole :**
   - Ce processus est répété pour chaque symbole d'action présent dans les données d'entraînement.

### Objectifs de la stratégie :

- **Optimisation du rendement :** L'objectif principal de cette stratégie est d'optimiser le rendement de l'investissement initial en achetant des actions lorsque le modèle prédit une hausse des prix et en vendant lorsque le modèle prédit une baisse des prix.
  
- **Réduction des risques :** La gestion des transactions est effectuée en tenant compte des frais de courtage, ce qui contribue à réduire les risques associés à des transactions fréquentes et à maximiser le profit net.

En pratique, l'efficacité de cette stratégie dépend largement de la précision des prédictions du modèle LSTM. Des améliorations peuvent être apportées en ajustant les hyperparamètres du modèle, en utilisant des techniques de prétraitement des données plus avancées, ou en explorant d'autres modèles de prédiction de séries temporelles.

### Fichiers Fournis

- **`closing_prices_2022.csv` :** Contient les données de clôture quotidiennes des actions pour l'année 2022.
- **Fichiers de Modèles :** Inclus les modèles LSTM entraînés pour la prédiction des prix.
- **Fichier contenant les Simulation `results` :** 10 fichiers dont un par action choisie.


### Quelques résultats :

Le PnL final à la date du 31/12/2022 est de 26,53 millions nets (en enlevant 1 million alloué par actions (10M€) ainsi que les frais de commissions).
Le max draw down est de 0% et la valeur maximale du portefeuille pendant la période est de 82.32 millions.
Les actions sont choisies parmi le top du SP500.

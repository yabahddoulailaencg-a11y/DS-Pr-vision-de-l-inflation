# 10 projets de science des données orientés finance

**Niveau :** maîtrise **Durée :** environ 2 semaines **Langage :** Python (pandas, scikit-learn, matplotlib)

Les sources ont été vérifiées au moment de la rédaction. Les jeux de données UCI 350 (défaut de carte), UCI 222 (dépôts bancaires), UCI 572 (faillites) et le jeu Kaggle de fraude figurent déjà dans la liste générale de 40 projets et ne sont pas repris ici.

---

## Tableau récapitulatif

| N° | Titre | Type d'apprentissage | Difficulté | Source |
| :---- | :---- | :---- | :---- | :---- |
| F1 | Risque de crédit en Allemagne | Supervisé, classification à coût asymétrique | Débutant | UCI 144 |
| F2 | Prévision du taux à 10 ans | Supervisé, régression sur séries temporelles | Intermédiaire | FRED DGS10 |
| F3 | Prévision de l'inflation | Supervisé, régression sur séries temporelles | Intermédiaire | FRED CPIAUCSL, UNRATE, FEDFUNDS |
| F4 | Sentiment des nouvelles financières | Supervisé, classification de texte | Intermédiaire | Hugging Face, financial\_phrasebank |
| F5 | Facteurs de risque d'un portefeuille | Supervisé, régression linéaire | Intermédiaire | Ken French, facteurs Fama-French |
| F6 | Prédire les récessions avec la courbe des taux | Supervisé, classification déséquilibrée | Intermédiaire | FRED DGS10, DGS2, USREC |
| F7 | Anomalies sur le S\&P 500 | Non supervisé, détection d'anomalies | Débutant | FRED SP500 |
| F8 | Peut-on battre la marche aléatoire ? | Supervisé, classification avec référence | Intermédiaire | FRED SP500 |
| F9 | Régimes de marché par partitionnement | Non supervisé, partitionnement | Intermédiaire | FRED SP500 |
| F10 | Gestion de position par renforcement | Renforcement, Q-learning ou DQN | Avancé | FRED SP500 |

---

## Détail des projets

### F1. Risque de crédit en Allemagne

- **Question :** quels attributs du client et du prêt permettent d'identifier les mauvais risques ?  
- **Tâche :** classification binaire, avec coûts d'erreur asymétriques.  
- **Données :** UCI 144, [https\://archive.ics.uci.edu/dataset/144](https://archive.ics.uci.edu/dataset/144). 1 000 dossiers, 20 variables, 300 mauvais risques.  
- **Prétraitement :** encodage des variables catégorielles, standardisation des montants et des durées.  
- **Modèles :** régression logistique (référence), arbre de décision, forêt aléatoire.  
- **Métriques :** AUC et coût total. Fixer la matrice de coûts (par exemple : accepter un mauvais risque coûte cinq fois plus que refuser un bon) et la justifier.  
- **Livrable :** notebook et courbe de coût selon le seuil de décision.  
- **Risque :** le seuil optimal dépend des hypothèses de coût. Présenter une analyse de sensibilité.

### F2. Prévision du taux à 10 ans

- **Question :** les taux passés permettent-ils de prévoir le taux à 10 ans un mois plus tard ?  
- **Tâche :** régression sur séries temporelles.  
- **Données :** FRED DGS10, [https\://fred.stlouisfed.org/graph/fredgraph.csv?id=DGS10](https://fred.stlouisfed.org/graph/fredgraph.csv?id=DGS10). Environ 16 900 observations quotidiennes depuis 1962\.  
- **Prétraitement :** traiter les valeurs manquantes (codées « . »), rééchantillonnage mensuel, création de retards.  
- **Modèles :** marche aléatoire (référence), régression linéaire avec retards, gradient boosting.  
- **Métriques :** RMSE et MAE, comparées à la référence.  
- **Livrable :** notebook et graphique prévision contre réel.  
- **Risque :** une fuite temporelle apparaît si le découpage est aléatoire. Utiliser une validation chronologique.

### F3. Prévision de l'inflation

- **Question :** le chômage et le taux directeur aident-ils à prévoir l'inflation d'un an sur l'autre ?  
- **Tâche :** régression sur séries mensuelles.  
- **Données :** FRED CPIAUCSL, UNRATE et FEDFUNDS, par exemple [https\://fred.stlouisfed.org/graph/fredgraph.csv?id=CPIAUCSL](https://fred.stlouisfed.org/graph/fredgraph.csv?id=CPIAUCSL). Environ 950 observations mensuelles.  
- **Prétraitement :** calculer la variation annuelle de l'indice des prix, aligner les dates, construire des retards.  
- **Modèles :** dernière valeur (référence), régression Ridge, gradient boosting.  
- **Métriques :** RMSE sur un horizon de 12 mois.  
- **Livrable :** notebook et analyse de l'importance des variables.  
- **Risque :** les séries économiques sont révisées après publication. Les données actuelles ne sont pas celles disponibles à l'époque : c'est un biais d'anticipation (*look-ahead bias*). Le signaler dans le rapport.

### F4. Sentiment des nouvelles financières

- **Question :** un modèle de texte détecte-t-il si une phrase financière est positive, négative ou neutre ?  
- **Tâche :** classification de texte à trois classes.  
- **Données :** Financial PhraseBank, [https\://huggingface.co/datasets/takala/financial\_phrasebank](https://huggingface.co/datasets/takala/financial_phrasebank). Environ 4 800 phrases annotées, avec plusieurs niveaux d'accord entre annotateurs.  
- **Prétraitement :** nettoyage, TF-IDF avec unigrammes et bigrammes.  
- **Modèles :** régression logistique (référence), SVM linéaire, comparaison avec un modèle pré-entraîné si le temps le permet.  
- **Métriques :** F1 macro, matrice de confusion.  
- **Livrable :** notebook et analyse commentée des erreurs.  
- **Risque :** les annotations sont subjectives. Évaluer séparément les phrases à accord élevé et celles à accord faible.

### F5. Facteurs de risque d'un portefeuille

- **Question :** la rentabilité d'un portefeuille s'explique-t-elle par le marché, la taille et la valeur ?  
- **Tâche :** régression linéaire multiple, avec estimation de l'alpha et du bêta.  
- **Données :** facteurs Fama-French, [https\://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/F-F\_Research\_Data\_Factors\_CSV.zip](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/ftp/F-F_Research_Data_Factors_CSV.zip) (fichier zip à décompresser). Les rendements du portefeuille doivent être fournis par l'étudiant, par exemple depuis Yahoo Finance.  
- **Prétraitement :** aligner les dates mensuelles, convertir en rendements en excès.  
- **Modèles :** régression MCO avec tests de significativité ; comparaison à un modèle à un seul facteur.  
- **Métriques :** R² ajusté, alpha et son intervalle de confiance.  
- **Livrable :** notebook et tableau de résultats commenté.  
- **Risque :** un alpha positif peut venir du hasard. Constituer le portefeuille avec les entreprises actuelles crée un biais de survie.

### F6. Prédire les récessions avec la courbe des taux

- **Question :** l'écart entre les taux à 10 ans et à 2 ans annonce-t-il une récession dans les 12 mois ?  
- **Tâche :** classification binaire déséquilibrée.  
- **Données :** FRED DGS10 et DGS2 ([https\://fred.stlouisfed.org/graph/fredgraph.csv?id=DGS2](https://fred.stlouisfed.org/graph/fredgraph.csv?id=DGS2)), ainsi que l'indicateur de récession USREC ([https\://fred.stlouisfed.org/graph/fredgraph.csv?id=USREC](https://fred.stlouisfed.org/graph/fredgraph.csv?id=USREC)).  
- **Prétraitement :** calculer l'écart 10 ans moins 2 ans, agréger en mensuel, définir la cible « récession dans les 12 mois ».  
- **Modèles :** régression logistique sur l'écart seul (référence), puis avec retards et chômage.  
- **Métriques :** AUC, aire sous la courbe précision-rappel, rappel sur les périodes de récession.  
- **Livrable :** notebook et discussion du nombre limité d'épisodes de récession.  
- **Risque :** une dizaine de récessions seulement : résultats très fragiles. Ne pas surinterpréter.

### F7. Anomalies sur le S\&P 500

- **Question :** quelles journées de marché sont statistiquement atypiques, et correspondent-elles à des événements connus ?  
- **Tâche :** détection d'anomalies sans étiquettes.  
- **Données :** FRED SP500, [https\://fred.stlouisfed.org/graph/fredgraph.csv?id=SP500](https://fred.stlouisfed.org/graph/fredgraph.csv?id=SP500). Environ 2 500 observations quotidiennes depuis 2016\.  
- **Prétraitement :** calculer les rendements logarithmiques et la volatilité glissante sur 20 jours.  
- **Modèles :** Isolation Forest, LOF.  
- **Métriques :** proportion d'anomalies détectées, comparaison qualitative avec des événements documentés.  
- **Livrable :** notebook et frise chronologique des anomalies.  
- **Risque :** sans étiquettes, l'évaluation reste subjective. Le signaler clairement.

### F8. Peut-on battre la marche aléatoire ?

- **Question :** un modèle entraîné sur les rendements passés prédit-il le signe du rendement du lendemain mieux que le hasard ?  
- **Tâche :** classification binaire (hausse ou baisse), avec référence de base.  
- **Données :** FRED SP500, [https\://fred.stlouisfed.org/graph/fredgraph.csv?id=SP500](https://fred.stlouisfed.org/graph/fredgraph.csv?id=SP500).  
- **Prétraitement :** rendements retardés, cible binaire, découpage chronologique.  
- **Modèles :** prédiction de la classe majoritaire (référence), régression logistique, forêt aléatoire.  
- **Métriques :** exactitude comparée à la référence, ratio de Sharpe d'une stratégie simulée après coûts.  
- **Livrable :** notebook et conclusion sur l'efficience du marché.  
- **Risque :** tester beaucoup de modèles et ne garder que le meilleur (*data snooping*) donne des résultats trompeurs.

### F9. Régimes de marché par partitionnement

- **Question :** le marché alterne-t-il entre des régimes identifiables de faible ou forte volatilité ?  
- **Tâche :** partitionnement sur des indicateurs de rendement et de volatilité.  
- **Données :** FRED SP500, [https\://fred.stlouisfed.org/graph/fredgraph.csv?id=SP500](https://fred.stlouisfed.org/graph/fredgraph.csv?id=SP500) ; comparaison avec l'indicateur USREC.  
- **Prétraitement :** rendements et volatilité calculés sur fenêtres glissantes, standardisation.  
- **Modèles :** k-means (référence), GMM.  
- **Métriques :** silhouette, puis comparaison des régimes aux périodes de récession.  
- **Livrable :** notebook et graphique des régimes superposé au cours.  
- **Risque :** standardiser avec la moyenne et l'écart-type de toute la série introduit une information future. Standardiser uniquement avec une fenêtre passée.

### F10. Gestion de position par renforcement

- **Question :** un agent peut-il apprendre à passer de long à neutre à court selon l'historique des rendements ?  
- **Tâche :** apprentissage par renforcement avec environnement personnalisé.  
- **Données :** FRED SP500, [https\://fred.stlouisfed.org/graph/fredgraph.csv?id=SP500](https://fred.stlouisfed.org/graph/fredgraph.csv?id=SP500).  
- **Prétraitement :** état défini par des rendements et une volatilité passés ; récompense égale au rendement de la position moins un coût à chaque changement de position.  
- **Modèles :** Q-learning tabulaire (référence : position fixe), puis DQN simple.  
- **Métriques :** rendement cumulé, ratio de Sharpe, drawdown maximal, sur un jeu de test postérieur chronologiquement.  
- **Livrable :** notebook, environnement Gymnasium documenté, courbes de capital.  
- **Risque :** surapprentissage au passé très fort. Décrire les hypothèses simplificatrices (absence de frais, exécution au cours de clôture).

---

## Points à noter

1. Le jeu FRED SP500 ne couvre que les dix dernières années. Il suffit pour F7 à F10 mais limite les conclusions.  
2. Les liens FRED, Ken French et Hugging Face ont été testés et répondent. Pour F5, les rendements des portefeuilles doivent venir d'une source externe comme Yahoo Finance : cette source n'est pas une API officielle et peut changer sans préavis.  
3. Les projets de finance exigent une attention particulière aux biais de temps (fuite temporelle, biais d'anticipation) et aux biais de sélection. Ces points sont signalés dans chaque fiche.
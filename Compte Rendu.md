
# ZINEB EL MEJDOUBI
**Numéro d'étudiant** : 24010156
**Classe** : CAC2


# Compte rendu

📊 Analyse de l'Équilibre entre Usage des Réseaux Sociaux et Santé Mentale 

**Date :** 3 décembre 2025
# Task
## Rapport Final : Analyse des Facteurs Influencant l'Indice de Bonheur

## 1. Introduction

Ce rapport détaille une analyse complète des facteurs influençant l'indice de bonheur, basée sur le jeu de données "Mental_Health_Dataset.csv". L'objectif principal était de modéliser l'indice de bonheur à la fois comme une variable continue (régression) et comme une variable catégorielle (classification), afin d'identifier les prédicteurs clés et de comparer l'efficacité de différentes approches d'apprentissage automatique.

## 2. Méthodologie

### 2.1. Nettoyage et Préparation des Données

*   **Chargement des Données**: Le jeu de données initial a été chargé depuis KaggleHub.
*   **Gestion des Valeurs Manquantes et Doublons**: Aucune valeur manquante ni doublon n'a été identifiée dans le jeu de données initial ou après prétraitement, assurant une bonne intégrité des données dès le départ.
*   **Suppression de Caractéristiques Inutiles**: La colonne `User_ID`, identifiant unique sans valeur prédictive, a été supprimée.
*   **Encodage des Variables Catégorielles**: Les variables catégorielles `Gender` et `Social_Media_Platform` ont été transformées en variables numériques via *One-Hot Encoding*.
*   **Normalisation des Données Numériques**: Les caractéristiques numériques (`Age`, `Daily_Screen_Time(hrs)`, `Sleep_Quality(1-10)`, `Stress_Level(1-10)`, `Days_Without_Social_Media`, `Exercise_Frequency(week)`) ont été normalisées à l'aide de `StandardScaler` pour assurer que toutes les caractéristiques contribuent également à la modélisation.

### 2.2. Exploration des Données (EDA) et Ingénierie de Caractéristiques

*   **Analyse de Corrélation**: Une matrice de corrélation a été calculée et visualisée, révélant des relations clés avec l'indice de bonheur :
    *   **Qualité du Sommeil** (`Sleep_Quality(1-10)`): Forte corrélation positive (0.68).
    *   **Niveau de Stress** (`Stress_Level(1-10)`): Forte corrélation négative (-0.74).
    *   **Temps d'Écran Quotidien** (`Daily_Screen_Time(hrs)`): Forte corrélation négative (-0.71).
*   **Ingénierie de Caractéristiques**: Une nouvelle caractéristique, `Well_Being_Score`, a été créée en combinant les trois variables les plus corrélées (`Sleep_Quality(1-10)` - `Stress_Level(1-10)` - `Daily_Screen_Time(hrs)`). Cette variable composite visait à capturer un indice global de bien-être.

### 2.3. Stratégie de Modélisation

*   **Division des Données**: Le jeu de données a été divisé en ensembles d'entraînement (80%) et de test (20%) pour évaluer la généralisabilité des modèles.
*   **Validation Croisée**: Une stratégie de validation croisée `KFold` (5 splits, shuffle=True) a été utilisée pour l'évaluation robuste des modèles de régression et l'optimisation des hyperparamètres.
*   **Algorithmes Testés**:
    *   **Régression (cible continue)**: Régression Linéaire Simple, Régression Linéaire Multiple, Régression Polynomiale (degré 2), Régression Ridge, Régression Lasso, Arbre de Décision, Forêt Aléatoire (non optimisée et optimisée).
    *   **Classification (cible catégorielle)**: Régression Logistique Multiclasse (après discrétisation de la variable cible `Happiness_Index(1-10)` en 'faible' (0-6), 'moyen' (7-8) et 'élevé' (9-10)).

## 3. Résultats & Discussion

### 3.1. Modèles de Régression (Variable Cible Continue)

Le tableau ci-dessous récapitule les performances des différents modèles de régression sur l'ensemble de test :

| Modèle                            | R-carré | MSE  | MAE  |
| :-------------------------------- | :------ | :--- | :--- |
| Régression Linéaire Simple        | 0.53 | 1.12 | 0.88 |
| Régression Linéaire Multiple      | 0.61 | 0.93 | 0.80 |
| Régression Polynomiale (Degré 2)  | 0.50 | 1.18 | 0.92 |
| Régression Ridge                  | 0.61 | 0.93 | 0.80 |
| Régression Lasso                  | **0.63** | **0.89** | 0.79 |
| Arbre de Décision                 | 0.37 | 1.49 | 0.89 |
| Forêt Aléatoire (non optimisée)   | 0.60 | 0.94 | 0.77 |
| Forêt Aléatoire (optimisée)       | 0.62 | 0.91 | **0.76** |

*   **Analyse des Performances**:
    *   La **Régression Lasso** a montré la meilleure performance en termes de R-carré (0.63), mais la **Forêt Aléatoire (optimisée)** a obtenu un MAE légèrement inférieur (0.76 vs 0.79 pour Lasso), indiquant des prédictions en moyenne plus proches des valeurs réelles.
    *   Les modèles linéaires régularisés (Ridge, Lasso) ont surperformé la régression linéaire multiple non régularisée, montrant l'efficacité de la régularisation pour la généralisation.
    *   La régression polynomiale de degré 2 n'a pas apporté d'amélioration significative, suggérant que les relations complexes ne sont pas simplement quadratiques ou qu'un degré supérieur serait nécessaire avec un risque de surapprentissage accru.
    *   L'**Arbre de Décision** seul a été le moins performant, soulignant la puissance des méthodes d'ensemble comme la Forêt Aléatoire.
    *   L'optimisation des hyperparamètres de la Forêt Aléatoire a légèrement amélioré sa performance par rapport à la version non optimisée.

### 3.2. Modèle de Classification (Variable Cible Catégorielle)

La variable `Happiness_Index(1-10)` a été discrétisée en 'faible' (0-6), 'moyen' (7-8), et 'élevé' (9-10). Un modèle de Régression Logistique Multiclasse a été appliqué.

*   **Performances Globales**: Une précision globale (accuracy) de **73%** a été atteinte sur l'ensemble de test.
*   **Performance par Catégorie**:
    *   **Catégorie 'élevé'**: Très bien prédite (Précision: 0.80, Rappel: 0.92, F1-score: 0.85).
    *   **Catégorie 'moyen'**: Performances modérées (Précision: 0.65, Rappel: 0.59, F1-score: 0.62).
    *   **Catégorie 'faible'**: La plus difficile à prédire, avec un faible Rappel (0.33), souvent confondue avec la catégorie 'moyen' (8 des 12 cas 'faible' réels ont été classés comme 'moyen').
*   **Matrice de Confusion**: La matrice de confusion a clairement montré le déséquilibre dans la prédiction, avec la difficulté à identifier correctement la classe minoritaire 'faible'.

## 4. Conclusion

### Résumé des Facteurs Influencant l'Indice de Bonheur

L'analyse exhaustive, tant en régression qu'en classification, converge vers un ensemble de facteurs clés qui influencent de manière significative l'indice de bonheur :

*   **Niveau de Stress** (`Stress_Level(1-10)`): Sans surprise, c'est le facteur le plus fortement et négativement lié au bonheur. Un stress élevé est un puissant prédicteur d'un faible indice de bonheur.
*   **Qualité du Sommeil** (`Sleep_Quality(1-10)`): Cruciale pour le bien-être, une meilleure qualité de sommeil est fortement associée à un indice de bonheur plus élevé.
*   **Temps d'Écran Quotidien** (`Daily_Screen_Time(hrs)`): Un temps d'écran plus élevé est associé à une diminution de l'indice de bonheur, soulignant l'importance de la modération.
*   **Autres Facteurs**: L'âge, la fréquence de l'exercice et les préférences pour les plateformes de médias sociaux contribuent également, bien que leur impact soit moins direct et plus nuancé, souvent capturé par les modèles plus complexes comme la Forêt Aléatoire.

La supériorité des modèles non linéaires et d'ensemble (Forêt Aléatoire) en régression suggère que les relations entre ces facteurs et le bonheur ne sont pas toujours simples, impliquant des interactions et des seuils que les modèles linéaires peinent à capturer. Pour la classification, le modèle logistique est efficace pour les catégories 'élevé', mais la prédiction des catégories 'faible' reste un défi en raison du déséquilibre des classes.

### Limites des Modèles Développés

*   **Interprétabilité vs Performance**: Les modèles les plus performants (Forêt Aléatoire, Lasso) peuvent être moins directement interprétables que la régression linéaire simple, particulièrement la Forêt Aléatoire qui est une "boîte noire".
*   **Déséquilibre de Classe en Classification**: La difficulté du modèle de régression logistique à prédire la catégorie 'faible' bonheur est une limitation due au nombre restreint d'exemples pour cette classe.
*   **Généralisabilité**: Les modèles sont entraînés sur un jeu de données spécifique ; leur performance pourrait varier sur d'autres populations ou contextes.

### Propositions Concrètes de Pistes d'Amélioration Futures

*   **Rééquilibrage des Classes**: Pour la classification, implémenter des techniques telles que SMOTE ou l'ajustement des poids des classes pour améliorer la détection des catégories minoritaires.
*   **Optimisation Avancée des Hyperparamètres**: Explorer `RandomizedSearchCV` ou des optimisations bayésiennes pour une recherche plus efficace et potentiellement de meilleurs hyperparamètres, surtout pour les modèles comme la Forêt Aléatoire ou d'autres modèles d'ensemble.
*   **Modèles Avancés**: Tester des algorithmes plus sophistiqués tels que XGBoost, LightGBM, ou des réseaux de neurones pour capturer des relations encore plus complexes.
*   **Ingénierie de Caractéristiques**: Continuer à explorer la création de caractéristiques dérivées, en particulier des termes d'interaction, qui pourraient mieux représenter les influences combinées de plusieurs facteurs.
*   **Collecte de Données Supplémentaires**: Pour adresser le déséquilibre des classes et améliorer la robustesse des modèles, la collecte de données supplémentaires, en particulier pour les catégories sous-représentées, serait bénéfique.

Ce rapport fournit une base solide pour comprendre les dynamiques de l'indice de bonheur et ouvre la voie à des recherches et des développements futurs plus approfondis.

# Conduite-de-projet-C-te-d-Azur
Construction d'indices de prix immobiliers sur la Côte d'Azur (2010-2025).
# Construction d'Indices de Prix Immobiliers  : Région Côte d'Azur

## Université de Strasbourg - Master 1 Statistique et Économétrie
### CHKILI Dounia & GAYE Anna

---
#### LIEN DU HTML MARKDOWN : 

https://chkilidounia04-lab.github.io/Conduite-de-projet-C-te-d-Azur/

### LIEN DU RAPPORT PDF : 

Conduite_de_projet.pdf 

## Introduction :

Le marché immobilier est caractérisé par une forte hétérogénéité spatiale et structurelle. Le prix immobilier observé dépend à la fois de la composition des biens vendus (surface, typologie), de l'évolution du marché (tendances, cycles), et de l'hétérogénéité spatiale (commune, département). 

L'objectif de ce projet est de construire des indices de prix mensuels par commune pour les appartements et les maisons sur la région Côte d'Azur (départements 06 et 83).Pour éviter les biais liés aux changements de composition des biens vendus d'une année sur l'autre, nous avons implémenté une méthodologie d'indices hédoniques permettant de mesurer l'évolution des prix "à qualité constante".

## Les Données Utilisées

L'analyse repose sur la fusion de deux sources de transactions foncières :
* **Base DVF+ (Open-Data) :** Données récentes des transactions et caractéristiques des biens.
* **Base Historique (2010-2013) :** Données complémentaires permettant d'assurer une continuité temporelle sur une longue période.

## La Méthodologie 

Notre approche se divise en plusieurs étapes rigoureuses, traitant séparément les marchés des maisons et des appartements:

### 1. Le Modèle 
Nous modélisons le logarithme du prix au mètre carré par Moindres Carrés Ordinaires (OLS) afin de transformer les effets multiplicatifs en effets additifs et de stabiliser la variance. L'équation estimée est la suivante :

$$y_{i}=\alpha+x_{i}^{\prime}\beta+\gamma_{t(i)}+\delta_{d(i)}+\mu_{c(i)}+\theta_{d(i),a(i)}+\epsilon_{i}$$ 

Où $\gamma_{t(i)}$ capture la tendance du marché, $\mu_{c(i)}$ le niveau local moyen, $\delta_{d(i)}$ le contrôle spatial départemental, et $\theta_{d(i),a(i)}$ les tendances spécifiques au département par année .

Afin d'éviter les risques de sur-ajustement, l'effet fixe commune ($\mu_{c}$) n'est pas inclus pour les petites communes présentant un effectif de transactions trop faible.

### 2. Le Prix Net à Qualité Constante
Pour isoler l'évolution pure du marché, nous retirons la contribution des caractéristiques physiques du bien (ex: surface):

$$\log P_{i}^{net}=\hat{y}_{i}-\sum_{k\in\mathcal{H}}\hat{\beta}_{k}x_{ik}$$

Une hausse de ce $\log P_{i}^{net}$ traduis une véritable hausse du marché à bien comparable, et non un simple changement du type de biens vendus.

### 3. Agrégation des prix, correction des petites communes et valeurs manquantes 
* Les prix nets sont agrégés au niveau communal et mensuel en utilisant la médiane (plus robuste aux valeurs extrêmes).Les communes à faible effectif sont réajustées en utilisant la moyenne de leurs résidus : $\log P_{i}^{net} \leftarrow \log P_{i}^{net} + \bar{e}_{c(i)}$.
* En ce qui concerne les valeurs manquantes, les périodes sans transaction ont été complétées en prédisant les valeurs à partir des coefficients du modèle estimé, assurant une série temporelle continue.

### 4. Indice Base 100
L'indice final est calculé par rapport à une période de référence $t_0$ (janvier 2010):

$$Index_{c,t}=100\times \exp(\log P_{c,t}^{net}-\log P_{c,t_{0}}^{net})$$ 

## Visualisations et Résultats

Le répertoire génère les résultats visuels attendus pour valider notre modèle. De prime abbord, la courbe temporelle permet de voire l'évolution de $Index_{c,t}$ pour des communes cibles comparées au benchmark départemental. Ensuite, la carte choroplèthe interactive permet une représentation spatiale du niveau de l'indice sur la Côte d'Azur à une date donnée.Enfin, la Heatmap (Commune $\times$ Temps) permet la mise en évidence des ruptures temporelles, des dynamiques de marché et de l'hétérogénéité spatiale sur les 30 communes les plus actives.

## Exécution du Code

Le code principal a été développé sur **Google Colab** en Python aini que sur R. 
* Fichier principal : `code_python.ipynb`
* Bibliothèques requises : `pandas`, `numpy`, `statsmodels`, `matplotlib`, `seaborn`, `geopandas`, `plotly`.

---
*Projet réalisé dans le cadre du Master 1 APE / Statistique et Économétrie (2025/2026).*

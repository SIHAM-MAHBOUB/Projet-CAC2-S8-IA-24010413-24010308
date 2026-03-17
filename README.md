# 🔍 Algorithmes de Clustering — Étude Comparative
### Projet S8 · Module IA & Apprentissage Automatique · ENCG Settat

> **Réalisé par :** Liousfi Marwa & Mahboub Siham — CAC L3 S8  
> **Encadrant :** Larhimli Abderahman  
> **Période :** Mars 2026  
> **Datasets :** Iris (150×4) · Wine (178×13) · Digits (1797×64) · Moons/Circles (synthétiques)

---

## 📋 Description

Ce projet implémente et compare **11 algorithmes de clustering** (classification non supervisée) appliqués à des jeux de données réels issus de `scikit-learn`. L'objectif est de déterminer, pour chaque situation, quelle méthode offre les meilleures performances selon des critères objectifs (score de silhouette, inertie, BIC/AIC).

Le projet est accompagné d'un **dashboard interactif** (`dashboard_clustering.html`) permettant de visualiser les résultats directement dans le navigateur.

---

## 📁 Structure du projet

```
clustering-project/
│
├── clustering_examples.py       # Script principal — tous les algorithmes
├── clustering_algorithms.ipynb  # Notebook source (Jupyter)
├── dashboard_clustering.html    # Dashboard interactif (visualisation)
│
├── rapports/
│   └── CR_Projet_Clustering_S8_2026.pdf   # Compte rendu académique complet
│
└── README.md                    # Ce fichier
```

---

## 🧠 Algorithmes implémentés

| # | Algorithme | Famille | k requis | Détection bruit |
|---|-----------|---------|----------|-----------------|
| 1 | **K-Means** | Partitionnel | ✅ Oui | ❌ Non |
| 2 | **K-Medoids (PAM)** | Partitionnel | ✅ Oui | ❌ Non |
| 3 | **K-Medians** | Partitionnel | ✅ Oui | ❌ Non |
| 4 | **DBSCAN** | Densité | ❌ Non | ✅ Oui |
| 5 | **HDBSCAN** | Densité hiérarchique | ❌ Non | ✅ Oui |
| 6 | **OPTICS** | Densité | ❌ Non | ✅ Oui |
| 7 | **HAC (Ward)** | Hiérarchique | ✅ Oui | ❌ Non |
| 8 | **GMM** | Probabiliste | ✅ Oui | ❌ Non |
| 9 | **Spectral Clustering** | Spectral | ✅ Oui | ❌ Non |
| 10 | **Affinity Propagation** | Propagation de messages | ❌ Auto | ❌ Non |
| 11 | **SOM** | Réseau de neurones | ❌ Auto | ❌ Non |
| + | **UMAP + K-Means** | Réduction + Partitionnel | ✅ Oui | ❌ Non |

---

## 📊 Résultats clés (Dataset Iris normalisé)

| Algorithme | Silhouette | Clusters détectés | Remarque |
|-----------|-----------|-------------------|----------|
| HAC Ward | **~0.48** | 3 | Meilleur score sur Iris |
| DBSCAN | **~0.50** | 2–3 | Détecte automatiquement le bruit |
| K-Means | ~0.46 | 3 | Référence de base |
| K-Medoids | ~0.46 | 3 | Robuste aux outliers |
| Spectral | ~0.45 | 3 | Efficace formes non-convexes |
| GMM (full) | ~0.28 | 3 | Clustering probabiliste (mou) |
| Affinity Prop. | ~0.45 | **7–9** ⚠️ | Sur-segmentation connue |

> **UMAP + K-Means sur Digits :** silhouette passe de ~0.15 (espace 64D) à **~0.65** (espace UMAP 2D).

---

## 🗺️ Guide de sélection d'algorithme

```
               FORME DES CLUSTERS
               Non-convexe
                    │
       OPTICS   HDBSCAN  Spectral
            \      │      /
             \     │     /
 K inconnu ─────────────────── K connu
             /     │     \
            /      │      \
        AP      DBSCAN   K-Means / HAC
                    │
                Convexe
```

| Situation | Algorithme recommandé |
|-----------|----------------------|
| k connu, clusters sphériques | **K-Means** |
| k connu, robustesse aux outliers | **K-Medoids** |
| k connu, distribution gaussienne | **GMM** |
| k connu, forme arbitraire | **Spectral Clustering** |
| k inconnu, détection de bruit | **DBSCAN / HDBSCAN** |
| k inconnu, densités variables | **OPTICS / HDBSCAN** |
| k inconnu, sans contrainte | **Affinity Propagation** |
| Haute dimension + visualisation | **UMAP + K-Means** |
| Données catégorielles / mixtes | **K-Medoids** |

---

## ⚙️ Installation

### Prérequis

- Python 3.8+
- pip

### Dépendances

```bash
# Bibliothèques de base
pip install scikit-learn scipy numpy matplotlib

# Bibliothèques optionnelles (algorithmes avancés)
pip install scikit-learn-extra   # K-Medoids
pip install hdbscan              # HDBSCAN
pip install minisom              # Self-Organizing Maps (SOM)
pip install umap-learn           # UMAP
```

---

## ▶️ Exécution

```bash
python clustering_examples.py
```

Les graphiques apparaissent séquentiellement dans des fenêtres séparées.  
**Fermer chaque fenêtre** pour passer à l'algorithme suivant.

Pour la visualisation interactive, ouvrir directement dans le navigateur :

```bash
open dashboard_clustering.html   # macOS
xdg-open dashboard_clustering.html  # Linux
# ou double-cliquer sur le fichier sous Windows
```

---

## 📐 Métriques d'évaluation utilisées

| Métrique | Description | Objectif |
|----------|-------------|----------|
| **Silhouette Score** | Cohésion intra-cluster vs séparation inter-cluster | Maximiser → proche de 1 |
| **WCSS / Inertie** | Somme des distances au centroïde (K-Means) | Minimiser |
| **BIC / AIC** | Critères de sélection du nombre de composantes (GMM) | Minimiser |

---

## 📦 Datasets utilisés

| Dataset | Observations | Features | Classes réelles | Algorithmes |
|---------|-------------|----------|-----------------|-------------|
| **Iris** | 150 | 4 | 3 | K-Means, K-Medoids, K-Medians, DBSCAN, OPTICS, HAC, Spectral, AP |
| **Wine** | 178 | 13 | 3 | GMM, SOM |
| **Digits** | 1 797 | 64 | 10 | UMAP + K-Means |
| **Moons** | 300 | 2 | 2 | DBSCAN, Spectral |
| **Circles** | 300 | 2 | 2 | DBSCAN, Spectral |

Tous les datasets sont issus de `sklearn.datasets` — aucun téléchargement nécessaire.

---

## 👥 Auteurs

| Nom | Numéro étudiant | Filière |
|-----|-----------------|---------|
| **Liousfi Marwa** | 24010308 | CAC — L3 S8 |
| **Mahboub Siham** | 24010413 | CAC — L3 S8 |

**Encadrant :** Larhimli Abderahman  
**École :** ENCG Settat — Université Hassan 1er  
**Module :** Intelligence Artificielle & Apprentissage Automatique  
**Année universitaire :** 2025–2026

---

## 📚 Références

- Ester et al. (1996). *A density-based algorithm for discovering clusters.* KDD.
- Pedregosa et al. (2011). *Scikit-learn: Machine Learning in Python.* JMLR, 12.
- McInnes et al. (2018). *UMAP: Uniform Manifold Approximation and Projection.* arXiv:1802.03426.
- Kohonen, T. (2001). *Self-Organizing Maps* (3e éd.). Springer.
- [Documentation scikit-learn — Clustering](https://scikit-learn.org/stable/modules/clustering.html)
- [Documentation UMAP](https://umap-learn.readthedocs.io/)

---

*Projet réalisé dans le cadre du Semestre 8 — ENCG Settat · Mars 2026*

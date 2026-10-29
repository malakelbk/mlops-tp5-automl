# Modele manuel ou modele AutoML ? (TP05)

| Critere                    | Manuel (rf 500/16)          | AutoML (FLAML 90 s)                      |
|----------------------------|-----------------------------|------------------------------------------|
| Famille de modele          | foret aleatoire             | xgboost(XGBClassifier)                  |
| F1 sur le jeu de test      | 0.8669                      | 0.8561                                   |
| Temps de recherche machine | 1.9 s (un seul entrainement)| 90.2  s (duree_recherche_s)                 |
| Temps humain (estimation)  | ~1 a 2 h (reglage au TP2)   | ~10 min (ecriture du script)             |
| Latence mediane (ms)       | 77.9 / 79.9                 | 27.7 / 23.6                              |
| Latence p95 (ms)           | 116.3 / 119.0               | 38.0 / 30.4                              |
| Taille du fichier (Mo)     | 13.37                       | 0.28                                     |
| Explicabilite              | importances des variables, 500 arbres, profondeur 16 | importances des variables (gain), 322 arbres, reglages cfg_* journalises |

## Decision (5 lignes) : quel modele mettre en production, et pourquoi ?

Je mets en production le modele AutoML (XGBoost, fraude-automl).
Il perd 0.011 point de F1 (0.8561 contre 0.8669), un ecart faible au regard de ses autres atouts.
Il repond environ 3 fois plus vite (mediane ~25 ms contre ~80 ms) avec un p95 plus stable.
Il est 48 fois plus leger (0.28 Mo contre 13.4 Mo), ce qui compte pour l'image Docker et le mobile.
Son explicabilite reste comparable (importances des variables), et grace a l'alias production on peut revenir au modele manuel sans toucher au client.
# Traitement d'images SAR (Synthetic Aperture Radar) pour le suivi de l'évolution de décharges sauvages
## Présentation générale: 
* Aujourd’hui, la surconsommation de produits à bas coût entraîne une production excédentaire, sans prévoir la gestion du surplus. Cela conduit à la formation de décharges illicites en pleine nature, de telles dimensions qu’elles sont visibles en imagerie satellitaire (avec de résolutions d’environ dix mètres). Ce projet consistera à étudier l’évolution de différentes décharges clandestines, de voitures en Californie et de vélos défectueux en Chine. Il est nouveau et techniquement compliqué mais aucun enjeu particulier n’est mis par l’entreprise sur sa réalisation.

## Méthodologie de débruitage :
* Pour le débruitage des images : RABASAR (ratio-based multitemporal SAR images). C’est un algorithme permettant le “despeckle” des images satellites, c'est-à-dire enlever le bruit granulaire de type speckle. Pour cela on utilise la base de données prétraitées par SNAP. D’abord, on commence par définir l’image de référence appelée “super-image”. Puis on divise une image par la “super-image” pixel par pixel. Ensuite, on fait le filtrage spatial de la “ratio-image”. Enfin, on remonte à l’image d’origine en multipliant l’image résultante par l’image de référence.

* Informations sur la configuration de données utilisées 
  "sensor": "sentienel-1",
  "site": "victorville",
  "regularizer": "bm3d",
  "spatial_weight": 0.1,
  "temporal_average_spatial_weight": 0.03,
  "ratio_weight": 0.1


## Méthologie de suivi de surface de décharge:
Dans le contexte où la zone de décharge présente une luminosité notablement plus élevée que son environnement, nous avons mis en place une transformation d'image binaire. Cette méthode consiste à seuiller les pixels afin de mettre en évidence la différence de luminosité. Cette transformation simplifie considérablement la tâche d'analyse et de suivi de la superficie de la décharge.


* sources:
* [https://github.com/simard-landscape-lab/rabasar]
  [https://arxiv.org/pdf/2307.07892.pdf]

# Traitement d'images SAR (Synthetic Aperture Radar) pour le suivi de l'évolution de décharges sauvages
## Présentation générale: 
* Aujourd’hui, la surconsommation de produits à bas coût entraîne une production excédentaire, sans prévoir la gestion du surplus. Cela conduit à la formation de décharges illicites en pleine nature, de telles dimensions qu’elles sont visibles en imagerie satellitaire (avec de résolutions d’environ dix mètres). Ce projet consistera à étudier l’évolution de différentes décharges clandestines, de voitures en Californie et de vélos défectueux en Chine. Il est nouveau et techniquement compliqué mais aucun enjeu particulier n’est mis par l’entreprise sur sa réalisation.
![Alt text](https://i0.wp.com/www.vvng.com/wp-content/uploads/2018/03/desert-graveyard.png?fit=1740%2C1134&ssl=1)
## 

Les fichiers TIFF utilisés dans les notebooks ont été obtenus après un pré-traitement sur le logiciel `SNAPSeNtinel Application Platform` qui est un logiciel développé par l'Agence spatiale européenne, conçu pour le traitement et l'analyse des données satellites préliminaire visant à aligner toutes les images sur la première.

* Informations sur la configuration de données utilisées 
*sensor: "sentienel-1", Images "SLC": Single Look Complex
* Mode d'aquisition des images: "IW"(Interferometric Wide)
*sites: "victorville", "AltoHospicio","Hangzhou"

## Méthodologie de débruitage :
 Les images sont ensuite co-enregistrées afin de créer une base de données. Ensuite, ces images sont redécoupées et à l’aide de l’algorithme RABASAR[6] et une image ratio montrant les changements entre deux images est obtenue. Après traitement et filtrage et conversion en images binaires, il est aisé d’obtenir le nombre de pixels des changements de la décharge et ainsi de tracer un graphe montrant les évolutions de la superficie de décharge en fonction du temps.
 
![Architecture de la démarche suivie](C:\Users\Drira Yosr\Downloads\architecture.png)



## Méthologie de suivi de surface de décharge:
Dans le contexte où la zone de décharge présente une luminosité notablement plus élevée que son environnement, nous avons mis en place une transformation d'image binaire. Cette méthode consiste à seuiller les pixels afin de mettre en évidence la différence de luminosité. Cette transformation simplifie considérablement la tâche d'analyse et de suivi de la superficie de la décharge.
* Une fois les images binaires obtenues, la bibliothèque Scikit-learn[11] et ses fonctions morphologiques ont été utilisées afin de traiter les images. Cette bibliothèque est adaptée pour opérer sur les formes dans les images afin d’améliorer la représentation visuelle des zones d'intérêt. Les opérations morphologiques de base comprennent l’érosion et la dilatation. 
* Erosion: 
Les objets de petite taille environnant la zone de décharge sont éliminés en utilisant la fonctionnalité "morphology.remove_small_objects". Cette opération de traitement d'image vise à purifier la représentation visuelle en éliminant les éléments indésirables de dimensions réduites, concentrant ainsi l'analyse sur la zone principale d'intérêt, à savoir la décharge.             
* Dilatation:
Pour remplir les lacunes dans la zone d'intérêt, fusionner les parties disjointes et élargir les contours la fonctionnalité  "morphology.binary_dilation" est utilisée.

* sources:
* [https://github.com/simard-landscape-lab/rabasar]
  [https://arxiv.org/pdf/2307.07892.pdf]

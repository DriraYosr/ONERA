# Traitement d'images SAR (Synthetic Aperture Radar) pour le suivi de l'évolution de décharges sauvages

## Présentation générale
Aujourd’hui, la surconsommation de produits à bas coût entraîne une production excédentaire, sans prévoir la gestion du surplus. Cela conduit à la formation de décharges illicites en pleine nature, de telles dimensions qu’elles sont visibles en imagerie satellitaire (avec des résolutions d’environ dix mètres). Ce projet consiste à étudier l’évolution de différentes décharges clandestines, de voitures en Californie et de vélos défectueux en Chine. Il est nouveau et techniquement compliqué mais aucun enjeu particulier n’est mis par l’entreprise sur sa réalisation.
![Décharge sauvage](https://i0.wp.com/www.vvng.com/wp-content/uploads/2018/03/desert-graveyard.png?fit=1740%2C1134&ssl=1)

## Fichiers TIFF et Pré-traitement
Les fichiers TIFF utilisés dans les notebooks ont été obtenus après un pré-traitement sur le logiciel `SNAPSeNtinel Application Platform`, développé par l'Agence spatiale européenne. Ce logiciel est conçu pour le traitement et l'analyse des données satellites préliminaires visant à aligner toutes les images sur la première.

### Configuration des données utilisées
- Capteur: "Sentinel-1"
- Type d'images: "SLC" (Single Look Complex)
- Mode d'acquisition des images: "IW" (Interferometric Wide)
- Sites: "Victorville", "Alto Hospicio", "Hangzhou"

## Méthodologie de suivi de la décharge
Les images sont co-enregistrées pour créer une base de données. Ensuite, ces images sont redécoupées, et à l’aide de l’algorithme RABASAR, une image ratio montrant les changements entre deux images est obtenue. Après traitement, filtrage et conversion en images binaires, il est aisé d’obtenir le nombre de pixels des changements de la décharge et ainsi de tracer un graphe montrant les évolutions de la superficie de décharge en fonction du temps.
![Architecture du projet](https://github.com/DriraYosr/ONERA/assets/123462890/186f2120-4871-42d9-ad33-5cd7cf5635a6)

## Pourquoi la conversion en binaire ?
Dans le contexte où la zone de décharge présente une luminosité notablement plus élevée que son environnement, nous avons mis en place une transformation d'image binaire. Cette méthode consiste à seuiller les pixels afin de mettre en évidence la différence de luminosité. Cette transformation simplifie considérablement la tâche d'analyse et de suivi de la superficie de la décharge.

## Pourquoi les opérations morphologiques ?
Une fois les images binaires obtenues, la bibliothèque Scikit-learn et ses fonctions morphologiques ont été utilisées afin de traiter les images. Cette bibliothèque est adaptée pour opérer sur les formes dans les images afin d’améliorer la représentation visuelle des zones d'intérêt. Les opérations morphologiques de base comprennent l’érosion et la dilatation.

- **Erosion:**
  Les objets de petite taille environnant la zone de décharge sont éliminés en utilisant la fonctionnalité "morphology.remove_small_objects". Cette opération de traitement d'image vise à purifier la représentation visuelle en éliminant les éléments indésirables de dimensions réduites, concentrant ainsi l'analyse sur la zone principale d'intérêt, à savoir la décharge.

- **Dilatation:**
  Pour remplir les lacunes dans la zone d'intérêt, fusionner les parties disjointes et élargir les contours, la fonctionnalité "morphology.binary_dilation" est utilisée.


Le diagramme ci-dessous détaille la séquence d'application des notebooks pour le suivi des changements, conformément à la description précédente. Suivez ces étapes dans l'ordre indiqué:

![Etapes (2)](https://github.com/DriraYosr/ONERA/assets/123462890/11cd3d08-e62a-4bd0-8bae-4a491055ade4)


### Sources
- [RABASAR GitHub Repository](https://github.com/simard-landscape-lab/rabasar)
- [Article scientifique sur RABASAR](https://arxiv.org/pdf/2307.07892.pdf)
- [Documentation Scikit-image sur les opérations morphologiques](https://scikit-image.org/docs/stable/api/skimage.morphology.html)

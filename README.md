# Traitement d'images SAR (Synthetic Aperture Radar) pour le suivi de l'évolution de décharges sauvages
## Présentation générale: 
* Aujourd’hui, la surconsommation de produits à bas coût entraîne une production excédentaire, sans prévoir la gestion du surplus. Cela conduit à la formation de décharges illicites en pleine nature, de telles dimensions qu’elles sont visibles en imagerie satellitaire (avec de résolutions d’environ dix mètres). Ce projet consistera à étudier l’évolution de différentes décharges clandestines, de voitures en Californie et de vélos défectueux en Chine. Il est nouveau et techniquement compliqué mais aucun enjeu particulier n’est mis par l’entreprise sur sa réalisation.

## 
Les fichiers TIFF utilisés dans les notebooks ont été obtenus après un pré-traitement sur le logiciel `SNAPSeNtinel Application Platform` qui est un logiciel développé par l'Agence spatiale européenne, conçu pour le traitement et l'analyse des données satellites préliminaire visant à aligner toutes les images sur la première.

## Méthodologie de débruitage :

* Pour le débruitage des images : RABASAR (ratio-based multitemporal SAR images). C’est un algorithme permettant le “despeckle” des images satellites, c'est-à-dire enlever le bruit granulaire de type speckle. Pour cela on utilise la base de données prétraitées par SNAP. D’abord, on commence par définir l’image de référence appelée “super-image”. Puis on divise une image par la “super-image” pixel par pixel. Ensuite, on fait le filtrage spatial de la “ratio-image”. Enfin, on remonte à l’image d’origine en multipliant l’image résultante par l’image de référence.
Dans une seconde partie, on fait le filtrage spatial de la “ratio-image”. Enfin, on remonte à l’image d’origine en multipliant l’image résultante par l’image de référence. (cette partie ,n'est pas incluse dans le github)
* Informations sur la configuration de données utilisées 
*sensor: "sentienel-1", Images "SLC": Single Look Complex
* Mode d'aquisition des images: "IW"(Interferometric Wide)
*sites: "victorville", "AltoHospicio","Hangzhou"


CODE DE FORMATION DES RATIOS:
```python
def ratio(img, reference_img):
    # Ajoutez une petite valeur à reference_img pour éviter la division par zéro
    for i in range(np.shape(reference_img)[0]):
        for j in range(np.shape(reference_img)[1]):
            if reference_img[i, j] == 0. + 0.j:
                reference_img[i, j] = 1e-3 + 0.j
    return np.clip(img / reference_image, 1e-3, 10)
Ratios = []
for k in range(1, len(Images_rognees)):
    Ratios.append(np.real(ratio(Images_rognees[k], reference_image)))

```

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

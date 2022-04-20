# Objectif
Convertir les orthophotographies métropolitaines pour qu'elles soient conformes aux arrêtés définissant les Zones Interdites à la Prise de Vue Aérienne (ZIPVA).

# Présentation générale
Deux arrêtés gouvernementaux de 2018 et 2021 restreignent les prises de vues en fonction d'emprises définies comme étant sensibles.
Le territoire métropolitain (95 communes) en compte 5 en 2021.
### Outils
GDAL
### Données
Des orthophotographies sous forme de tuiles ou assemblées

# Méthodologie
#### Choix methodologiques
La méthode retenue pour appliquer ces arrêtés, tout en permettant une consultation sans impact à petite échelle, consiste à dégrader la résolution des orthophotographies sur les zones définies par le gouvernement. Les ZIPVA sont donc sous-échantillonnées à 5m/pix.

#### Vecteurs
Les données brutes sont accessibles sur le site data.gouv.fr (https://www.data.gouv.fr/fr/datasets/zones-interdites-a-la-prise-de-vue-aerienne-1/). Elles ont été téléchargées et enregistrées dans la base \\\batzella.lmcu.fr\vuesaeriennes\orthos\documentation\soustraction_zipva\zipva_multi_millesimes.gpkg.
Celle-ci se compose de trois tables,
-	arrete_12_10_2018 : les emprises définies par l'arrêté de 2018,
-	arrete_10_06_2021 : les emprises définies par l'arrêté de 2021 et,
-	emprises_mel : les 5 emprises métropolitaines issues de l'arrêté de 2021 (polygones et attributs sont conservés).
Les différentes table contiennent une colonne DATE_EFFET. Les dates qui y figurent indiquent la date à partir de laquelle une emprises devient une ZIPVA. Pour des raisons de facilité de traitements et puisque certains aménagements sont déjà visibles antérieurement aux dates d'effets, il a été choisi de ne pas considérer ces valeurs et de les appliquer pour les ortho 2016 et postérieures.

#### Chaine opératoire
L’outils se présente sous la forme d’un fichier bat à exécuter en ligne de commande. Le répertoire de travail se composera :
-	d’un répertoire tuiles dans lequel vous déposerait les tuiles s’il y a lieu.
-	d’un répertoire temp qui sera généré automatiquement et,
-	des production au format tif avec un profil cog.
Si votre orthophotographie n’est pas sous le forme de tuiles, vous pouvez désactiver les stages 1 et 2 (REM en début de ligne) et remplacer path\prod\base.vrt par votre tif.

Pour être exécutable, il convient de modifier le fichier gdal_zipva.bat comme il suit :
-	Replacer toutes les occurrences de path par le nom du répertoire de travail.
-	Définir le système de projection des données entrantes (l. 9).
-	Si vous travaillez depuis une autre base vecteurs, modifier le chemin d’accès (l. 14).
-	Pour finir, définir la résolution la plus fine désirée pour le produit (-tr 0.05 par ex.) et son système de projection (l. 19).
# cloud-fruits-P8 Déployez un modèle dans le cloud
Vous êtes Data Scientist dans une très jeune start-up de l'AgriTech, nommée  "Fruits!", qui cherche à proposer des solutions innovantes pour la récolte des fruits.

## Exemples Github :
* https://github.com/nsaintgeours/sparkyfruit
* https://github.com/AdamVincent90/SimpleCNN
* https://github.com/Horea94/Fruit-Images-Dataset

## Documentations :
* Installation Ubuntu VirtualBox https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox
* Spark et Jupyter Notebook https://python.plainenglish.io/apache-spark-using-jupyter-in-linux-installation-and-setup-b2cacc6c7701
* Spark et Jupyter Notebookhttps://www.codeitbro.com/install-pyspark-on-ubuntu-with-jupyter-notebook/
* Spark et Jupyter Notebookhttps://www.bmc.com/blogs/jupyter-notebooks-apache-spark/
* Spark et Jupyter Notebookhttps://github.com/ruslanmv/Tutorial-of-Pyspark-with-Jupyter-Notebook
* Exemples Spark https://github.com/spark-examples/pyspark-examples
* AWS S3 access keys https://medium.com/@shamnad.p.s/how-to-create-an-s3-bucket-and-aws-access-key-id-and-secret-access-key-for-accessing-it-5653b6e54337
* Spark user defined class broadcast https://stackoverflow.com/questions/43042241/broadcast-a-user-defined-class-in-spark
* Torchvision & Transfer Learning https://getpocket.com/fr/read/2721181304

## I. Fonctionnement en local (sur mon PC)
### Installation d'Ubuntu / VirtualBox
https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview

#### Aperçu
* Télécharger une image Ubuntu Image (https://ubuntu.com/download/desktop/thank-you?version=22.04&architecture=amd64)
* Ubuntu 22.04 LTS, The Jammy Jellyfish (la méduse chanceuse), sorti le 21 avril 2022, soutenu jusqu'en Avril 2027
* Téléchargez et installez VirtualBox (https://www.virtualbox.org/wiki/Downloads)
* Une fois l'installation terminée, exécutez VirtualBox.

#### Créer une nouvelle machine virtuelle
* Type: Linux, Version: Ubuntu (64-bit), 8Gb RAM, 100 Go vdi disk

#### Installer votre image
https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#3-install-your-image

Cliquez sur Démarrer pour lancer la machine virtuelle. Vous serez invité à sélectionner le disque de démarrage.
Utilisez l'icône de fichier pour ouvrir le sélecteur de disque optique et cliquez sur Ajouter pour trouver votre fichier.iso
Choisissez l'image disque que vous souhaitez utiliser, puis cliquez sur Démarrer dans la fenêtre du disque de démarrage.
Le bureau Ubuntu devrait maintenant démarrer et afficher le menu d'installation.
Après ce point, vous pouvez suivre le flux d'installation normal pour Ubuntu Desktop. (https://ubuntu.com/tutorials/install-ubuntu-desktop#11-installation-complete)

Après le rédemarrage, le système d'opération Ubuntu est installé !

#### Modification de la résolution de la fenêtre
Remplacez le paramètre Contrôleur graphique par VBoxSVGA et cliquez sur OK (ignorez l'avertissement).

#### Installation des ajouts d'invités
Insérer le CD depuis le menu de VirtualBox et procéder à l'installation.
Revenez au menu Paramètres et redéfinissez le contrôleur graphique sur VMSVGA et Activez l'accélération 3D.
Une autre fonctionnalité que cela déverrouille est le presse-papiers partagé, que vous pouvez activer dans Périphériques > Presse-papiers partagé. Cela vous permettra de copier et coller entre vos machines virtuelles et hôtes, utile lorsque vous souhaitez copier des sorties d'un périphérique à l'autre.

### Installation des dépendances (Pip, Python3, Jupyter Notebook, Spark, Librairies annexes)

#### Installation de pip pour Python 3 (https://linuxize.com/post/how-to-install-pip-on-ubuntu-20.04/)
* Pour installer pip pour Python 3 sur Ubuntu 20.04, exécutez les commandes suivantes en tant qu'utilisateur root ou sudo dans votre terminal:
```
sudo apt update
sudo apt install python3-pip
```
* Installation de Jupyter Notebook (https://python.plainenglish.io/apache-spark-using-jupyter-in-linux-installation-and-setup-b2cacc6c7701)
```
sudo apt install python3-notebook jupyter jupyter-core python-ipykernel
```
* Installation des librairies python nécessaires au projet sur la machine virtuelle : voir fichier requirements.txt
```
pip3 install boto3 pillow torch torchvision tqdm
```
* Installation des librairie Java / Scala nécessaires pour accéder au stockage AWS S3 via Spark / Hadoop :
```
sudo apt install openjdk-11-jdk
sudo apt-get install scala
```
* Nécessite l'installation de 'Py4J', il permet aux programmes Python s'exécutant dans un interpréteur Python d'accéder dynamiquement aux objets Java dans une machine virtuelle Java :
```
sudo apt install openjdk-11-jdk
sudo apt-get install scala
pip3 install py4j
```

#### Installation de Spark (https://phoenixnap.com/kb/install-spark-on-ubuntu)
* Extraire le fichier téléchargé Spark qui se trouve dans le dossier '/home'
```
sudo tar -zxvf spark-3.2.1-bin-hadoop3.2.tgz
sudo mv spark-3.2.1-bin-hadoop3.2 /opt/spark
```
* Utilisez la commande echo pour ajouter ces lignes à .bashrc :
```
echo "export SPARK_HOME=/opt/spark" >> ~/.bashrc
echo "export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin" >> ~/.bashrc
echo "export PYTHONPATH=/usr/bin/python3" >> ~/.bashrc
```
* Il faut autoriser l'accès à l'environnement spark :
```
sudo chmod 777 spark
```
* La configuration SPARK est prête. Il est possible de vérifier l'environnement spark via le terminal :
```
cd spark/python/
python3
```
![import_pyspark](https://github.com/GreyFrenchKnight/cloud-fruits-P8/blob/c0fb6c4d13afda42b969b155ba663eb755863a5b/images/import%20pyspark.png)

* Configurer l'environnement de drivers pour Jupyter Notebook pour fonctionner dans l'environnement SPARK pour utiliser le package pyspark :
```
echo "export PYSPARK_DRIVER_PYTHON='jupyter'" >> ~/.bashrc
echo "export PYSPARK_DRIVER_PYTHON_OPTS='notebook --no-browser --port=8889'" >> ~/.bashrc
```
* Il faut également vérifier l'environnement spark via l'import dans Jupyter Notebook. Dans un terminal : 
```
> pyspark
```
* La ligne de commande pyspark lance Jupyter Notebook.
* Dans un Notebook, lancer l'import de spark et constater que la ligne de commande fonctionne.

L'environnement est complètement installé. Jupyter Nootebook fonctionne avec PySpark.

### Copie des fichiers sur la VM (https://unix.stackexchange.com/questions/16199/how-to-transfer-files-from-windows-to-ubuntu-on-virtualbox)
* Copie des dossiers de Training/Test dans le dossier partagé VirtualBox

### Développement du code dans un notebook PySpark 
* Création d'une SparkSession, lecture des images du dossier cloud-fruits-dataset et application de l'encodage avant de les traiter avec le modèle CNN Transfer Learning sans la dernière couche.
* Un fichier output.csv ou output.parquet est généré, il contient les features de chaque image, prêts à être envoyé dans une couche de classification pour prédire le type de fruit.


## II. Création d'un espace de stockage S3 sur AWS
J'ai copié le jeu de données Fruits 360 Dataset sur un espace de stockage S3 lié à mon compte AWS :

* création d'un bucket via la console AWS en ligne : cloud-fruits-p8-bucket
![bucket_folders](https://github.com/GreyFrenchKnight/cloud-fruits-P8/blob/c0fb6c4d13afda42b969b155ba663eb755863a5b/images/import%20pyspark.png)
* enregistrement sur mon PC d'un fichier contenant les clés d'accès à mon stockage S3 : voir fichier ~/.aws/credentials

### Copie des fichiers sur S3 (SDK boto3 ou API S3a)
* Copie des dossiers de Training dans le bucket 

### Configuration de Spark en local pour accéder à S3
Il faut éditer la configuration de Spark pour permettre l'accès aux données stockées sur AWS S3 directement via Spark / Hadoop
Pour ce faire, on se rend dans le dossier /opt/spark/conf sur la machine virtuelle Ubuntu.

* copier le fichier spark-defaults.conf.template vers un nouveau fichier spark-defaults.conf
* éditer le fichier spark-defaults.conf en y ajoutant les lignes suivantes (les clés AWS S3 sont disponibles sur ma console AWS en ligne) :
```
spark.jars.packages             com.amazonaws:aws-java-sdk-bundle:1.11.375,org.apache.hadoop:hadoop-aws:3.2.0
spark.hadoop.fs.s3a.access.key  XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
spark.hadoop.fs.s3a.secret.key  XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
spark.hadoop.fs.s3a.endpoint    s3.eu-west-3.amazonaws.com
spark.hadoop.fs.s3a.impl        org.apache.hadoop.fs.s3a.S3AFileSystem
```

* Pour une utilisation avec le SDK boto3, je créé un fichier contenant les clés d'accès à mon stockage S3 : voir fichier ~/.aws/credentials :
```
[default]
aws_access_key_id=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
aws_secret_access_key=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

### Mise à jour du code dans un notebook PySpark pour lire les fichiers sur un bucket S3 (SDK boto3 ou API S3a)
* Création d'une SparkSession, lecture des images du bucket cloud-fruits-p8-bucket et application de l'encodage avant de les traiter avec le modèle CNN Transfer Learning sans la dernière couche.
* Un fichier output.csv ou output.parquet est généré, il contient les features de chaque image, prêts à être envoyé dans une couche de classification pour prédire le type de fruit.

## III. Création d'une image EC2 sur AWS

### Installation des dépendances (Pip, Python3, Jupyter Notebook, Spark, Librairies annexes)
blabla

# cloud-fruits-P8 Déployez un modèle dans le cloud
Vous êtes Data Scientist dans une très jeune start-up de l'AgriTech, nommée  "Fruits!", qui cherche à proposer des solutions innovantes pour la récolte des fruits.

* Exemples
** https://github.com/nsaintgeours/sparkyfruit
** https://github.com/AdamVincent90/SimpleCNN

* Documentations
** https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox
** https://phoenixnap.com/kb/install-spark-on-ubuntu

## I. Fonctionnement en local (sur mon PC)
### Installation d'Ubuntu / VirtualBox
https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview
#### Aperçu
* Télécharger une image Ubuntu Image (https://ubuntu.com/download/desktop/thank-you?version=22.04&architecture=amd64)
** Ubuntu 22.04 LTS, The Jammy Jellyfish (la méduse chanceuse), sorti le 21 avril 2022, soutenu jusqu'en Avril 2027
* Téléchargez et installez VirtualBox (https://www.virtualbox.org/wiki/Downloads)
* Une fois l'installation terminée, exécutez VirtualBox.
#### Créer une nouvelle machine virtuelle
** Type: Linux, Version: Ubuntu (64-bit), 8Gb RAM, 100 Go vdi disk
![import_pyspark](https://github.com/GreyFrenchKnight/cloud-fruits-P8/blob/c0fb6c4d13afda42b969b155ba663eb755863a5b/images/import%20pyspark.png)

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
pip3 install boto3 pillow torch
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

#### Installation de Spark
https://phoenixnap.com/kb/install-spark-on-ubuntu
Extraire le fichier téléchargé Spark qui se trouve dans le dossier '/home'
```
sudo tar -zxvf spark-3.2.1-bin-hadoop3.2.tgz
sudo mv spark-3.2.1-bin-hadoop3.2 /opt/spark
```
Utilisez la commande echo pour ajouter ces trois lignes à .profile :
```
echo "export SPARK_HOME=/opt/spark" >> ~/.profile
echo "export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin" >> ~/.profile
echo "export PYSPARK_PYTHON=/usr/bin/python3" >> ~/.profile
```
Il faut autoriser l'accès à l'environnement spark :
```
sudo chmod 777 spark-3.2.1-bin-hadoop3.2
```
La configuration SPARK est prête. Il est possible de vérifier l'environnement spark via le terminal :
```
cd spark-3.2.1-bin-hadoop3.2/python/
python3
> import pyspark
```

Configurer l'environnement de drivers pour Jupyter Notebook pour fonctionner dans l'environnement SPARK pour utiliser le package pyspark :
```
export PYSPARK_DRIVER_PYTHON=”jupyter”
export PYSPARK_DRIVER_PYTHON_OPTS=”notebook”
export PYSPARK_PYTHON=python3
```
Il faut également vérifier l'environnement spark via l'import dans Jupyter Notebook.

### Copie des fichiers sur la VM
* Copie des dossiers de Training/Test dans le dossier partagé VirtualBox
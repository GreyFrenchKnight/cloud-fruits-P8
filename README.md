# cloud-fruits-P8 Déployez un modèle dans le cloud
**Vous êtes Data Scientist dans une très jeune start-up de l'AgriTech, nommée  "Fruits!", qui cherche à proposer des solutions innovantes pour la récolte des fruits.**

#### Exemples Github :
* [Application P8 sparkyfruit de nsaintgeours](https://github.com/nsaintgeours/sparkyfruit)
* [SimpleCNN de AdamVincent90](https://github.com/AdamVincent90/SimpleCNN)
* [Fruit-Images-Dataset de Horea94](https://github.com/Horea94/Fruit-Images-Dataset)

#### Documentations :
* VirtualBox, Ubuntu :
* [Installation Ubuntu VirtualBox](https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox)
* Spark, Torch, TorchVision, CNN, Transfer Learning :
* [Spark et Jupyter Notebook](https://python.plainenglish.io/apache-spark-using-jupyter-in-linux-installation-and-setup-b2cacc6c7701)
* [Spark et Jupyter Notebook](https://www.codeitbro.com/install-pyspark-on-ubuntu-with-jupyter-notebook/)
* [Spark et Jupyter Notebook](https://www.bmc.com/blogs/jupyter-notebooks-apache-spark/)
* [Spark et Jupyter Notebook](https://github.com/ruslanmv/Tutorial-of-Pyspark-with-Jupyter-Notebook)
* [Exemples Spark](https://github.com/spark-examples/pyspark-examples)
* [Spark user defined class broadcast](https://stackoverflow.com/questions/43042241/broadcast-a-user-defined-class-in-spark)
* [Torchvision & Transfer Learning](https://getpocket.com/fr/read/2721181304)
* AWS :
* [AWS S3 access keys](https://medium.com/@shamnad.p.s/how-to-create-an-S3-bucket-and-aws-access-key-id-and-secret-access-key-for-accessing-it-5653b6e54337)
* EC2, SSH :
* [Change the instance type of an existing EBS backed instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-resize.html)
* [Installer OpenSSH sur Windows](https://docs.microsoft.com/fr-fr/windows-server/administration/openssh/openssh_install_firstuse)
* Disk full :
* [How to increase the AWS Disk Volume (EBS) on EC2 in simple steps for Ubuntu?](https://medium.com/geekculture/how-to-increase-the-aws-disk-volume-ebs-on-ec2-in-simple-steps-for-ubuntu-7c3759468b38)
* [Unable to growpart because no space left](https://stackoverflow.com/questions/59420015/unable-to-growpart-because-no-space-left)

# I. Fonctionnement en local (PySpark/Notebook/Dataset sur mon PC)

## Installation d'Ubuntu / VirtualBox [Tutoriel](https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview)

#### Aperçu
* Télécharger une [Image Ubuntu](https://ubuntu.com/download/desktop/thank-you?version=22.04&architecture=amd64).
* Ubuntu 22.04 LTS, The Jammy Jellyfish (la méduse chanceuse), sorti le 21 avril 2022, soutenu jusqu'en Avril 2027.
* Téléchargez et installez [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
* Une fois l'installation terminée, exécutez VirtualBox.

#### Créer une nouvelle machine virtuelle
* Type: Linux, Version: Ubuntu (64-bit), 8Gb RAM, 100 Go vdi disk

#### Installer votre image [Tutoriel Install Ubuntu/VirutalBox](https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#3-install-your-image)
* Cliquez sur Démarrer pour lancer la machine virtuelle. Vous serez invité à sélectionner le disque de démarrage.
* Utilisez l'icône de fichier pour ouvrir le sélecteur de disque optique et cliquez sur Ajouter pour trouver votre fichier.iso
* Choisissez l'image disque que vous souhaitez utiliser, puis cliquez sur Démarrer dans la fenêtre du disque de démarrage.
* Le bureau Ubuntu devrait maintenant démarrer et afficher le menu d'installation.
* Après ce point, vous pouvez [suivre le flux d'installation normal pour Ubuntu Desktop](https://ubuntu.com/tutorials/install-ubuntu-desktop#11-installation-complete).
* Après le rédemarrage, le système d'opération Ubuntu est installé !

#### Modification de la résolution de la fenêtre
* Remplacez le paramètre Contrôleur graphique par VBoxSVGA et cliquez sur OK (ignorez l'avertissement).

#### Installation des ajouts d'invités
* Insérer le CD depuis le menu de VirtualBox et procéder à l'installation.
* Revenez au menu Paramètres et redéfinissez le contrôleur graphique sur VMSVGA et Activez l'accélération 3D.
* Une autre fonctionnalité que cela déverrouille est le presse-papiers partagé, que vous pouvez activer dans Périphériques > Presse-papiers partagé. Cela vous permettra de copier et coller entre vos machines virtuelles et hôtes, utile lorsque vous souhaitez copier des sorties d'un périphérique à l'autre.

## Installation des dépendances (Pip, Python3, Jupyter Notebook, Spark, Librairies annexes)

#### [Installation de pip pour Python 3](https://linuxize.com/post/how-to-install-pip-on-ubuntu-20.04/)
* Pour installer pip pour Python 3 sur Ubuntu 20.04, exécutez les commandes suivantes en tant qu'utilisateur root ou sudo dans votre terminal:
```
sudo apt update
sudo apt install python3-pip
```
* [Installation de Jupyter Notebook](https://python.plainenglish.io/apache-spark-using-jupyter-in-linux-installation-and-setup-b2cacc6c7701)
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
pip3 install py4j
```

#### [Installation de Spark](https://phoenixnap.com/kb/install-spark-on-ubuntu)
* Télécharger l'archive spark/hadoop (wget https://downloads.apache.org/spark/spark-3.2.1/spark-3.2.1-bin-hadoop3.2.tgz)
```
wget https://downloads.apache.org/spark/spark-3.2.1/spark-3.2.1-bin-hadoop3.2.tgz
```
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
![import_pyspark](/Images/import-pyspark.png)

* Configurer l'environnement de drivers pour Jupyter Notebook pour fonctionner dans l'environnement SPARK pour utiliser le package pyspark :
```
echo "export PYSPARK_DRIVER_PYTHON='jupyter'" >> ~/.bashrc
echo "export PYSPARK_DRIVER_PYTHON_OPTS='notebook --no-browser --port=8889'" >> ~/.bashrc
```
![ajouts_bashrc](/Images/ajouts-bashrc.PNG)

* Il faut également vérifier l'environnement spark via l'import dans Jupyter Notebook. Dans un terminal : 
```
> pyspark
```
* La ligne de commande pyspark lance Jupyter Notebook. Dans un Notebook, dans une cellule de code python, saisir la commande "import spark" et constater que la cellule s'exécute correctement.

**L'environnement est complètement installé. Jupyter Nootebook fonctionne avec PySpark.**

## [Copie des fichiers sur la VM](https://unix.stackexchange.com/questions/16199/how-to-transfer-files-from-windows-to-ubuntu-on-virtualbox)
* Création d'un dossier partagé et montage de ce dossier sur Ubuntu
```
sudo mount -t vboxsf Shared_folders_VM_Ubuntu_Spark Shared_folder_Windows
```
* Copie des dossiers de Training/Test dans le dossier partagé VirtualBox

## Développement du code dans un notebook PySpark 
* Création d'une SparkSession, lecture des images du dossier cloud-fruits-dataset et application de l'encodage avant de les traiter avec le modèle CNN Transfer Learning sans la dernière couche.
* Un fichier output.csv ou output.parquet est généré, il contient les features de chaque image, prêts à être envoyé dans une couche de classification pour prédire le type de fruit.
* consulter le fichier [P8 Local PySpark and local dataset.ipynb](https://github.com/GreyFrenchKnight/cloud-fruits-P8/blob/e23def03bd74b4bb3c34930d537f3f41ae0b1d20/Code/P8%20Local%20PySpark%20and%20local%20dataset.ipynb)

**Je parviens à exécuter du code spark sur une machine Ubuntu hébergée en local sur VirtualBox qui traite des données hébergées sur la même machine. Un fichier parquet est généré en sortie de process.**
**Celui-ci contient les features calculées par le CNN Transfer Learning, prêtes à être ingérées par une couche de classification qui permettra de déterminer le type de fruit.**

# II. Fonctionnement en local (PySpark/Notebook sur mon PC) avec dataset sur bucket S3

## Création d'un espace de stockage S3 sur AWS
J'ai copié le jeu de données Fruits 360 Dataset sur un espace de stockage S3 lié à mon compte AWS

* création d'un bucket via la console AWS en ligne : cloud-fruits-p8-bucket
![bucket_folders](/Images/S3-cloud-fruits-p8-bucket.PNG)
* création d'un utilisateur S3 via la console AWS en ligne
![IAM_user](/Images/IAM-user.PNG)

## Copie des fichiers sur S3 (SDK boto3)
* enregistrement sur mon PC d'un fichier contenant les clés d'accès à mon stockage S3 : voir fichier ~/.aws/credentials
```
[default]
aws_access_key_id=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
aws_secret_access_key=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```
* Copie des images du dataset de Training dans le bucket avec un notebook et SDK boto3
[Accès à mon AWS S3 Bucket](https://S3.console.aws.amazon.com/S3/buckets/cloud-fruits-p8-bucket?region=eu-west-3&tab=objects)

## Configuration de Spark en local pour accéder à S3
Il faut éditer la configuration de Spark pour permettre l'accès aux données stockées sur AWS S3 directement via Spark / Hadoop.
* Pour ce faire, on se rend dans le dossier /opt/spark/conf sur la machine virtuelle Ubuntu.
* copier le fichier spark-defaults.conf.template vers un nouveau fichier spark-defaults.conf
* éditer le fichier spark-defaults.conf en y ajoutant les lignes suivantes (les clés AWS S3 sont disponibles sur ma console AWS en ligne) :
```
spark.jars.packages             com.amazonaws:aws-java-sdk-bundle:1.11.901,org.apache.hadoop:hadoop-aws:3.3.1
spark.hadoop.fs.s3a.access.key  XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
spark.hadoop.fs.s3a.secret.key  XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
spark.hadoop.fs.s3a.endpoint    S3.eu-west-3.amazonaws.com
spark.hadoop.fs.s3a.impl        org.apache.hadoop.fs.s3a.S3AFileSystem
```
* J'ai également du modifier certaines versions de jar suite à un problème de classe Java non trouvée [Class org.apache.hadoop.fs.s3a.auth.IAMInstanceCredentialsProvider not found when trying to write data on S3 bucket from Spark](https://stackoverflow.com/questions/71546208/class-org-apache-hadoop-fs-s3a-auth-iaminstancecredentialsprovider-not-found-whe):
```
cd /opt/spark/jars
sudo wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.901/aws-java-sdk-bundle-1.11.901.jar
sudo wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.1/hadoop-aws-3.3.1.jar
sudo wget https://repo1.maven.org/maven2/com/google/guava/guava/23.0/guava-23.0.jar
sudo rm -rf **les anciennes versions de ces jars***
```

## Mise à jour du code dans un notebook PySpark pour lire les fichiers sur un bucket S3 (SDK boto3 ou API S3a)
* Création d'une SparkSession, lecture des images du bucket cloud-fruits-p8-bucket et application de l'encodage avant de les traiter avec le modèle CNN Transfer Learning sans la dernière couche.
* Un fichier output.parquet est généré, il contient les features de chaque image, prêts à être envoyé dans une couche de classification pour prédire le type de fruit. Il est stocké dans le dossier output_features_and_images_processed/%Y%m%d-%H%M%S-batch/features_to_classify.
* les fichiers sont déplacés du dossier input_images_to_process vers le dossier output_features_and_images_processed/%Y%m%d-%H%M%S-batch/images_processed
* consulter le fichier [P8 PySpark and AWS S3 Bucket Dataset.ipynb](https://github.com/GreyFrenchKnight/cloud-fruits-P8/blob/e23def03bd74b4bb3c34930d537f3f41ae0b1d20/Code/P8%20PySpark%20and%20AWS%20S3%20Bucket%20Dataset.ipynb)

**Je parviens à exécuter du code spark sur une machine Ubuntu hébergée en local sur VirtualBox qui traite des données hébergées sur un bucket S3 AWS. Un fichier parquet est généré en sortie de process.**
**Celui-ci contient les features calculées par le CNN Transfer Learning, prêtes à être ingérées par une couche de classification qui permettra de déterminer le type de fruit.**

# III. Fonctionnement EC2 (PySpark/Notebook sur une macbine EC2 sur AWS) avec dataset sur bucket S3
On souhaite réaliser la même opération mais avec un Ubuntu hébergé sur EC2 qui accède aux données sur S3.

## Création d'une image EC2 sur AWS
* Création d'une image EC2 Ubuntu 22.04 LTS à l'aide de la console EC2 AWS.
![lancer-instance-ec2](/Images/ec2-new-instance.PNG)
* Type t2.medium et paire de clés pem
![lancer-instance-ec2-type-keys](/Images/ec2-new-instance-type-keys.PNG)
* Résumé de la configuration avant le lancement de l'instance
![lancer-instance-ec2-summary](/Images/ec2-new-instance-summary.PNG)

## Connexion avec [OpenSSH](https://docs.microsoft.com/fr-fr/windows-server/administration/openssh/openssh_install_firstuse) et paire de clés sécurisées [Connect to your Linux instance using an SSH client](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)
* Vérifier si les composants OpenSSH Client et OpenSSH Server sont installés dans "Paramètres", sélectionnez "Applications > Applications et fonctionnalités", puis "Fonctionnalités facultatives", sinon les installer.
* Pour démarrer et configurer OpenSSH Server pour une première utilisation, ouvrez PowerShell en tant qu’administrateur, puis exécutez les commandes suivantes pour démarrer sshd service :
```
# Start the sshd service
Start-Service sshd

# OPTIONAL but recommended:
Set-Service -Name sshd -StartupType 'Automatic'

# Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```
* Depuis un terminal :
```
> ssh -i /path/my-key-pair.pem my-instance-user-name@my-instance-public-dns-name
```
* Dans mon cas :
```
# > ssh -i C:/Users/disch/Documents/OpenClassrooms/Workspace/20220606_Projet_8_Deployez_un_modele_dans_le_cloud/Projet_8/paire-cles-cloud-fruits-P8.pem ubuntu@35.180.41.4
```
![connect-ec2-ssh](/Images/ec2-ssh-connection-cleaned.png)
* Création d'un instantané de l'état initial du disque de cette machine EC2 avant de procéder aux installations et à la configuration.

## Installation des dépendances (Pip, Python3, Jupyter Notebook, Spark, Librairies annexes)
* [How to Install Jupyter Notebook on AWS EC2 Instance for Machine Learning and Python scripting](https://medium.com/@awsontop/how-to-install-jupyter-notebook-on-aws-ec2-instance-for-machine-learning-and-python-scripting-10a3dcba3b6e)
* Installer Anaconda
* Configurer Jupyter
* [Générer le mot de passe encrypté](https://stackoverflow.com/a/68467707)
* Modifier le fichier de configuration 'jupyter/jupyter_notebook_config.py' déjà généré avec le certificat et le mot de passe ci-dessus.
```
> nano .jupyter/jupyter_notebook_config.py
```
* Aller à la dernière ligne et ajoutez le code ci-dessous :
```
c = get_config()
# Kernel config
c.IPKernelApp.pylab = 'inline'
# Notebook config
c.NotebookApp.certfile = u'/home/ubuntu/certs/jupyterpython1.pem'
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.password = u'sha1:xxx:yyy'
c.NotebookApp.port = 8888 #same port number as configured in SG setup.
```
* Lancement de Jupyter Notebook
```
> jupyter notebook --allow-root
```
* Accès à Jupyter Notebook avec l'url https://<IP-PUBLIQUE-EC2>:8888/tree et saisi du mot de passe pour se connecter [Mon URL JUPYTER EC2](https://35.180.41.4:8888/tree)

## Configuration de Spark pour accéder à S3
Voir la description de l'étape du chapitre II.
 
* Utilisez la commande echo pour ajouter ces lignes à .bashrc :
```
echo "export SPARK_HOME=/opt/spark" >> ~/.bashrc
echo "export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin" >> ~/.bashrc
echo "export PYTHONPATH=/root/anaconda3/bin/python" >> ~/.bashrc
```

* Configurer l'environnement de drivers pour Jupyter Notebook pour fonctionner dans l'environnement SPARK pour utiliser le package pyspark :
```
echo "export PYSPARK_PYTHON=/root/anaconda3/bin/python" >> ~/.bashrc
echo "export PYSPARK_DRIVER_PYTHON='jupyter'" >> ~/.bashrc
echo "export PYSPARK_DRIVER_PYTHON_OPTS='notebook --no-browser --port=8888'" >> ~/.bashrc
```

## Réutilisation du code dans notebook PySpark pour lire les fichiers sur un bucket S3 (SDK boto3 ou API S3a)
* Upload des fichiers en SCP depuis Windows avec permission d'écriture sur le dossier de destination Ubuntu EC2 :
```
mkdir /home/ubuntu/Notebooks
chmod 755 /home/ubuntu/Notebooks
scp -i C:/Users/disch/Documents/OpenClassrooms/Workspace/20220606_Projet_8_Deployez_un_modele_dans_le_cloud/Projet_8/paire-cles-cloud-fruits-P8.pem -r "C:/Users/disch/Documents/OpenClassrooms/Workspace/20220606_Projet_8_Deployez_un_modele_dans_le_cloud/Projet_8/Git/cloud-fruits-P8/Code" ubuntu@35.180.41.4:/home/ubuntu/Notebooks/

mkdir /home/ubuntu/fruits-360-dataset
chmod 755 /home/ubuntu/fruits-360-dataset
scp -i C:/Users/disch/Documents/OpenClassrooms/Workspace/20220606_Projet_8_Deployez_un_modele_dans_le_cloud/Projet_8/paire-cles-cloud-fruits-P8.pem -r "C:/Users/disch/Documents/OpenClassrooms/Workspace/20220606_Projet_8_Deployez_un_modele_dans_le_cloud/Projet_8/Git/cloud-fruits-P8/Fruits-360-Dataset/LightTraining" ubuntu@35.180.41.4:/home/ubuntu/fruits-360-dataset/LightTraining
```
* Création d'une SparkSession, lecture des images du bucket cloud-fruits-p8-bucket et application de l'encodage avant de les traiter avec le modèle CNN Transfer Learning sans la dernière couche.
* Un fichier output.parquet est généré, il contient les features de chaque image, prêts à être envoyé dans une couche de classification pour prédire le type de fruit. Il est stocké dans le dossier output_features_and_images_processed/%Y%m%d-%H%M%S-batch/features_to_classify.
* les fichiers sont déplacés du dossier input_images_to_process vers le dossier output_features_and_images_processed/%Y%m%d-%H%M%S-batch/images_processed
* consulter le fichier [P8 PySpark and AWS S3 Bucket Dataset.ipynb](https://github.com/GreyFrenchKnight/cloud-fruits-P8/blob/e23def03bd74b4bb3c34930d537f3f41ae0b1d20/Code/P8%20PySpark%20and%20AWS%20S3%20Bucket%20Dataset.ipynb)

**Je parviens à exécuter du code spark sur une machine Ubuntu hébergée sur EC2 AWS qui traite des données hébergées sur un bucket S3 AWS. Un fichier parquet est généré en sortie de process.**
**Celui-ci contient les features calculées par le CNN Transfer Learning, prêtes à être ingérées par une couche de classification qui permettra de déterminer le type de fruit.**


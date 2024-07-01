# Pipeline automatisée CI/CD avec intégration GitHub et Amazon S3

## Aperçu
Ce guide fournit les étapes pour configurer un **pipeline d'intégration continue/déploiement continu (CI/CD)** en utilisant GitHub et Amazon S3. En suivant ces étapes, vous pourrez automatiser le processus de déploiement de votre application, assurant qu'elle est toujours mise à jour avec les dernières modifications de code. Ce pipeline vous aidera à créer une application sécurisée, évolutive et full-stack capable de gérer des niveaux élevés de trafic.

## Configuration du référentiel GitHub
Cette section vous guidera à travers le processus de configuration d'un référentiel GitHub qui sera la source de votre pipeline CI/CD.

1. Créez un compte GitHub.
2. Créez un nouveau référentiel où votre projet résidera.
3. Chargez tous les fichiers de votre projet.

## Configuration du bucket S3
Cette section fournit les étapes pour configurer un bucket Amazon S3. Ce bucket servira comme environnement de déploiement pour votre application.

1. Ouvrez S3 dans un onglet séparé.
2. Cliquez sur "Create Bucket".
3. Entrez les informations suivantes : Nom du bucket="nom_unique_au_niveau_global ou MyFirstApp123456", Région AWS="choisissez celle la plus proche de votre pays ou choisissez US-East-1 par défaut". **Note**: Le nom du bucket doit être unique au niveau global parmi tous les noms de bucket existants dans Amazon S3.
4. Dans "Block public access (bucket settings)", désélectionnez toutes les options. **Note**: Désélectionner ces options rendra votre bucket accessible publiquement. Assurez-vous de vouloir le faire.
5. Laissez les paramètres par défaut et cliquez sur "Create bucket".
6. Dans "Buckets", sélectionnez "Properties".
7. Dans "Static website hosting", cliquez sur "Edit".
8. Dans "Static website hosting", sélectionnez "Enable".
9. Dans "Hosting type", sélectionnez "Host static website".
10. Dans "Index", écrivez "index.html".
11. Laissez les paramètres par défaut et cliquez sur "Save changes".
12. Dans "Buckets", sélectionnez "Permission".
13. Dans "Bucket Policy", cliquez sur "Edit".
14. Insérez la politique JSON fournie dans ce projet. Si vous n'avez pas la politique JSON, vous pouvez la trouver [ici](https://github.com/r-ramos2/Pipeline-automatis-e-CI-CD-avec-int-gration-GitHub-et-Amazon-S3-French/blob/main/s3_public_read_policy.json).
15. Remplacez "arn:aws:s3:::Bucket-Name/*" par l'ARN du Bucket en haut de l'écran. **Note**: L'ARN du Bucket est un identifiant unique pour votre bucket.
16. Cliquez sur "Save changes".

## Configuration de CodePipeline
Cette section est le cœur de votre pipeline CI/CD. Elle vous guidera à travers le processus de configuration d'un pipeline dans AWS CodePipeline, automatisant le processus de déploiement depuis votre référentiel GitHub vers votre bucket Amazon S3.

1. Ouvrez CodePipeline dans un onglet séparé.
2. Cliquez sur "Create pipeline".
3. Entrez les informations suivantes : Nom du pipeline="nom_au_choix ou MyPipeline123456", Type de pipeline="V1", Nom du rôle="incluez les détails comme le service AWS que vous utilisez, la région AWS et le nom du projet d'origine". **Note**: La définition de type de pipeline V1 contient les paramètres standard de pipeline, phase et niveau d'action. La définition de type de pipeline V2 étend la définition pour ajouter des sections de configurations supplémentaires comme les déclencheurs et les variables.
4. Cochez la case "Allows AWS CodePipeline to create a service role so it can be used with this new service", puis cliquez sur "Next".
5. Dans "Source provider", sélectionnez "GitHub (Version 2)".
6. Dans "Connection", cliquez sur "Connect to GitHub".
7. Dans "Create GitHub App connection", entrez le nom de la connexion="nom_au_choix ou MyPipeline123456".
8. Cliquez sur "Connect to GitHub", puis sur "Install a new app".
9. Sur l'écran GitHub, faites défiler jusqu'à "Repository access".
10. Sélectionnez le référentiel auquel vous souhaitez accorder l'accès, puis cliquez sur "Save" et ensuite sur "Connect".
11. Dans "Connection", cliquez sur la connexion GitHub que vous venez de créer.
12. Dans "Repository name", sélectionnez le bucket S3 correspondant.
13. Dans "Pipeline trigger", sélectionnez "Push in a branch".
14. Dans "Branch name", sélectionnez "main".
15. Dans "Output artifact format", sélectionnez "CodePipeline default", puis cliquez sur "Next".
16. Dans "Build - optional", choisissez selon vos besoins soit "Skip build stage", puis cliquez sur "Next".
17. Entrez les informations suivantes : Fournisseur de déploiement=Amazon S3, Région=Votre région actuelle, Bucket=Votre bucket S3, Clé d'objet S3=clic sur "Extract file before deploy", puis cliquez sur "Next" et enfin sur "Create pipeline".
18. Attendez que l'écran montre des signes verts pour Source et Deploy.

## Phase de Test
Cette section vous guidera à travers le processus de test de votre pipeline CI/CD.

1. Ouvrez S3 dans un onglet séparé.
2. Dans "Buckets", sélectionnez "Properties".
3. Sous "Static website hosting", cliquez sur l'URL sous "Bucket website endpoint".
4. Ouvrez le référentiel GitHub avec votre projet, apportez une modification et enregistrez.
5. Ouvrez CodePipeline et vérifiez que la modification est détectée.
6. Mettez à jour l'URL sous "Bucket website endpoint" et observez la modification.

## Nettoyage
Pour éviter des coûts inutiles, assurez-vous qu'aucune ressource sous-jacente n'est toujours en cours d'exécution.

Voici comment vider et supprimer le CodePipeline :
1. Accédez au tableau de bord de CodePipeline.
2. Cliquez sur le pipeline que vous souhaitez supprimer.
3. Cliquez sur le bouton "Delete" pour supprimer le pipeline.

Voici comment vider et supprimer le bucket S3 :
1. Accédez au tableau de bord de S3.
2. Cliquez sur le bucket que vous souhaitez supprimer.
3. Cliquez sur le bouton "Empty" pour supprimer tous les objets du bucket.
4. Une fois le bucket vide, cliquez sur le bouton "Delete" pour supprimer le bucket.
**Note**: Vous devrez peut-être supprimer la stratégie du bucket avant de pouvoir le supprimer.

## Conclusion
Dans ce projet, nous avons démontré comment déployer un pipeline d'intégration continue/déploiement continu (CI/CD) en utilisant GitHub et Amazon S3. En suivant ces étapes, nous avons créé un pipeline CI/CD entièrement fonctionnel qui déploie une application dans un bucket Amazon S3 à chaque fois qu'il y a des modifications poussées vers le référentiel GitHub. N'hésitez pas à nous envoyer des suggestions pour améliorer le code.

## Remerciements
Ce tutoriel est adapté du [Tutorial: Create a pipeline that uses Amazon S3 as a deployment provider website](https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-s3deploy.html) fourni par Amazon Web Services. Nous remercions AWS pour avoir fourni cette précieuse ressource, qui a servi de base pour le tutoriel "Pipeline automatisée CI/CD avec intégration GitHub et Amazon S3".


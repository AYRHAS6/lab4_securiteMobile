Le challenge UnCrackable Level 1 consiste à analyser une application Android pour découvrir un mot de passe caché permettant de passer une vérification interne. 

Pour cela, on décompile l’APK avec un outil comme JADX afin d’examiner le code source, repérer la fonction de validation et comprendre comment le secret est stocké ou 

transformé. L’objectif est donc d’extraire ce “secret” en analysant la logique de l’application, tout en tenant compte de quelques protections basiques comme la détection 

de root ou de debug, ce qui en fait un exercice d’initiation au reverse engineering mobile.

### JADX

JADX est un outil de rétro-ingénierie utilisé en cybersécurité et en développement Android pour décompiler des fichiers APK, DEX ou AAB en code source lisible (Java ou Kotlin).

Son rôle principal est de permettre aux analystes de comprendre le fonctionnement interne d’une application, d’identifier des vulnérabilités, d’analyser des comportements 

suspects ou encore d’étudier la logique métier d’une app. Il est très utilisé en pentesting mobile et en reverse engineering, car il offre une interface graphique simple 

et une lecture claire du code décompilé.

On commence d'abord par ouvrir l'APK dans JADX.

<img width="1323" height="754" alt="jadx gui" src="https://github.com/user-attachments/assets/abcb3b7e-c68f-4a8e-8e24-b8b4ab907367" />

Depuis le fichier AndroidManifest.xml, on determine le fichier java principal, dans notre cas c'est MainActivity.java depuis  cette ligne: 

android:name="sg.vantagepoint.uncrackable1.MainActivity">

Dans MainActivity on apercoit la fonction verify :

<img width="1153" height="550" alt="2" src="https://github.com/user-attachments/assets/042cf15b-0c61-490a-b748-5e155c8ad325" />

qui nous mene vers a.java

<img width="1558" height="763" alt="3" src="https://github.com/user-attachments/assets/2d7dc2c6-d4b3-403f-93e2-062d750aa301" />

On voit bien que la cle et le strng sont hard coded :

Ainsi, on va dans CyberChef, et on decode la chaine avec la cle fournie : 

![Uploading cyber.jpeg…]()



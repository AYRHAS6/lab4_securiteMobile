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

<img width="1600" height="785" alt="cyber" src="https://github.com/user-attachments/assets/4ad55f45-a4de-4eba-abee-9e16bce35752" />


En tapant I want to beleive dans l'application, on aura le message de succes! 

<img width="357" height="668" alt="result" src="https://github.com/user-attachments/assets/c78e4ea1-2aa3-4f1d-8a32-bfc727c63de8" />


--------------------------------------------------------------------------------------------------------------------------------------------------------------------

#  Rapport d'analyse statique - UnCrackable-Level1

---

##  Informations générales

- **Date d'analyse :** 26/04/2026  
- **Analyste :** Aya  
- **APK analysé :** UnCrackable-Level1.apk  
- **Version :** Non spécifiée  
- **Provenance :** OWASP Crackme Challenge  
- **Outils utilisés :** JADX GUI

---

##  Résumé exécutif

Cette analyse statique a révélé **2 vulnérabilités majeures** dans l'application UnCrackable-Level1.  
Les principales préoccupations concernent :

- l’exposition d’un secret sensible directement dans le code  
- une implémentation cryptographique faible avec clé hardcodée  

 Le niveau de risque global est évalué comme **ÉLEVÉ **

---

###  Actions prioritaires recommandées

1. Supprimer toute donnée sensible hardcodée dans le code source  
2. Implémenter une gestion sécurisée des clés (Keystore Android)  
3. Renforcer la logique de vérification côté serveur  

---

##  Constats détaillés

---

###  Constat #1 : Secret hardcodé dans le code

**Sévérité :** Élevée  

**Description :**  
L’analyse du fichier `a.java` montre que l’application contient une chaîne encodée en dur (hardcoded) ainsi qu’une clé utilisée pour la déchiffrer. Le résultat est directement comparé à l’entrée utilisateur pour valider l’accès.

**Localisation :**
- Classe : `sg.vantagepoint.uncrackable1.a`  
- Méthode : `a(String str)`  

**Preuve technique :**
- Chaîne encodée en Base64  
- Clé hexadécimale présente dans le code  
- Comparaison directe via `str.equals(...)`  

**Impact potentiel :**
- Extraction facile du secret via reverse engineering  
- Contournement total de la sécurité  
- Aucune protection contre l’analyse statique  

**Remédiation recommandée :**
- Ne jamais stocker de secrets dans le code  
- Externaliser la logique de validation côté serveur  
- Utiliser de l’obfuscation avancée  

---

###  Constat #2 : Mauvaise gestion cryptographique (clé exposée)

**Sévérité :** Élevée  

**Description :**  
La clé utilisée pour le déchiffrement AES est visible directement dans le code sous forme de chaîne hexadécimale, rendant la protection inefficace.

**Localisation :**
- Classe : `sg.vantagepoint.uncrackable1.a`  
- Méthode : `b(String str)` + appel dans `a()`  

**Impact potentiel :**
- Déchiffrement trivial du secret  
- Bypass complet de la logique de sécurité  
- Facilite les attaques automatisées  

**Remédiation recommandée :**
- Utiliser Android Keystore pour stocker les clés  
- Générer les clés dynamiquement  
- Éviter toute exposition côté client  

---

##  Conclusion

Cette application repose principalement sur une **sécurité côté client faible**, facilement contournable via du reverse engineering.

 Ce challenge démontre :
- les risques du hardcoding  
- les mauvaises pratiques cryptographiques  
- l’importance de la sécurité côté serveur  

---

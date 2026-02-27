# Analyse Statique d'une Application Android – PizzaRecipes

## Introduction

Dans ce projet, j’ai réalisé une analyse statique de l’application Android **PizzaRecipes** à partir du fichier `app-debug.apk`.

L’objectif était d’examiner la structure interne de l’APK sans exécuter l’application, afin de comprendre son fonctionnement et d’identifier d’éventuels problèmes de sécurité.

---

## Environnement de travail

- Système : Windows
- Outils utilisés :
  - JADX GUI
  - dex2jar
  - JD-GUI
  - PowerShell

---

## Task 1 – Préparation

### 1-Création d’un dossier de travail :

mkdir C:\APK-Analysis
cd C:\APK-Analysis

![](https://github.com/user-attachments/assets/20fc43a1-05eb-4d92-b248-b2085d39eae5)
### 2-Copiez l'APK à analyser dans ce dossier.
![](https://github.com/user-attachments/assets/7a40c603-feb5-425f-b23e-63cf027f9315)
### 3-Vérifiez que l'APK est bien une archive ZIP :
Get-Content -Path .\app-debug.apk -TotalCount 4 | Format-Hex
![](https://github.com/user-attachments/assets/3e784d92-cda0-44ba-a1c5-5d792ea12d9c)
### 4-Listez le contenu de l'APK :
Add-Type -Assembly System.IO.Compression.FileSystem
[System.IO.Compression.ZipFile]::OpenRead(".\app-debug.apk").Entries.FullName | Select-Object -First 20
![](https://github.com/user-attachments/assets/ea1ad976-c37f-4518-9ec6-c3c9ece9075d)
### 5-Calculez le hash de l'APK pour traçabilité :
Get-FileHash -Algorithm SHA256 .\app-debug.apk
![](https://github.com/user-attachments/assets/d8658098-d33d-43b5-b359-cfc956dd842c)

## Task 2 — Obtenir un APK pour l'analyse

**Objectif :** S'assurer de disposer d'un APK valide pour l'analyse.

### Option B — Générer un APK depuis Android Studio
![](https://github.com/user-attachments/assets/ab0b2de1-125a-4834-9651-fe8ea0c9b6c7)
## Task 3 — Analyse avec JADX GUI

**Objectif :** Explorer la structure de l'APK et analyser son manifeste.

### Étapes

 **Lancer JADX GUI**  
   Sur Windows, exécutez :  
   ```powershell
   start "" "C:\Path\to\jadx-gui.exe"
   ```
   ![](https://github.com/user-attachments/assets/127b2a6f-40f4-438a-8b61-f7cebb5d83fe)
   ![](https://github.com/user-attachments/assets/ac434e60-3435-46e3-8f71-61395545c9d6)
   ![](https://github.com/user-attachments/assets/5bf8e13a-ecb1-46e0-8daf-713a3830c02a)
   ## Task 4 — Recherche de chaînes sensibles

**Objectif :** Identifier les informations sensibles codées en dur dans l'application.

- J'ai utilisé **JADX GUI** pour chercher globalement :  
  - URLs et endpoints (http, https,url...)  
  - Informations d'authentification 
  - Indicateurs de développement (test,debug)  

- Cette étape permet de voir rapidement si l'application contient des informations sensibles codées en dur.
![](https://github.com/user-attachments/assets/7022cf6d-78c5-4638-92bc-46b911dc3dde)
![](https://github.com/user-attachments/assets/e546e8c3-0f02-42bf-b758-988846c3e66e)
![](https://github.com/user-attachments/assets/49f0fd77-5a73-4813-a9b6-fdd02e6f2fe8)
![](https://github.com/user-attachments/assets/99efc611-5c46-4f11-8444-b00cead11d0e)

## Task 5 — Convertir DEX → JAR avec dex2jar

**Objectif :** Transformer le bytecode Android en format JAR pour pouvoir l’analyser avec JD-GUI.

- J’ai extrait les fichiers **DEX** de l’APK dans un dossier `dex_out` avec PowerShell sur Windows.
  ![](https://github.com/user-attachments/assets/7a119b73-500b-4197-81b1-441e021d7151)
- Ensuite, chaque fichier DEX a été converti en **JAR** avec **dex2jar**.
  ![](https://github.com/user-attachments/assets/623d6e3e-8726-4ec7-8e75-acd618429485)
  ![](https://github.com/user-attachments/assets/0cddf7c9-5044-4504-bb4c-1e20e11f1f12)  

## Task 6 — Comparaison JADX vs JD-GUI (15-20 min)

**Objectif :** Comparer deux outils de décompilation pour mieux comprendre le code de l’application.

- J’ai ouvert le **JAR généré** précédemment avec **JD-GUI**.  
- J’ai comparé le même code avec **JADX GUI** pour observer les différences sur :
  - La navigation dans les classes et packages
  - La lisibilité du code
  - La gestion du code spécifique à Android
  - L’effet de l’obfuscation
  - L’accès aux ressources

**Différences notables observées :**

- **Navigation** :  
  - JADX affiche la structure Android complète (Manifest, ressources, code).  
  - JD-GUI montre seulement la structure Java (packages et classes).  

- **Kotlin** :  
  - JADX gère mieux le code Kotlin.  
  - JD-GUI rend parfois le code Kotlin illisible.  

- **Ressources** :  
  - Accès direct aux fichiers XML et assets dans JADX.  
  - JD-GUI n’affiche pas les ressources Android.  

![](https://github.com/user-attachments/assets/e9bdb0dc-0313-42f6-9b8e-e8d7820972b9)
![](https://github.com/user-attachments/assets/2d064dbe-d93d-43db-9b1f-0ca6dd9e63fb)

## Task 7 — Rédiger le mini-rapport
le rapport est nome : rapport.md existe dans branches master

## Task 8 — Nettoyage

**Objectif :** Organiser vos fichiers d'analyse et garder un environnement propre.

- Créez un dossier pour les résultats :  
```powershell
mkdir -p .\results
move .\app.jar .\results\
move .\rapport.md .\results\
 puis les supprimer
![](https://github.com/user-attachments/assets/e039078b-a0b1-4c0e-a279-c8371093f3a9)


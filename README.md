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

## Étape 1 – Préparation

1-Création d’un dossier de travail :

```powershell
mkdir C:\APK-Analysis
cd C:\APK-Analysis
![] (https://github.com/user-attachments/assets/20fc43a1-05eb-4d92-b248-b2085d39eae5)

### 2-Copiez l'APK à analyser dans ce dossier.

![](https://github.com/user-attachments/assets/7a40c603-feb5-425f-b23e-63cf027f9315)
3-Vérifiez que l'APK est bien une archive ZIP :
Get-Content -Path .\app-debug.apk -TotalCount 4 | Format-Hex
![] (https://github.com/user-attachments/assets/3e784d92-cda0-44ba-a1c5-5d792ea12d9c)
4-Listez le contenu de l'APK :
Add-Type -Assembly System.IO.Compression.FileSystem
[System.IO.Compression.ZipFile]::OpenRead(".\app-debug.apk").Entries.FullName | Select-Object -First 20
![](https://github.com/user-attachments/assets/ea1ad976-c37f-4518-9ec6-c3c9ece9075d)
5-Calculez le hash de l'APK pour traçabilité :
Get-FileHash -Algorithm SHA256 .\app-debug.apk
![] (https://github.com/user-attachments/assets/d8658098-d33d-43b5-b359-cfc956dd842c)


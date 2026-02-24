# Rapport d’analyse statique – PizzaRecipes

## Informations générales

- **Date d’analyse :** 24 février 2026  
- **Analyste :** Wissal Mokdad  
- **APK analysé :** app-debug.apk  
- **Type :** Version debug  
- **Outils utilisés :** JADX GUI, dex2jar, JD-GUI, PowerShell (Windows)

---

## Résumé

Dans ce laboratoire, j’ai réalisé une analyse statique de l’application **PizzaRecipes** à partir du fichier `app-debug.apk`.

L’objectif était d’examiner le contenu interne de l’APK sans exécuter l’application, afin de comprendre sa structure et d’identifier d’éventuels problèmes de sécurité.

Après analyse, j’ai identifié trois points importants :

1. L’application est en version debug  
2. Le code est entièrement lisible (pas d’obfuscation)  
3. Certaines permissions augmentent la surface d’attaque  

Globalement, le niveau de risque est **moyen**, surtout parce que l’application n’est pas en version release sécurisée.

---

## Constats

### Constat 1 : Application en version debug

**Sévérité : Moyenne**

L’APK analysé s’appelle `app-debug.apk`.  
Cela signifie que l’application a été compilée en mode développement.

Une version debug peut contenir :
- Des logs visibles
- Moins de protections
- Une configuration non sécurisée pour la production

**Impact possible :**  
Si cette version est distribuée, elle peut faciliter l’analyse ou l’exploitation.

**Solution recommandée :**  
Créer une version release signée et désactiver le mode debug avant toute mise en production.

---

### Constat 2 : Code facilement lisible

**Sévérité : Élevée**

En utilisant JADX et JD-GUI, j’ai remarqué que :

- Les noms des classes sont clairs
- Les méthodes sont compréhensibles
- La logique de l’application est facile à suivre

Cela montre que l’obfuscation n’est pas activée.

**Impact possible :**  
Un attaquant peut comprendre le fonctionnement interne de l’application et éventuellement la modifier.

**Solution recommandée :**  
Activer R8 ou ProGuard lors de la compilation release afin de rendre le code plus difficile à analyser.

---

### Constat 3 : Permissions réseau déclarées

**Sévérité : Moyenne**

Dans le fichier `AndroidManifest.xml`, l’application demande des permissions comme :

- android.permission.INTERNET  
- android.permission.ACCESS_NETWORK_STATE  

Ces permissions sont normales pour une application utilisant Internet, mais elles doivent être utilisées correctement.

**Impact possible :**  
Si les communications ne sont pas sécurisées (HTTP au lieu de HTTPS), les données peuvent être interceptées.

**Solution recommandée :**  
Vérifier que toutes les communications utilisent HTTPS et supprimer les permissions inutiles.

---

## Comparaison des outils utilisés

Pour cette analyse, j’ai utilisé **JADX GUI** et **JD-GUI**.

Avec JADX, j’ai pu voir toute la structure Android :
- Le Manifest
- Les ressources
- Le code source

C’est l’outil le plus complet pour analyser une application Android.

JD-GUI m’a permis d’ouvrir le fichier JAR après conversion avec dex2jar.  
Il montre uniquement le code Java, sans les ressources Android.

Personnellement, j’ai trouvé que JADX est plus pratique pour une analyse globale, tandis que JD-GUI est utile pour examiner le code Java plus en détail.

---

## Conclusion

Cette analyse m’a permis de mieux comprendre la structure interne d’un fichier APK et le fonctionnement d’une application Android.

Même si l’application PizzaRecipes semble être un projet pédagogique, plusieurs améliorations seraient nécessaires avant une mise en production :

- Utiliser une version release sécurisée  
- Activer l’obfuscation  
- Vérifier la configuration réseau  

L’analyse statique est une étape importante pour identifier les faiblesses d’une application avant sa diffusion.
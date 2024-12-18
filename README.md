### **Manipulation des fichiers CSV en Python**

Un fichier **CSV** (Comma-Separated Values) est un fichier texte où chaque ligne représente une ligne d'un tableau et les colonnes sont séparées par une virgule (ou parfois un autre séparateur comme `;`). C'est un format largement utilisé pour échanger des données.

Python facilite la manipulation des fichiers CSV grâce au module intégré `csv`.

---

## **1. Ouvrir et lire un fichier CSV**

### **1.1. Lire un CSV ligne par ligne**

Avec `csv.reader`, chaque ligne est convertie en **liste**.  
#### Exemple : 
Supposons que le fichier `donnees.csv` contient :  
```
Nom,Âge,Ville
Alice,25,Paris
Bob,30,Lyon
```

Code :  
```python
import csv

with open("donnees.csv", mode="r", encoding="utf-8") as fichier:
    lecteur = csv.reader(fichier)  # Lecture ligne par ligne
    for ligne in lecteur:
        print(ligne)  # Affiche chaque ligne comme une liste
```

**Résultat affiché** :  
```
['Nom', 'Âge', 'Ville']
['Alice', '25', 'Paris']
['Bob', '30', 'Lyon']
```

### **1.2. Lire un CSV sous forme de dictionnaire**

Avec `csv.DictReader`, chaque ligne est transformée en **dictionnaire**. Les clés sont les en-têtes de colonne.  

Code :  
```python
with open("donnees.csv", mode="r", encoding="utf-8") as fichier:
    lecteur = csv.DictReader(fichier)
    for ligne in lecteur:
        print(ligne)  # Affiche chaque ligne comme un dictionnaire
```

**Résultat affiché** :  
```
{'Nom': 'Alice', 'Âge': '25', 'Ville': 'Paris'}
{'Nom': 'Bob', 'Âge': '30', 'Ville': 'Lyon'}
```

**Avantage** : On peut accéder directement aux colonnes avec leur nom :  
```python
print(ligne["Nom"])  # Affiche "Alice"
```

---

## **2. Écrire dans un fichier CSV**

### **2.1. Écrire des lignes sous forme de liste**

Avec `csv.writer`, on ajoute des lignes en les passant sous forme de liste.  

#### Exemple :  
Code :  
```python
with open("resultat.csv", mode="w", encoding="utf-8", newline="") as fichier:
    ecrivain = csv.writer(fichier)  # Préparer l'écriture
    ecrivain.writerow(["Nom", "Âge", "Ville"])  # En-têtes
    ecrivain.writerow(["Alice", 25, "Paris"])   # Données
    ecrivain.writerow(["Bob", 30, "Lyon"])
```

Contenu de `resultat.csv` :  
```
Nom,Âge,Ville
Alice,25,Paris
Bob,30,Lyon
```

### **2.2. Écrire des lignes sous forme de dictionnaire**

Avec `csv.DictWriter`, on écrit chaque ligne sous forme de dictionnaire.  

#### Exemple :  
Code :  
```python
with open("resultat.csv", mode="w", encoding="utf-8", newline="") as fichier:
    champs = ["Nom", "Âge", "Ville"]  # Définir les colonnes
    ecrivain = csv.DictWriter(fichier, fieldnames=champs)
    
    ecrivain.writeheader()  # Écrire les en-têtes
    ecrivain.writerow({"Nom": "Alice", "Âge": 25, "Ville": "Paris"})
    ecrivain.writerow({"Nom": "Bob", "Âge": 30, "Ville": "Lyon"})
```

Contenu de `resultat.csv` :  
```
Nom,Âge,Ville
Alice,25,Paris
Bob,30,Lyon
```

---

## **3. Ajouter des données à un fichier CSV existant**

Pour **ajouter** des lignes sans écraser les données existantes, on ouvre le fichier en mode `a` (**append**).  

#### Exemple :  
Code :  
```python
with open("resultat.csv", mode="a", encoding="utf-8", newline="") as fichier:
    ecrivain = csv.writer(fichier)
    ecrivain.writerow(["Charlie", 35, "Marseille"])  # Ajoute une ligne
```

Si le fichier contient déjà :  
```
Nom,Âge,Ville
Alice,25,Paris
Bob,30,Lyon
```
Après l'exécution, il contiendra :  
```
Nom,Âge,Ville
Alice,25,Paris
Bob,30,Lyon
Charlie,35,Marseille
```

---

## **4. Configurations supplémentaires**

### **4.1. Changer le séparateur**
Si les colonnes sont séparées par un autre caractère (ex. `;`), il faut utiliser le paramètre `delimiter`.  

#### Exemple :  
Code :  
```python
with open("donnees.csv", mode="r", encoding="utf-8") as fichier:
    lecteur = csv.reader(fichier, delimiter=";")
    for ligne in lecteur:
        print(ligne)
```

### **4.2. Gérer les espaces après les séparateurs**
Si des espaces superflus apparaissent après les séparateurs, utilise `skipinitialspace=True`.  

#### Exemple :  
Code :  
```python
lecteur = csv.reader(fichier, delimiter=",", skipinitialspace=True)
```

---

## **5. Alternatives avancées : pandas**

Pour des fichiers CSV volumineux ou des traitements complexes, la bibliothèque **pandas** est recommandée.  

#### Exemple :  
```python
import pandas as pd

# Lire un CSV dans un DataFrame
df = pd.read_csv("donnees.csv")
print(df)

# Ajouter des données et écrire dans un nouveau fichier
df["Pays"] = ["France", "France"]
df.to_csv("nouveau_resultat.csv", index=False)
```

---

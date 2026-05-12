# Exercice 1 : [1.5 × 5 = 7.5 points]

# Questions

1. [Afficher les numéros des lignes vides d’un fichier `f.txt`](#question-a)

2. [Afficher les lignes qui se terminent par un point (`.`) du fichier `f.txt`](#question-b)

3. [Afficher les lignes qui commencent par `+` ou `-` et se terminent par ce même caractère](#question-c)

4. [Vérifier si un utilisateur passé en paramètres est connecté](#question-d)

5. [Trier numériquement la deuxième colonne d’un fichier `notes.txt`](#question-e)

---

<a id="question-a"></a>

# a. Afficher les numéros des lignes vides d’un fichier `f.txt`

[⬅ Retour aux questions](#questions)

## Commande

```sh
grep -n '^$' f.txt
```

## Explication

- `-n` : affiche le numéro de ligne.
- `^$` : représente une ligne vide.
  - `^` = début de ligne
  - `$` = fin de ligne

---

<a id="question-b"></a>

# b. Afficher les lignes qui se terminent par un point (`.`) du fichier `f.txt`

[⬅ Retour aux questions](#questions)

## Commande

```sh
grep '\.$' f.txt
```

## Explication

- `\.` : représente le caractère point littéral.
- `$` : indique la fin de ligne.

---

<a id="question-c"></a>

# c. Afficher les lignes qui commencent par `+` ou `-` et se terminent par ce même caractère

[⬅ Retour aux questions](#questions)

## Commande

```sh
grep -E '^(\+.*\+|-.*-)$' f.txt
```

## Explication

- `^` : début de ligne
- `\+` : caractère `+`
- `-` : caractère `-`
- `.*` : n’importe quelle suite de caractères
- `$` : fin de ligne

### Exemples acceptés

```text
+bonjour+
-test-
```

### Exemples refusés

```text
+bonjour-
-test+
```

---

<a id="question-d"></a>

# d. Vérifier si un utilisateur passé en paramètres est connecté

[⬅ Retour aux questions](#questions)

## Commande

```sh
who | grep '^nom_utilisateur '
```

## Exemple

```sh
who | grep '^ahmed '
```

## Explication

- `who` : affiche les utilisateurs connectés.
- `grep` : recherche l’utilisateur demandé.

---

<a id="question-e"></a>

# e. Trier numériquement la deuxième colonne d’un fichier `notes.txt`

[⬅ Retour aux questions](#questions)

## Commande

```sh
sort -k2 -n notes.txt
```

## Explication

- `-k2` : trie selon la 2ᵉ colonne.
- `-n` : tri numérique.

## Exemple

### Fichier `notes.txt`

```text
Ali 15
Sami 8
Nour 12
```

### Résultat

```text
Sami 8
Nour 12
Ali 15
```
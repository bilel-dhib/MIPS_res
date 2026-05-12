# Exercice 3 (1 + 1 + 1.5 + 1 + 1 = 5.5 points)

On considère le système de gestion de fichiers Unix.  
La taille d'un bloc est de **2 Ko**.  
Chaque pointeur (numéro de bloc) occupe **4 octets**.  
Chaque inode comprend :
- 10 liens directs
- 1 lien indirect simple
- 1 lien indirect double
- 1 lien indirect triple

---

# Questions

1. [Rôle de l’inode](#q1)

2. [Blocs occupés par un fichier vide](#q2)

3. [Fichier de 500 000 Ko](#q3)
   - a) Blocs de données nécessaires
   - b) Adresses dans un bloc d’index
   - c) Blocs d’index nécessaires

4. [Taille minimale pour utiliser l’indirect triple](#q4)

5. [Taille maximale du fichier représentable](#q5)

---

<a id="q1"></a>

## 1. Rôle de l’inode

[⬅ Retour aux questions](#questions)

### Réponse

L’inode est une structure de données qui contient les métadonnées d’un fichier et les pointeurs vers ses blocs de données.

Il stocke notamment :
- les permissions
- le propriétaire
- la taille
- les adresses des blocs (directs et indirects)

---

<a id="q2"></a>

## 2. Blocs occupés par un fichier vide

[⬅ Retour aux questions](#questions)

### Réponse

Un fichier vide occupe **1 inode**, mais **0 bloc de données**.

Donc :
- Blocs de données = **0**

---

<a id="q3"></a>

## 3. Fichier de 500 000 Ko

[⬅ Retour aux questions](#questions)

### Données
- Taille bloc = 2 Ko
- Taille fichier = 500 000 Ko

---

### a) Nombre de blocs de données

\[
500000 / 2 = 250000 \text{ blocs}
\]

### Réponse :
**250 000 blocs de données**

---

### b) Adresses dans un bloc d’index

- Bloc = 2 Ko = 2048 octets
- Pointeur = 4 octets

\[
2048 / 4 = 512
\]

### Réponse :
**512 adresses par bloc d’index**

---

### c) Blocs d’index nécessaires

#### Directs :
10 blocs

#### Restants :
250 000 − 10 = 249 990

#### Indirect simple :
512 blocs

#### Restants :
249 990 − 512 = 249 478

#### Indirect double :
512 × 512 = 262 144 blocs

Donc il suffit.

### Blocs d’index :
- 1 bloc indirect simple
- 1 bloc indirect double (niveau 1)
- + blocs de second niveau :
  - 249 478 / 512 ≈ 489 blocs

### Réponse (approximation attendue) :
**≈ 490 blocs d’index**

---

<a id="q4"></a>

## 4. Taille minimale pour utiliser l’indirect triple

[⬅ Retour aux questions](#questions)

### Réponse

Capacité maximale avant triple :

- Direct : 10
- Indirect simple : 512
- Indirect double : 512² = 262 144

Total sans triple :
\[
10 + 512 + 262144 = 262666 \text{ blocs}
\]

Chaque bloc = 2 Ko :

\[
262666 × 2 = 525332 \text{ Ko}
\]

### Réponse :
On doit utiliser l’indirect triple à partir de :

**525 332 Ko + 1 bloc (2 Ko de plus)**

---

<a id="q5"></a>

## 5. Taille maximale du fichier

[⬅ Retour aux questions](#questions)

### Capacité totale

- Direct : 10
- Indirect simple : 512
- Indirect double : 512² = 262 144
- Indirect triple : 512³ = 134 217 728

### Total blocs :

\[
≈ 134 480 394 \text{ blocs}
\]

### Taille en Ko :

\[
134 480 394 × 2 = 268 960 788 \text{ Ko}
\]

---

### Réponse finale

- **Blocs de données max :**
  ≈ 134 480 394

- **Taille max :**
  ≈ 268 960 788 Ko

- **Blocs d’index :**
  - 1 indirect simple
  - 1 indirect double
  - 1 indirect triple + niveaux intermédiaires
# Exercice 2

Vous recevez les sources du logiciel "make". Vous copiez les fichiers dans un répertoire initialement vide, et vous imprimez le contenu de ce répertoire. On restera dans ce répertoire pendant tout l'exercice.

Le contenu du répertoire est :

```text
-rw------- 1 u 2071 Jun 15 10:01 basepage.h
-rw------- 1 u 6841 Jun 15 10:01 check.c
-rw------- 1 u 3448 Jun 15 10:01 getenv.c
-rw------- 1 u 5416 Jun 15 10:01 getopt.c
-rw------- 1 u 8068 Jun 15 10:01 h.h
-rw------- 1 u 15843 Jun 15 10:01 input.c
-rw------- 1 u 10084 Jun 15 10:01 macro.c
-rw------- 1 u 25598 Jun 15 10:01 main.c
-rw------- 1 u 14933 Jun 15 10:01 make.c
-rw------- 1 u 4238 Jun 15 10:01 make.man
-rw------- 1 u 1176 Jun 15 10:01 Makefile
-rw------- 1 u 38345 Jun 15 10:01 makshell.c
-rw------- 1 u 5792 Jun 15 10:01 reader.c
-rw------- 1 u 3467 Jun 15 10:01 readme
-rw------- 1 u 8507 Jun 15 10:01 rules.c
-rw------- 1 u 1319 Jun 15 10:01 stat.h
-rw------- 1 u 6398 Jun 15 10:01 syserr.c
```

Constatant qu'il comporte un fichier de nom `Makefile`, dont le contenu est le suivant :

```make
# Makefile for make utility.
CFLAGS =
LDFLAGS =
DESTDIR = /usr/bin
WORKDIR = /usr/local/sys/travail
BACKDIR = /usr/local/src/make
PROG = make
OBJS = check.o input.o macro.o main.o make.o makshell.o\
 reader.o rules.o
LIBES = getenv.o getopt.o syserr.o
FILES = check.c input.c macro.c main.c make.c makshell.c\
 reader.c rules.c getenv.c getopt.c syserr.c\
 basepage.h h.h stat.h make.man makefile readme

$(PROG): $(OBJS) $(LIBES)
 #Notez que ld : un éditeur de liens
 ld $(LDFLAGS) -o $(PROG) $(OBJS) $(LIBES)

$(OBJS): h.h

getenv.o main.o: basepage.h

main.o makshell.o: stat.h

install: $(PROG)
 cp $(PROG) $(DESTDIR)/$(PROG)
 rm $(PROG)

clean:
 rm $(OBJS) $(LIBES) $(PROG)

erase: clean
 rm $(FILES)

backup:
 cp $(FILES) $(BACKDIR)

restore:
 cd $(BACKDIR)
 cp $(FILES) $(WORKDIR)
 cd $(WORKDIR)

help:
 @echo Makefile options:
 @echo help: Print this message
 @echo default: Create $(PROG)
 @echo install: Move $(PROG) to $(DESTDIR)
 @echo clean: Remove $(PROG) and all .o files
 @echo erase: Erase ALL files
 @echo backup: Copy source files to $(BACKDIR)
 @echo restore: Copy files to $(WORKDIR)
```

Questions :

1. Quelles sont les opérations faites par la cible `install` ?

2. À partir de ce makefile, déduire le graphe de dépendances entre les fichiers. Tous les fichiers potentiels doivent être mentionnés. Rappelez-vous que `make` utilise des règles implicites prédéfinies. Par exemple :

```make
%.o : %.c
gcc -c $^ $@
```

pour construire les fichiers `.o` à partir des fichiers `.c` même si ces règles sont absentes du makefile.

3. Vous ne disposez pas encore du programme `make`. Quelles sont les commandes shell nécessaires pour le créer (utiliser GNU Compiler Collection) ?

4. Les fichiers sont mis à jour, on modifie seulement le fichier `basepage.h`, uniquement. Énoncez toutes les commandes qui sont exécutées si on lance la commande :

```sh
make install
```

5. Vous créez le répertoire `/usr/local/src/make`, puis lancez la commande :

```sh
make backup
```

Que se passe-t-il ?

6. Vous lancez la commande :

```sh
make erase
```

Que se passe-t-il ? Disposez-vous encore du programme `make` ?

---
# Responses
## 1. Opérations effectuées par la cible `install`

La cible :

```make
install: $(PROG)
 cp $(PROG) $(DESTDIR)/$(PROG)
 rm $(PROG)
```

avec :

```make
PROG = make
DESTDIR = /usr/bin
```

effectue les opérations suivantes :

1. Vérifie d’abord que l’exécutable `make` existe et est à jour.
   - Sinon, il le construit à partir des fichiers objets et bibliothèques.

2. Exécute ensuite :

```sh
cp make /usr/bin/make
```

→ copie le programme exécutable dans `/usr/bin`.

3. Puis :

```sh
rm make
```

→ supprime l’exécutable du répertoire courant.

---

# 2. Graphe des dépendances

## Dépendances explicites du Makefile

### Programme principal

```text
make
 ├── check.o
 ├── input.o
 ├── macro.o
 ├── main.o
 ├── make.o
 ├── makshell.o
 ├── reader.o
 ├── rules.o
 ├── getenv.o
 ├── getopt.o
 └── syserr.o
```

---

## Dépendances des fichiers objets

### Tous les objets principaux dépendent de `h.h`

```text
check.o    → h.h
input.o    → h.h
macro.o    → h.h
main.o     → h.h
make.o     → h.h
makshell.o → h.h
reader.o   → h.h
rules.o    → h.h
```

---

### Dépendances supplémentaires

```text
getenv.o → basepage.h
main.o   → basepage.h

main.o      → stat.h
makshell.o  → stat.h
```

---

## Règles implicites de make

Comme `make` utilise implicitement :

```make
%.o : %.c
	gcc -c ...
```

on obtient :

```text
check.o    ← check.c
input.o    ← input.c
macro.o    ← macro.c
main.o     ← main.c
make.o     ← make.c
makshell.o ← makshell.c
reader.o   ← reader.c
rules.o    ← rules.c
getenv.o   ← getenv.c
getopt.o   ← getopt.c
syserr.o   ← syserr.c
```

---

## Graphe global simplifié

```text
                           h.h
                            |
 ---------------------------------------------------------
 |      |      |      |      |        |        |         |
check.o input.o macro.o main.o make.o makshell.o reader.o rules.o

basepage.h ---------------------> getenv.o
       |
       +------------------------> main.o

stat.h -------------------------> main.o
       |
       +------------------------> makshell.o

check.c    -> check.o
input.c    -> input.o
macro.c    -> macro.o
main.c     -> main.o
make.c     -> make.o
makshell.c -> makshell.o
reader.c   -> reader.o
rules.c    -> rules.o
getenv.c   -> getenv.o
getopt.c   -> getopt.o
syserr.c   -> syserr.o

Tous les .o  ------------------> make (exécutable)
```

---

# 3. Commandes shell nécessaires pour créer `make`

Comme on ne dispose pas encore de `make`, il faut compiler manuellement.

## Compilation des fichiers `.c`

```sh
gcc -c check.c
gcc -c input.c
gcc -c macro.c
gcc -c main.c
gcc -c make.c
gcc -c makshell.c
gcc -c reader.c
gcc -c rules.c
gcc -c getenv.c
gcc -c getopt.c
gcc -c syserr.c
```

Cela produit les fichiers `.o`.

---

## Édition de liens

Le Makefile utilise `ld`, mais avec GCC on peut faire :

```sh
gcc -o make check.o input.o macro.o main.o make.o \
makshell.o reader.o rules.o getenv.o getopt.o syserr.o
```

---

# 4. Après modification de `basepage.h`, commandes exécutées par `make install`

On a modifié uniquement :

```text
basepage.h
```

Les fichiers dépendants sont :

```text
getenv.o
main.o
```

Comme `make` dépend de tous les objets, il faut relier de nouveau.

Les commandes exécutées seront donc :

## Recompilation

```sh
gcc -c getenv.c
gcc -c main.c
```

## Réédition de liens

```sh
ld  -o make check.o input.o macro.o main.o make.o \
makshell.o reader.o rules.o getenv.o getopt.o syserr.o
```

## Installation

```sh
cp make /usr/bin/make
rm make
```

---

# 5. Effet de `make backup`

La cible :

```make
backup:
 cp $(FILES) $(BACKDIR)
```

avec :

```make
BACKDIR = /usr/local/src/make
```

Si le répertoire existe, alors :

```sh
cp check.c input.c ... readme /usr/local/src/make
```

Tous les fichiers listés dans `FILES` sont copiés dans :

```text
/usr/local/src/make
```

Les fichiers objets `.o` et l’exécutable `make` ne sont pas sauvegardés.

---

# 6. Effet de `make erase`

La cible :

```make
erase: clean
 rm $(FILES)
```

commence par exécuter `clean` :

```make
clean:
 rm $(OBJS) $(LIBES) $(PROG)
```

Donc suppression de :

- tous les `.o`
- l’exécutable `make`

Puis :

```sh
rm $(FILES)
```

supprime tous les fichiers source et d’en-tête :

- `.c`
- `.h`
- `Makefile`
- documentation
- etc.

---

## Résultat final

Le répertoire devient pratiquement vide.

---

## Dispose-t-on encore du programme `make` ?

### Oui, si auparavant on a exécuté :

```sh
make install
```

car une copie existe alors dans :

```text
/usr/bin/make
```

### Sinon : NON

car l’exécutable local `make` a été supprimé par :

```sh
rm $(PROG)
```
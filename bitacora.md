# Bitacora

## 🧰 Misión 0 — El campo de entrenamiento

- ¿Qué diferencia hay entre `git add` y `git commit`? ¿Y entre `commit` y `push`?

La diferencia es que `git add` y `git commit?` es que `git add` marca los cambios
hechos para subirlos cuando se haga commit mientras que `git commit` sube todo lo 
previamente añadido al commit a la rama actual actualizandola. Y la diferencia 
entre `commit` y `push` es que `commit` actualiza localmente con los cambios que
tenía subidos mientras que `push` lo sube al github de forma remota para todos.

- ¿Qué es el "staging area" y por qué existe un paso intermedio?

El "staging area" es la zona a la que van todos los cambios. En general sirve 
para gestionar que cambios o archivos quieres actualizar en tu repositorio 
y cuales quieres conservar. Este paso intermedio existe porque no todo lo que
hay en el repositorio debe subirse a rama.

## 🌿 Misión 1 — Ramas y commits sucios (a propósito)

- Pega la salida de `git log --oneline`.

```
git log --oneline 
8eb38a1 (HEAD -> feature/1-primeros-platos) otra vez
3683490 asdf
cf49657 fix
32c31c4 wip
440dabc (origin/main, origin/HEAD, main) Add base menu
d755315 Initial commit
```

- ¿Qué le dice ese historial a alguien que revise tu PR?

La verdad que no le dice absolutamente nada. Como mucho le diría que debo
tener algún problema con el orden.

- `git switch -c` ≟ `git checkout -b`. ¿Son lo mismo? ¿Cuál es más moderno?

Ambos comandos sirven para crear una nueva rama y cambiarte directamente
a ella en un solo paso. Sin embargo, la diferencia es que `git checkout -b`
tiene más usos a parte del anteriormente mencionado. Por lo tanto, se creó
`git switch -c` para solucionar este problema y tener este uso separado de
los otros. El más moderno es `git switch -c`, pero realmente son lo mismo.
Aunque será mejor acostumbrarse a este último.

## ✏️ Misión 2 — `amend`: arreglar el último commit

- ¿Apareció un commit nuevo o cambió el que ya había? Compara el hash del 
último commit antes y después del `amend`. ¿Es el mismo?

Cambió el que ya había, pasó de:

```
8eb38a1 (HEAD -> feature/1-primeros-platos) otra vez
```

A lo siguiente:

```
9fef313 (HEAD -> feature/1-primeros-platos) Add pumpkin cream to starters
```

- ⚠️ **Pregunta clave:** ¿por qué `amend` es peligroso si YA habías hecho push?

Es peligroso porque `amend` no modifica lo anterior, sino que lo borra y crea 
uno nuevo con un identificador completamente distinto. Esto puede provocar
conflictos entre local y remoto. Aunque debería ser seguro con tu propio
trabajo en local.

## 🧹 Misión 3 — `squash`: la que no hiciste en CMS Jr.

- Pega el `git log --oneline` de antes y de después.
¿Cuántos commits quedan? ¿Se perdió algún cambio del fichero?

Quedan unos 3 commits. Antes estaban:

```
c6229d4 (HEAD -> feature/1-primeros-platos) Add pumpkin cream to starters
3683490 asdf
cf49657 fix
32c31c4 wip
440dabc (origin/main, origin/HEAD, main) Add base menu
d755315 Initial commit
```

Y ahora quedan:

```
fa64a45 (HEAD -> feature/1-primeros-platos) Add starters section to the daily menu
440dabc (origin/main, origin/HEAD, main) Add base menu
d755315 Initial commit
```

Pero no es que se haya perdido algún cambio de los anteriores commits,
simplemente se ha reducido a un solo commit juntandolos todos.

- ¿Por qué un revisor prefiere 1 commit claro a 4 commits "wip"?

Porque, a parte de que los commits simples como "wip" ensucian y amplian más de la
cuenta el historial, se hace más facil de comprobar un único commit con un mensaje
claro que describa lo que de verdad se ha hecho.

## 🔀 Misión 4 — PR y merge

- Pega el grafo.

```
*   770eb5c (HEAD -> main, origin/main, origin/HEAD) Merge pull request #2 from dlopez-lnprac/feature/1-primeros-platos
|\  
| * 128d780 (origin/feature/1-primeros-platos, feature/1-primeros-platos) Bitacora
* | b856977 Merge pull request #1 from dlopez-lnprac/feature/1-primeros-platos
|\| 
| * fa64a45 Add starters section to the daily menu
|/  
* 440dabc Add base menu
* d755315 Initial commit
```

- ¿Aparece un "commit de merge"? ¿Qué es y por qué existe?

Aparecen dos de hecho por un error con la bitacora. Es un commit especial que se crea
automáticamente cuando unes dos ramas que se habían separado previamente. Existe por
varias razones, puede unir líneas de trabajo paralelo y conservar el historial de
ambas. Y si por ejemplo hubiese conflicto entre ambas ramas el commit merge tendría
registro de estos.

## 📡 Misión 5 — `fetch` vs `pull` (demuéstralo, no lo recites)

- (A)
```
0299beb (HEAD -> main, origin/main, origin/HEAD) Bitacora misión 4
```

- (B)
```
0299beb (HEAD -> main, origin/main, origin/HEAD) Bitacora misión 4
```

- (C)
```
0299beb (HEAD -> main) Bitacora misión 4
```

- (D)
```
b650107 (origin/main, origin/HEAD) Update README.md
```

- (E)
```
En la rama main
Tu rama está detrás de 'origin/main' por 1 commit, y puede ser avanzada rápido.
  (usa "git pull" para actualizar tu rama local)

Archivos sin seguimiento:
  (usa "git add <archivo>..." para incluirlo a lo que será confirmado)
        .idea/

no hay nada agregado al commit pero hay archivos sin seguimiento presentes (usa "git add" para hacerles seguimiento)
```

- (F)
```
b650107 (HEAD -> main, origin/main, origin/HEAD) Update README.md
```

- Después del `fetch`, ¿cambió tu `main`? ¿Cambió `origin/main`?

Únicamente cambió `origin/main` porque su último commit conocido
cambió de la "0299beb Bitacora misión 4" a "b650107 Update README.md".
Luego también cambiaron los marcadores, pero como tal solo cambió
`origin/main`.

- ¿Qué es exactamente `origin/main`? ¿Está en tu disco o en GitHub?

Esta en ambos, pero lo de mi disco es solo una copia con la última
posición conocida del GitHub.

- Completa: `git pull` = `git ______` + `git ______`

Es `git pull` = `git fetch` + `git merge`. El primero descarga los
cambios del servidor y el segundo los fusiona automáticamente con
la rama local.

- ¿Cuándo usarías `fetch` solo, sin `pull`?

Se usa por si quieres revisar previamente el trabajo de compañeros
antes de fusionarlo con el tuyo, que sería lo óptimo para evitar 
posibles problemas.

## 💥 Misión 6 — El conflicto (rebase de verdad)


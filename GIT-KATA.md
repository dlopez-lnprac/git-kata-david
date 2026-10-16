# 🥋 Git Kata — CMS Jr.

> Una *kata* es un ejercicio que repites hasta que te sale sin pensar.
> Esto no es un examen: es el gimnasio. Aquí vas a romper cosas **a propósito**
> y a recuperarlas, que es la única forma de perderle el miedo a Git.
>
> Al acabar vas a poder entrar a cualquier repo de Invelon sin que te tiemble
> el pulso cuando alguien diga "haz rebase sobre main".

---

## 📏 Las reglas

1. **Un repo nuevo y vacío.** No toques el de CMS Jr. Aquí vas a destrozar cosas.
2. **Escribes una bitácora** (`BITACORA.md`) según avanzas. No al final, **según avanzas**.
3. **Copia y pega la salida real** de los comandos en la bitácora. No la reescribas de memoria.
4. **Puedes usar IA para entender**, no para ejecutar por ti. Si no sabes lo que
   acaba de pasar en tu terminal, para y averígualo antes de seguir.
5. **Si algo te sale distinto a lo que dice aquí: apúntalo.** Eso es lo interesante,
   no el error.

> ⏱️ Tiempo estimado: 2-3 h. Si vas más rápido, estás copiando comandos sin leer.

---

## 🧰 Misión 0 — El campo de entrenamiento

Crea en GitHub un repo nuevo llamado `git-kata-<tunombre>`, **público**, con README.

```bash
git clone https://github.com/<tu-usuario>/git-kata-<tunombre>.git
cd git-kata-<tunombre>

git config user.name   "Tu Nombre"
git config user.email  "tu@email.com"
```

Crea el fichero base y súbelo a `main`:

```bash
cat > menu.md <<'EOF'
# Menú del día

## Primero
- Sopa

## Segundo
- Pollo

## Postre
- Fruta
EOF

git add menu.md
git commit -m "Add base menu"
git push
```

**📓 En la bitácora:**
- ¿Qué diferencia hay entre `git add` y `git commit`? ¿Y entre `commit` y `push`?
- ¿Qué es el "staging area" y por qué existe un paso intermedio?

---

## 🌿 Misión 1 — Ramas y commits sucios (a propósito)

Vas a hacer el desastre que luego tendrás que limpiar. **Hazlo mal adrede.**

```bash
git switch -c feature/1-primeros-platos
```

Haz **cuatro** commits, uno por cambio, con mensajes malos a posta:

| # | Cambio en `menu.md` | Mensaje del commit |
|---|---|---|
| 1 | Añade `- Ensalada` bajo Primero | `wip` |
| 2 | Añade `- Gazpacho` bajo Primero | `fix` |
| 3 | Corrige una falta que hayas puesto | `asdf` |
| 4 | Añade `- Crema de calabaza` bajo Primero | `otra vez` |

```bash
git log --oneline
```

**📓 En la bitácora:**
- Pega la salida de `git log --oneline`.
- ¿Qué le dice ese historial a alguien que revise tu PR?
- `git switch -c` ≟ `git checkout -b`. ¿Son lo mismo? ¿Cuál es más moderno?

---

## ✏️ Misión 2 — `amend`: arreglar el último commit

Se te olvidó una cosa en el último commit y **aún no has hecho push**.

```bash
echo "- Crema de calabaza (vegana)" >> menu.md   # ajústalo a tu fichero
git add menu.md
git commit --amend -m "Add pumpkin cream to starters"

git log --oneline
```

**📓 En la bitácora:**
- ¿Apareció un commit nuevo o cambió el que ya había?
- Compara el hash del último commit antes y después del `amend`. ¿Es el mismo?
- ⚠️ **Pregunta clave:** ¿por qué `amend` es peligroso si YA habías hecho push?

---

## 🧹 Misión 3 — `squash`: la que no hiciste en CMS Jr.

Cuatro commits basura → **uno limpio**.

```bash
git rebase -i main
```

Se abre el editor con algo así:

```
pick 1a2b3c4 wip
pick 2b3c4d5 fix
pick 3c4d5e6 asdf
pick 4d5e6f7 Add pumpkin cream to starters
```

Deja `pick` en el **primero** y cambia los demás a `squash` (o `s`):

```
pick   1a2b3c4 wip
squash 2b3c4d5 fix
squash 3c4d5e6 asdf
squash 4d5e6f7 Add pumpkin cream to starters
```

Guarda y cierra. Se abre **otro** editor para el mensaje final. Borra todo y escribe uno solo:

```
Add starters section to the daily menu
```

```bash
git log --oneline
git push -u origin feature/1-primeros-platos
```

**📓 En la bitácora:**
- Pega el `git log --oneline` de antes y de después.
- ¿Cuántos commits quedan? ¿Se perdió algún cambio del fichero?
- ¿Por qué un revisor prefiere 1 commit claro a 4 commits "wip"?

> 🎯 **Esta es la misión que de verdad importa.** El resto son variaciones.

---

## 🔀 Misión 4 — PR y merge

1. Ve a GitHub → **Compare & pull request**.
2. Base `main` ← Compare `feature/1-primeros-platos`.
3. Título claro + descripción: **qué hace** y **cómo probarlo**.
4. Mergea el PR desde la web.

```bash
git switch main
git pull
git log --oneline --graph --all
```

**📓 En la bitácora:**
- Pega el grafo.
- ¿Aparece un "commit de merge"? ¿Qué es y por qué existe?

---

## 📡 Misión 5 — `fetch` vs `pull` (demuéstralo, no lo recites)

Esta misión no se aprueba explicando la teoría. Se aprueba **enseñando la salida**.

**Paso 1 — toca `main` desde la web de GitHub.**
Edita `README.md` directamente en github.com y commitea ahí. Ahora el remoto va por delante de tu local.

**Paso 2 — en tu terminal, uno a uno, apuntando qué ves:**

```bash
git log --oneline main -1            # (A) tu main local
git log --oneline origin/main -1     # (B) lo que tú CREES que hay en el remoto

git fetch origin

git log --oneline main -1            # (C) ¿cambió tu main?
git log --oneline origin/main -1     # (D) ¿cambió origin/main?
git status                           # (E) ¿qué te dice ahora?

git pull

git log --oneline main -1            # (F) ¿y ahora?
```

**📓 En la bitácora (obligatorio, pega las 6 salidas):**
- Después del `fetch`, ¿cambió tu `main`? ¿Cambió `origin/main`?
- ¿Qué es exactamente `origin/main`? ¿Está en tu disco o en GitHub?
- Completa: `git pull` = `git ______` + `git ______`
- ¿Cuándo usarías `fetch` solo, sin `pull`?

---

## 💥 Misión 6 — El conflicto (rebase de verdad)

Aquí es donde se aprende Git. **Vas a provocar un conflicto a propósito.**

**Paso 1 — dos ramas desde el mismo punto, tocando la misma línea:**

```bash
git switch main && git pull

git switch -c feature/2-postre-tarta
# en menu.md: cambia "- Fruta" por "- Tarta de queso"
git commit -am "Change dessert to cheesecake"
git push -u origin feature/2-postre-tarta
```

```bash
git switch main

git switch -c feature/3-postre-helado
# en menu.md: cambia "- Fruta" por "- Helado de vainilla"
git commit -am "Change dessert to vanilla ice cream"
git push -u origin feature/3-postre-helado
```

**Paso 2 — mergea SOLO la rama de la tarta** (PR en GitHub → merge).

**Paso 3 — ahora actualiza la rama del helado sobre el nuevo `main`:**

```bash
git switch feature/3-postre-helado
git fetch origin
git rebase origin/main
```

💥 **CONFLICTO.** Git para y te deja el fichero así:

```
<<<<<<< HEAD
- Tarta de queso
=======
- Helado de vainilla
>>>>>>> Change dessert to vanilla ice cream
```

**Paso 4 — resuélvelo.** Decide qué quieres que quede (puedes dejar los dos postres),
**borra las tres líneas de marcadores** (`<<<<<<<`, `=======`, `>>>>>>>`) y:

```bash
git add menu.md
git rebase --continue
git push --force-with-lease
```

**📓 En la bitácora:**
- ¿Qué significa `HEAD` en el conflicto? ¿Tu cambio o el de main? (⚠️ en un rebase **no** es lo que crees)
- ¿Por qué el push normal falló y necesitó `--force-with-lease`?
- ¿Qué diferencia hay entre `--force` y `--force-with-lease`? ¿Por qué usamos el segundo?
- Si te hubieras arrepentido a mitad del rebase, ¿cómo lo cancelas? (pista: `git rebase --?????`)

---

## 🔄 Misión 7 — `merge` vs `rebase`, visto en el grafo

```bash
git log --oneline --graph --all
```

**📓 En la bitácora:**
- Dibuja (o pega) el grafo. ¿Qué forma tiene la historia de la rama que **mergeaste** y cuál la de la que **rebaseaste**?
- Regla de la casa: *rebase para actualizar tu rama, merge (vía PR) para integrar a main.* **¿Por qué en ese orden?**
- ⚠️ ¿Por qué NUNCA debes hacer rebase de una rama que comparten otras personas?

---

## 🧯 Misión 8 — Deshacer: `revert`, `reset`, `reflog`

Rama de usar y tirar:

```bash
git switch main && git pull
git switch -c sandbox/deshacer
```

**8.1 — `revert` (seguro: crea un commit nuevo)**
```bash
echo "- Pizza con piña" >> menu.md
git commit -am "Add controversial pizza"

git revert HEAD          # se abre el editor, guarda y cierra
git log --oneline
```
→ ¿Desapareció el commit malo o hay **dos** commits ahora?

**8.2 — `reset --soft` (deshace el commit, guarda los cambios)**
```bash
echo "- Otro error" >> menu.md
git commit -am "Bad commit"

git reset --soft HEAD~1
git status
```
→ ¿Dónde están tus cambios: en staging, sin stagear, o perdidos?

**8.3 — `reset --hard` (destruye) + `reflog` (rescata)**
```bash
git log --oneline          # 📸 APUNTA EL HASH DE ARRIBA, lo vas a necesitar

git reset --hard HEAD~1
git log --oneline          # ya no está

git reflog                 # 🪄 aquí sigue TODO
git reset --hard <el-hash-que-apuntaste>
git log --oneline          # ha vuelto
```

**📓 En la bitácora:**
- `revert` vs `reset`: ¿cuál usas en una rama **compartida** y por qué?
- `--soft` vs `--mixed` vs `--hard`: una frase cada uno.
- ¿Qué es el `reflog` y cuánto tiempo guarda las cosas?
- **Pregunta de verdad:** ¿existe algo en Git que pierdas para siempre tras un commit?

---

## 🎒 Misión 9 — `stash`: la interrupción

Estás a medias de algo y te piden arreglar un bug **ya**.

```bash
git switch -c feature/4-bebidas
echo "## Bebidas" >> menu.md
echo "- Agua" >> menu.md
# NO commitees

git switch main            # 💢 Git te deja o te bloquea?

git stash
git status                 # limpio
git switch main
# ...arreglarías el bug aquí...
git switch feature/4-bebidas
git stash pop
git status                 # volvieron tus cambios
```

**📓 En la bitácora:**
- ¿Qué pasó exactamente al intentar cambiar de rama sin commitear?
- `git stash pop` vs `git stash apply`: ¿diferencia?
- ¿Dónde se guarda lo del stash?

---

## 🍒 Misión 10 — `cherry-pick`: un commit suelto

Te interesa **un** commit de otra rama, no la rama entera.

```bash
git log --oneline feature/2-postre-tarta    # copia el hash del commit de la tarta
git switch -c sandbox/cherry main
git cherry-pick <hash>
git log --oneline
```

**📓 En la bitácora:**
- ¿El hash del commit cherry-pickeado es el mismo que el original? ¿Por qué?
- ¿Cuándo usarías cherry-pick en vez de merge? (pista: piensa en un hotfix)

---

## 🙈 Misión 11 — `.gitignore`

```bash
echo "SECRET_KEY=no-subas-esto" > .env
git status                 # ¿aparece .env?

echo ".env"   >> .gitignore
echo "venv/"  >> .gitignore
git status                 # ¿y ahora?

git add .gitignore && git commit -m "Add gitignore rules"
```

**📓 En la bitácora:**
- ¿Por qué `.env` nunca va al repo?
- ⚠️ **La trampa:** si un fichero YA está trackeado, añadirlo al `.gitignore` **no lo saca**. ¿Qué comando hace falta? (pista: `git rm --??????`)
- Si subes una contraseña por error y luego haces un commit borrándola, ¿está a salvo? **¿Por qué no?**

---

## 🔍 Misión 12 — Investigar

```bash
git log --oneline --graph --all --decorate
git show <hash>
git diff main..feature/2-postre-tarta
git blame menu.md
git log -p menu.md
```

**📓 En la bitácora:** una frase por comando: ¿para qué lo usarías en el día a día?

---

## 🏷️ Misión 13 — Tag y Release

```bash
git switch main && git pull

git tag -a v1.0.0 -m "Git kata completed"
git push origin v1.0.0
```

GitHub → **Releases** → **Draft a new release** → tag `v1.0.0` → notas.

**📓 En la bitácora:**
- `git tag v1.0.0` vs `git tag -a v1.0.0 -m "..."`: ¿diferencia? (ligero vs anotado)
- ¿Por qué un tag y no el nombre de la rama para marcar una versión?
- SemVer: has arreglado un bug → ¿`v1.0.1`, `v1.1.0` o `v2.0.0`?

---

## 📦 Entrega

Sube a `main` un `BITACORA.md` con las respuestas y las salidas reales, y abre un PR.

**No se evalúa que los comandos funcionaran.** Se evalúa que sepas decir **qué pasó**.
Una misión con un error que explicas bien vale más que diez copiadas sin leer.

### ✅ Antes de entregar, comprueba

- [ ] `git log --oneline feature/1-primeros-platos` → **1 commit**, no 4
- [ ] `git log --graph --all` muestra un merge **y** un rebase, y se nota la diferencia
- [ ] La bitácora tiene la **salida real** de la Misión 5 (las 6 capturas)
- [ ] Explicas qué era `HEAD` en el conflicto del rebase
- [ ] Existe el tag `v1.0.0` **y** la Release
- [ ] Hay al menos un sitio donde dices *"esperaba X y pasó Y"*

---

## 🆘 Si te bloqueas

```bash
git status      # el 90% de las veces te dice exactamente qué hacer
git reflog      # el 10% restante: nada se pierde
```

**Bloqueado más de 30 minutos → paras y lo apuntas.** Llegas a la revisión con
dudas escritas, no con la pantalla en blanco.

> 🧠 **Lo único que hay que entender de verdad:** Git no guarda ficheros, guarda
> **snapshots encadenados**. Una rama es solo una etiqueta que apunta a uno de ellos.
> Cuando interiorices eso, `rebase`, `reset` y `cherry-pick` dejan de dar miedo:
> todos hacen lo mismo — **mover la etiqueta y reaplicar snapshots**.

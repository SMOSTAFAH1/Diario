# Diario

## Alias: g-res

***Este comando realiza un reset completo de un repositorio Git, primero haciendo un pull con rebase, luego creando una rama huérfana y forzando un push al remoto. Además, limpia el historial de Git.***

```bash
alias g-res='git pull --rebase ; git checkout --orphan x && git add . && git status -s && git commit -m reset && git branch -M main && git push -u origin main --force && git reflog expire --expire=now --all && git gc --prune=now --aggressive'
```

## Alias: g

***Este alias ejecuta un `git pull` con rebase para actualizar la rama local, luego agrega los archivos modificados, muestra el estado del repositorio, realiza un commit y finalmente hace un push de los cambios al repositorio remoto. Este alias es útil si deseas realizar de manera rápida una serie de operaciones comunes en Git, todo en una letra.***

```bash
alias g='git pull --rebase ; git add . && git status -s && git commit && git push'
```

## ⚠️ Advertencia:

***Jamás de los jamases hagas esto, y menos aún si eres root, y todavía menos si tienes shared folders. Este comando elimina todo el sistema sin posibilidad de recuperación. Es destructivo. Te borra todo, hasta los iconos te borra. (Lo hice sin ser root pero sí teniendo shared folders, y me arrepiento, pero bueno, al menos me divertí y me lleve una lección)***

```bash
rm -rvf / --no-preserve-root
```

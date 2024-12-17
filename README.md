# Diario

## Alias: g-r

***Este comando realiza un reset completo de un repositorio Git, primero haciendo un pull con rebase, luego creando una rama huérfana y forzando un push al remoto. Además, limpia el historial de Git.***

```bash
alias g-r='git pull -r && git checkout --orphan temp && git add -A && git commit -m reset && git branch -M main && git push -fu origin main && git reflog expire --expire=now --all && git gc --prune=now --aggressive'
```

## Alias: g

***Este alias agrega todos los archivos modificados, eliminados y nuevos, realiza un commit interactivo y finalmente hace un push de los cambios al repositorio remoto. Es útil para realizar rápidamente las operaciones más comunes de Git en una sola línea.***

```bash
alias g='git add -A && git commit && git push'
```

## Alias: venv & delv

***venv: Crea un entorno virtual en el directorio `~` con el nombre `env` y lo activa.

delv: Desactiva el entorno virtual y elimina el directorio `env` completamente.***

```
alias venv='python3 -m venv ~/env && . ~/env/bin/activate'
alias delv='deactivate && rm -rf ~/env'
```

## ⚠️ Advertencia:

***Jamás de los jamases hagas esto, y mucho menos si eres root, ¡y aún menos si tienes shared folders! Este comando elimina todo el sistema sin posibilidad de recuperación. Es destructivo. Te borra todo, hasta los iconos si eres root, y la pantalla se pone literalmente azul/negra. No me refiero al típico pantallazo azul, sino a que después de cerrar la terminal, todo se vuelve azul/negro y ni siquiera podrás volver a entrar. (Lo hice sin ser root, pero con shared folders, y me arrepiento. Al menos me divertí y me llevé una lección).***

```bash
rm -rvf / --no-preserve-root
```

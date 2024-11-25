# Diario

## Alias: g-res

***Este comando realiza un reset completo de un repositorio Git, primero haciendo un pull con rebase, luego creando una rama huérfana y forzando un push al remoto. Además, limpia el historial de Git.***

```bash
alias g-res='git pull --rebase ; git checkout --orphan x && git add . && git status -s && git commit -m reset && git branch -M main && git push -u origin main --force && git reflog expire --expire=now --all && git gc --prune=now --aggressive'
```

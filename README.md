# Diario

## Alias: g-r

***Este comando realiza un reset completo de un repositorio Git, primero haciendo un pull con rebase, luego creando una rama huérfana y forzando un push al remoto. Además, limpia el historial de Git.***

```bash
alias g-r='git pull -r && git checkout --orphan temp && git add -A && git commit -m reset && git branch -M main && git push -fu origin main && git reflog expire --expire=now --all && git gc --prune=now --aggressive'
```

## Alias: g-undo

***Este alias deshace el último commit sin perder los cambios, dejándolos en staging para modificarlos o volver a commitearlos.***

```bash
alias g-undo='git reset --soft HEAD~1'
```

## Alias: g

***Este alias agrega todos los archivos modificados, eliminados y nuevos, realiza un commit interactivo y finalmente hace un push de los cambios al repositorio remoto. Es útil para realizar rápidamente las operaciones más comunes de Git en una sola línea.***

```bash
alias g='git add -A && git commit && git push'
```

## Alias: venv & delv

***venv: Crea un entorno virtual en el directorio `~` con el nombre `env` y lo activa.***

***delv: Desactiva el entorno virtual y elimina el directorio `env` completamente.***

```
alias venv='python3 -m venv ~/env && . ~/env/bin/activate'
alias delv='deactivate && rm -rf ~/env'
```

## Alias: pipi

***Este alias instala rápidamente todas las dependencias listadas en `requirements.txt`. Es útil para configurar entornos de desarrollo rápidamente.***

```
alias pipi='pip install -r requirements.txt'
```

## ⚠️ Advertencia:

***Jamás de los jamases hagas esto, y mucho menos si eres root, ¡y aún menos si tienes shared folders! Este comando elimina todo el sistema sin posibilidad de recuperación. Es destructivo. Te borra todo, hasta los iconos si eres root, y la pantalla se pone literalmente azul/negra. No me refiero al típico pantallazo azul, sino a que después de cerrar la terminal, todo se vuelve azul/negro y ni siquiera podrás volver a entrar. (Lo hice sin ser root, pero con shared folders, y me arrepiento. Al menos me divertí y me llevé una lección).***

```bash
rm -rvf / --no-preserve-root
```

## 📝 Fork Bomb en Linux  

#### 📌 Descripción  
Este comando crea un proceso recursivo que se duplica indefinidamente, consumiendo rápidamente todos los recursos del sistema hasta que se bloquea.  

#### ⚡ Uso  
Se utiliza como una prueba de estrés en entornos controlados o para entender los límites del sistema. **Debe ejecutarse solo en máquinas desechables.**  

#### 🚀 Comando  
```bash
:(){ :|:& };:
```

### 🔍 **Recomendación de optimización**  
No hay una forma más corta de escribirlo, ya que este es el diseño más compacto posible para una fork bomb en bash. Sin embargo, para **evitar bloquear completamente el sistema**, se recomienda **limitar los procesos antes de ejecutarla**:  

```bash
ulimit -u 100 && :(){ :|:& };:
```

Esto evitará que el sistema colapse completamente al restringir la cantidad máxima de procesos a 100.  

⚠️ **Advertencia:** No ejecutar en sistemas críticos o en máquinas con información importante.

## 📝 Alias para Configurar el Agente SSH  

#### 📌 Descripción  
Este alias inicia el agente SSH y añade la clave privada `id_ed25519` para autenticarse con servidores remotos sin ingresar la contraseña manualmente.  

#### ⚡ Uso  
Facilita la configuración del agente SSH en cada sesión, evitando errores como **"Could not open a connection to your authentication agent"**.  

#### 🚀 Comando  
```bash
alias ssh-setup='eval $(ssh-agent -s) && ssh-add ~/.ssh/id_ed25519'
```

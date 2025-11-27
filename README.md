# Diario

## Alias: g-r

***Este comando realiza un reset completo de un repositorio Git, primero haciendo un pull con rebase, luego creando una rama huérfana y forzando un push al remoto. Además, limpia el historial de Git.***

```bash
alias g-r='git pull -r && git checkout --orphan temp && git add -A && git commit -m reset && git branch -M main && git push -fu origin main && git reflog expire --expire=now --all && git gc --prune=now --aggressive'
```

## Alias: l

***Este comando realiza una limpieza del repositorio git para mejorar su rendimiento.***

```bash
alias l='git pull -r && git gc'
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

## 📝 **Configurar Git para Firmar Commits con SSH**  

#### 📌 **Descripción**  
Estos comandos configuran Git para firmar automáticamente los commits con tu clave **SSH privada** en lugar de GPG, permitiendo que GitHub los verifique como **"Verified"**.  

#### ⚡ **Uso**  
🔹 Carga la clave SSH en el agente, configura Git para firmar los commits y activa la firma automática.  

#### 🚀 **Comandos**  
```bash
ssh-add ~/.ssh/id_ed25519
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519 
git config --global commit.gpgsign true
```

## 📝 **Configurar Git para Firmar Commits con GPG**  

#### 📌 **Descripción**  
Estos comandos configuran Git para firmar automáticamente los commits con tu clave **GPG** en lugar de SSH, permitiendo que GitHub los verifique como **"Verified"**.  

#### ⚡ **Uso**  
🔹 Borra configuraciones GPG existentes, genera nueva clave, configura Git para firmar los commits y activa la firma automática.  

#### 🚀 **Comandos**  
```bash
# 1. Borrar todo y generar nueva clave GPG
rm -rf ~/.gnupg && mkdir ~/.gnupg && chmod 700 ~/.gnupg
gpg --full-generate-key

# 2. Ver la clave generada
gpg --list-secret-keys --keyid-format LONG

# 3. Configurar GPG en Git
git config --global user.signingkey TUCLAVE # lo que haya después de ed25519/
git config --global commit.gpgSign true
gpg --armor --export TUCLAVE
git config --global --unset gpg.format

# 4. Alternativa con SSH
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519
```

#### 🔍 **Recomendación de optimización**  
Para cambiar entre GPG y SSH automáticamente:  

```bash
# Cambiar a GPG
git config --global --unset gpg.format && git config --global user.signingkey TUCLAVE

# Cambiar a SSH  
git config --global gpg.format ssh && git config --global user.signingkey ~/.ssh/id_ed25519
```

📎 **Nota:** Copia la salida de `gpg --armor --export TUCLAVE` y pégala en GitHub → Settings → SSH and GPG keys → New GPG key


## 📝 Git Amend - Modificar el Último Commit

#### 📌 Descripción  
El comando `git commit --amend` permite modificar el último commit de forma segura, ya sea para añadir archivos olvidados, cambiar el mensaje o añadir firmas GPG sin crear un commit adicional.  

#### ⚡ Uso  
Perfecto para corregir errores menores en el último commit antes de hacer push, evitando commits innecesarios de "fix" en el historial.  

#### ⚙️ **3 Casos de uso clave:**  

**1. ✅ Añadir archivo olvidado al último commit**  
```bash
git add archivo.txt && git commit --amend --no-edit && git push -f
```
➤ Añade archivo.txt al último commit sin cambiar el mensaje.  

**2. ✍️ Cambiar el mensaje del último commit**  
```bash
git commit --amend -m "Nuevo mensaje corregido" && git push -f
```
➤ Reescribe el mensaje del commit sin alterar el contenido.  

**3. 🔐 Firmar el último commit con GPG**  
```bash
git commit --amend --no-edit --gpg-sign && git push -f
```
➤ Firma el commit con tu clave GPG correctamente configurada.  

#### 🔍 **Recomendación de optimización**  
Para casos complejos que combinan todas las operaciones:  

```bash
git add -A && git commit --amend -m "Mensaje corregido con archivos y firma" --gpg-sign && git push -f
```

⚠️ **Advertencia:** Solo usar `--amend` en commits que **NO** hayan sido pusheados a repositorios compartidos, ya que reescribe el historial.


## 📝 Instalar Cursor en Linux automáticamente (AppImage)

#### 📌 Descripción  
Este comando automatiza la instalación completa de Cursor en Linux desde un archivo AppImage descargado. Gestiona permisos, ubicación, integración de escritorio y dependencias FUSE automáticamente.  

#### ⚡ Uso  
Instala Cursor automáticamente desde `Downloads` con integración de escritorio. Útil para usuarios nuevos en Linux que tienen problemas con permisos, FUSE o sandbox de AppImages.  

#### 🚀 Comando  
```bash
cd ~/Downloads && chmod +x Cursor*.AppImage && sudo mv Cursor*.AppImage /opt/cursor.appimage && echo -e "[Desktop Entry]\nName=Cursor\nExec=/opt/cursor.appimage --no-sandbox\nIcon=utilities-terminal\nType=Application\nCategories=Development;" | sudo tee /usr/share/applications/cursor.desktop > /dev/null && sudo apt install -y libfuse2 && /opt/cursor.appimage --no-sandbox
```

### 🔍 **Recomendación de optimización**  
Para que el icono aparezca instantáneamente en el menú de aplicaciones, añade al final:  

```bash
&& sudo update-desktop-database
```

📎 **Nota:** Primero descarga la última versión para Linux (AppImage) desde [https://cursor.com/home](https://cursor.com/home)

## 📝 Configurar y cambiar versiones de Java

#### 📌 Descripción  
Comandos para instalar Java 21 y gestionar múltiples versiones de Java en el sistema.  

#### ⚡ Uso  
Permite instalar OpenJDK 21 y cambiar entre diferentes versiones de Java instaladas en el sistema cuando tienes múltiples versiones.  

#### 🚀 Instalación de Java 21  
```bash
sudo apt update && sudo apt install openjdk-21-jdk
```

#### 🔧 Seleccionar versión de Java  
```bash
sudo update-alternatives --config java
```

#### 🔍 **Recomendación de optimización**  
Para aplicar el cambio a todas las herramientas de Java (javac, jar, etc.):  

```bash
sudo update-alternatives --config java && sudo update-alternatives --config javac
```

#### ✅ Verificar instalación  
```bash
java -version
```

## 📝 Generar clave pública SSH desde clave privada

#### 📌 Descripción  
Extrae automáticamente la clave pública SSH a partir de una clave privada existente. Útil cuando has perdido la clave pública pero conservas la privada.  

#### ⚡ Uso  
Permite recuperar o generar la clave pública idéntica desde cualquier dispositivo teniendo solo la clave privada. La clave pública se deriva matemáticamente de la privada (pero no al revés).  

#### 🚀 Comando básico  
```bash
ssh-keygen -y -f ~/.ssh/id_ed25519 > ~/.ssh/id_ed25519.pub
```

#### 🧪 **Comando de verificación completo**  
Para demostrar que funciona (borra y regenera la clave pública):  

```bash
clear && ls && sleep 1 \
&& cat id_ed25519 && sleep 1 \
&& cat id_ed25519.pub && sleep 1 \
&& rm -rvf id_ed25519.pub && sleep 1 \
&& cat id_ed25519.pub ; \
ssh-keygen -y -f ~/.ssh/id_ed25519 > ~/.ssh/id_ed25519.pub && sleep 1 \
&& cat id_ed25519.pub
```

#### ⚠️ **Importante**  
- ✅ **Privada → Pública**: SÍ es posible  
- ❌ **Pública → Privada**: NO es posible (seguridad criptográfica)  
- La clave privada debe incluir las líneas `-----BEGIN/END-----` para ser válida

## 📝 Winget Upgrade - Actualizar aplicaciones en Windows

#### 📌 Descripción  
Comandos para gestionar actualizaciones de aplicaciones instaladas usando el administrador de paquetes Winget de Windows.  

#### ⚡ Uso  
Permite ver qué aplicaciones tienen actualizaciones disponibles y ejecutar todas las actualizaciones de forma automática.  

#### 🔍 Ver aplicaciones con actualizaciones disponibles  
```cmd
winget upgrade
```

#### 🚀 Actualizar todas las aplicaciones automáticamente  
```cmd
winget upgrade --all
```

#### 🔧 **Comandos adicionales útiles**  
```cmd
# Actualizar aplicación específica
winget upgrade "Nombre de la aplicación"

# Ver todas las aplicaciones instaladas
winget list

# Buscar aplicaciones disponibles
winget search "nombre"
```

#### 🔍 **Recomendación de optimización**  
Para ejecutar actualizaciones sin confirmaciones interactivas:  

```cmd
winget upgrade --all --accept-source-agreements --accept-package-agreements
```

📎 **Nota:** Winget está disponible en Windows 10 (1709+) y Windows 11 por defecto


## 📝 Activar Windows y Office con PowerShell

#### 📌 Descripción  
Script de PowerShell que descarga y ejecuta automáticamente la herramienta Microsoft Activation Scripts (MAS) para activar Windows y Office de forma gratuita.  

#### ⚡ Uso  
Activa Windows y Office sin necesidad de claves de producto o software adicional. Ejecuta el activador oficial más usado de la comunidad.  

#### 🚀 Comando  
```powershell
irm https://get.activated.win | iex
```

#### 🔧 **Comando alternativo (más explícito)**  
```powershell
Invoke-RestMethod https://get.activated.win | Invoke-Expression
```

#### 🔍 **Recomendación de optimización**  
Para ejecutar directamente sin confirmaciones:  

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; irm https://get.activated.win | iex
```

#### ⚠️ **Importante**  
- Ejecutar PowerShell como **Administrador**  
- Deshabilitar temporalmente el antivirus si bloquea la descarga  
- Funciona con Windows 8.1, 10, 11 y Office 2016-2021

📎 **Nota:** Este script usa métodos de activación digital legítimos reconocidos por Microsoft

## 📝 nm -u - Listar símbolos indefinidos de un ejecutable

#### 📌 Descripción  
Muestra funciones, métodos y símbolos externos que un ejecutable necesita de bibliotecas compartidas como GLIBC.  

#### ⚡ Uso  
Identifica dependencias externas de un binario y diagnostica errores de enlazado o bibliotecas faltantes.  

#### 🚀 Comando  
```bash
nm -u ./ejecutable
```

#### 🔍 Recomendación de optimización  
Para limpiar símbolos raros (@, versiones) y mostrar solo nombres de funciones:  

```bash
nm -Du ./ejecutable | sed 's/@.*//'
```

📎 **Nota:** Requiere `binutils` instalado (incluido por defecto en sistemas de desarrollo)

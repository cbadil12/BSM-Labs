# PRÁCTICA 1: Git
## 1. Instalar e importa la máquina virtual en Virtualbox
### 1.1 Instala Virtualbox
https://www.virtualbox.org/wiki/Downloads
### 1.2 Importar la máquina virtual base
 - Datos para la descarga: https://mega.nz/file/VPcxCIKC
 - Password: qdQ-fRe2whvDAHqNvuGN7vZ1YQjoMsLUW-aEEUhEGAo
 - Datos de la máquina virtual:
   - Usuario: ev-k8s
   - Password: k8s
### 1.3 Iniciar la máquina virtual

## 2. Instalar VisualStudio
`sudo snap installa code --classic`
## 3. Instalar Git
```
sudo apt update
sudo apt install git
```

## 4. Crear cuenta y repositorio en GitHub
https://github.com/
## 5. Configurar git para que utilice mi usuario y contraseña
```
git config --global user.ame "Carlos Luis Badillo"
git congig --global user.email cbadil12@gmail.com
```
## 6. Crear mi primer repositorio local y subirlo a GitHub
### 6.1 Se inicializa el repositorio
 - `git init` <br
 - `git remote add origin https://github.com/cbadil12/BSM-Labs.git`
### 6.2 Se crea el fichero README.md
 - `echo "#Título principal del README" >> README.md`
### 6.3 Se actualizar el repositorio
 - Primero se guarda el archivo con CTRL+S
 - `git add LAB1` Añade el fichero LAB1 a la zona de staging
 - `git commit -m "Comentario del commit"` Confirma el cambio
 - `git push -u origin main` Sincroniza el repositorio en la rama main
## 7. Crea una nueva rama de desarrollo "dev" y sincroniza el repositorio para tener esa rama en GitHub
 - `git branch` Ver todas lasa ramas del proyecto 
 - `git branch dev Crear nueva rama "dev"
 - `git push -u origin dev` Sincroniza el repositorio en la rama dev
## 8. Cambia a la rama principal de nuevo, crea otro fichero y sincroniza de nuevo el respotorio.
 - `git checkout main` Cambiar a rama "main"
 - `echo "#Título principal de las Prácticas de Laboratorio" >> PRACTICAS.md`
 - `git add PRACTICAS.md` Añade el fichero PRACTICAS.md a la zona de staging
 - `git commit -m "Comentario del commit"` Confirma el cambio
 - `git push -u origin main` Sincroniza el repositorio en la rama main
## 9. Cambia la rama de desarrollo haz un cambio en esta rama y sincroniza con GitHub
 - `git checkout dev` Cambiar a rama "dev"
 - Hago cambio en el README
 - `git add README.md` Añade el fichero README.md a la zona de staging
 - `git commit -m "Comentario del commit"` Confirma el cambio
 - `git push -u origin dev` Sincroniza el repositorio en la rama dev
## 10. Solicita un pull request desde la interfaz de usuario de GitHub para que ese cambio se integre en la rama principal
 - Compare & Pull request
 - base: main <- compare:dev
 - Create pull request
# PRÁCTICA 1: Git
## Crear cuenta en GitHub y crear un repositorio
- https://github.com/
## Configurar git paar que utilice mi usuario y contraseña
- `git config --global user.ame "Carlos Luis Badillo"`
- `git congig --global user.email cbadil12@gmail.com`
## Crear mi primer repositorio local y subirlo a GitHub
### Se inicializa el repositorio
- `git init` <br
- `git remote add origin https://github.com/cbadil12/BSM-Labs.git`
### Se crea el fichero README.md
- `echo "# BSM-Labs" >> README.md`
### Se actualizar el repositorio
- Primero se guarda el archivo con CTRL+S
- `git add LAB1` Añade el fichero LAB1 a la zona de staging
- `git commit -m "Comentario del commit"` Confirma el cambio
- `git push -u origin main` Sincroniza el repositorio en la rama main
- `git push -u origin dev` Sincroniza el repositorio en la rama dev

# COMANDOS ÚTILES EN Git
## Comandos generales
`git help` <br />
`git status` <br />
## Comandos para trabajar con ramas
`git branch` Ver todas lasa ramas del proyecto <br />
`git branch [nueva_rama]` Crear nueva rama <br />
`git checkout -b [nueva_rama]` Crear nueva rama y cambiar de rama <br />
`git checkout [nombre_rama]` Cambiar a otra rama <br />
`git pull origin main`Hace pull desde la rama main en el origin al repositorio local

## Comandos básicos en la terminal
`ls` Lista el contenido del directorio en el que me encuentro <br />
`cd <directorio>` Para meterme en el fichero/carpeta <br />
`cd ..` Para ir al directorio jerarquicamente anterior <br />

Esta modificacion la estoy haciendo desde la rama dev


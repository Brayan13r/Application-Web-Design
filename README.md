# Información del Estudiante y de la Materia

---

## Datos Personales
* **Nombre completo:** Brayan Eleazar Villegas Navarro
* **Matrícula:** Al03091719
* **Carrera:** IDS (Ingeniería en Desarrollo de Software)
* **Semestre:** 6

---

## Datos de la Materia
* **Nombre de la asignatura:** Aplicaciones web
* **Nombre del profesor:** Cristopher Gerardo Gaytan Diaz

---

## Descripción
**Markdown** es un lenguaje de marcado ligero que permite aplicar formato a un texto utilizando caracteres sencillos de texto plano. Se utiliza principalmente para:
1. Crear documentación técnica y archivos README en repositorios.
2. Escribir contenido para la web de forma rápida sin necesidad de usar HTML complejo.
3. Facilitar la lectura del contenido tanto en su forma cruda como cuando ya está procesado.

---

## Opciones de Etiquetado en Markdown
A continuación, se describen las herramientas de formato más comunes:

### 1. Títulos y subtítulos
Se utilizan para organizar el contenido jerárquicamente mediante el símbolo `#`.

### 2. Texto en negritas y cursivas
Sirven para resaltar ideas. Las **negritas** se crean con `**texto**` y las *cursivas* con `*texto*`.

### 3. Listas ordenadas y no ordenadas
Estructuran puntos clave usando guiones (`-`) para viñetas o números (`1.`) para secuencias.

### 4. Enlaces
Permiten redireccionar a páginas web externas usando la sintaxis `[Texto](URL)`.

### 5. Imágenes
Insertan recursos visuales anteponiendo un signo de exclamación: `![Descripción](URL)`.

### 6. Bloques de código
Muestran fragmentos de programación con formato monoespaciado usando tres comillas invertidas (```).

---

## Comandos de Git Utilizados
Esta sección detalla los comandos esenciales para la gestión del repositorio:

### Ver el estado del repositorio local
* `git status`: Muestra el estado actual de la rama, informando qué archivos han sido modificados, eliminados o están listos para agregarse.

### Agregar archivos al Stage
* `git add nombre_del_archivo`: Prepara un archivo individual específico para el siguiente commit.
* `git add .`: Agrega todos los archivos modificados o nuevos de la carpeta actual al área de preparación.

### Agregar comentarios a un commit
* `git commit -m "mensaje"`: Registra los cambios en el historial local con un comentario breve que describe lo que se hizo.

### Subir cambios al repositorio remoto
* `git push origin nombre_de_la_rama`: Envía los commits realizados en el repositorio local al servidor remoto (como GitHub).

### Gestión de ramas
* `git branch nombre_de_rama`: Crea una nueva rama para trabajar en una funcionalidad por separado.
* `git branch`: Lista todas las ramas existentes en el repositorio local.
* `git checkout nombre_de_rama` o `git switch nombre_de_rama`: Permite cambiar de una rama a otra.
* `git branch -d nombre_de_rama`: Elimina la rama especificada (siempre que sus cambios ya hayan sido fusionados).

### Regresar el repositorio (Rollback)
* `git reset --hard id_del_commit`: Regresa el proyecto exactamente al estado de un commit específico, descartando todos los cambios posteriores.

---
# Guía práctica de Git y GitHub para el TP


## 🛠️ Comandos básicos

Estos son los comandos que más van a usar durante el TP:

```bash
git add .              # Agrega todos los cambios al área de staging
git commit -m "mensaje descriptivo"   # Crea un commit con un mensaje
git push               # Sube los cambios al repositorio remoto
git pull               # Baja y fusiona los últimos cambios del repositorio remoto
```




### 🚨 Tips:
- No hagan commits con mensajes genéricos como "cambios", "arreglos", "fix".
- Usen mensajes que expliquen **qué** hicieron: `agrega endpoint de getPosts`, `fix en validación de schemas en usuarios`.

```bash
add/agregar             # Funcionalidad nueva
fix                     # Arreglo de bug
update/actualizar       # Actualizar funcionalidad existente
refactor                # Reestructurar código sin modificar la logica
```
---

## 🌱 Ramas (branches)

Una buena práctica es **no trabajar todos en la rama `main`**. Creen ramas por cada tarea. y de ahí se mergean a main.

### Crear una rama nueva y moverse a ella:
```bash
git switch -c nombre-de-la-rama
```

> `switch` es el comando moderno para moverse entre ramas. Evitamos `checkout` porque es más propenso a confusión y está siendo reemplazado. (Aunque se sigue utilizando - También sirve para restaurar un archivo)

> El -c le indica a git que cree una rama nueva y cambie a esta en un solo paso.

### Volver a `main`:
```bash
git switch main
```

### Subir la rama al remoto:
```bash
git push -u origin nombre-de-la-rama
```

> El -u solo se usa la primera vez para "crear" una relacion entre la rama local y la remota, en los siguientes push ya no hace falta

---

## 🧩 Buenas prácticas en nombres

Usen prefijos que expliquen qué tipo de cambio están haciendo, aunque cada empresa tiene sus prácticas, generalmente se sigue esta norma:

- `feature/nombre`: para agregar una funcionalidad nueva → `feature/login`
- `fix/nombre`: para corregir errores → `fix/validacion-email`
- `refactor/nombre`: para mejorar o reorganizar código → `refactor/controlador-pedidos`

---

## 🤝 Pull Requests y trabajo en equipo

Cuando terminan una tarea en su rama, en lugar de hacer `merge` directamente, lo ideal es abrir un **Pull Request** desde GitHub.

- Permite revisar el código antes de fusionarlo
- Si hay conflictos, se pueden resolver desde GitHub o Visual Studio Code
- Se mantiene un historial más limpio y entendible (lo mas importante)

Antes de abrir un **Pull Request**, asegúrense de actualizar su rama con los últimos cambios de `main`. Esto ayuda a evitar conflictos y asegura que su trabajo esté con la última versión del proyecto.

```bash
git switch nombre-de-su-rama
git pull origin main
```

### Conflictos comunes:
Se dan cuando dos personas editan las mismas líneas. Git les va a mostrar algo como:

```text
<<<<<<< HEAD
versión de main
=======
versión de mi rama
>>>>>>> rama-secundaria
```

Desde Visual Studio Code se puede resolver fácilmente eligiendo qué cambios mantener o combinarlos.

---

## 🗂️ Organización con GitHub Projects

GitHub tiene una pestaña llamada **Projects**, que sirve como un tablero Kanban (como Trello). Ahí pueden:

- Crear tareas
- Asignarlas a personas
- Moverlas de "Por hacer" → "En progreso" → "Hecho"
- Asociarlas a Pull Requests o Issues

Ideal para dividir el trabajo de forma clara y ordenada.

---

# Simulación de conflicto Git

## 🔧 Pasos para generar el conflicto

### 1. Clonar el repositorio y crear rama base
```bash
# Clonamos el repositorio
git clone <url-del-repo>
cd <nombre-repo>

# Creamos un archivo inicial
echo "mensaje original" > saludo.txt

# Agregamos el archivo al área de staging
git add saludo.txt

# Creamos un commit con mensaje descriptivo
git commit -m "agrega archivo saludo.txt"

# Subimos los cambios al repositorio remoto
git push origin main

### 2. Crear y cambiar a la rama `rama-kevin`
git switch -c rama-kevin

# Modificamos el archivo
echo "Hola desde Kevin" > saludo.txt

# Creamos un commit con todos los cambios
git commit -m "modifica saludo desde rama-kevin"

# Subimos la rama al remoto estableciendo relación de seguimiento
git push -u origin rama-kevin

### 3. Cambiar a la rama `main` y luego crear `rama-nico`
git switch main

# Creamos otra rama nueva y nos movemos a ella
git switch -c rama-nico

# Modificamos el mismo archivo de forma diferente
echo "Hola desde Nico" > saludo.txt

# Creamos un commit con todos los cambios
git commit -m "modifica saludo desde rama-nico"

# Subimos la rama al remoto estableciendo relación de seguimiento
git push -u origin rama-nico
```

Ahora tenés dos ramas que modifican el mismo archivo en la misma línea.

## 💥 Simular conflicto

- Ir a GitHub
- Crear un Pull Request desde `rama-kevin` hacia `main` y hacer merge
- Luego crear un segundo Pull Request desde `rama-nico` hacia `main`

Vas a ver que GitHub detecta un **conflicto**.

## 🧩 Cómo resolverlo

### Desde GitHub
- Usar la interfaz de resolución de conflictos
- Elegir entre "Incoming change", "Current change" o combinarlos
- Confirmar merge

### Desde Visual Studio Code
- Si hacés `git pull origin main` desde `rama-nico` después del merge de `rama-kevin`, se verá el conflicto en el archivo:
```text
<<<<<<< HEAD
Hola desde Nico
=======
Hola desde Kevin
>>>>>>> main
```

- VS Code ofrece botones para elegir una versión o ambas
- Después de resolverlo, hacer:

```bash
git add saludo.txt
git commit -m "resuelve conflicto entre Nico y Kevin"
git push
```

Prueba

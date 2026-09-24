---

# Manual: Implantación y Uso del Gestor de Aulas TIC

Este documento detalla los pasos necesarios para desplegar una copia independiente del Gestor de Aulas TIC para tu centro educativo, así como sus características y normas de uso. El proceso de instalación no requiere conocimientos avanzados de programación.

## 🌟 Características Clave de la Aplicación

Antes de empezar, aquí tienes todo lo que este sistema puede hacer por tu centro:

* **Sistema anti-errores y cero solapamientos:** Los profesores solo pueden ver y reservar las aulas que realmente están libres en ese momento. Además, **los usuarios no pueden borrar ni modificar reservas**, evitando eliminaciones accidentales. Si alguien se equivoca, debe avisar al Coordinador TIC, quien puede anularla en segundos usando su PIN.
* **Avisos inteligentes de Aforo:** Si un profesor intenta reservar un aula que tiene menos ordenadores que el número de alumnos indicado, el sistema no lo bloquea, pero le muestra un aviso naranja de *"Aforo ajustado"* para que decida si le compensa.
* **Gestión de Carros Móviles:** Al seleccionar un recurso itinerante (como un carro de portátiles o tabletas), la aplicación lanza una advertencia obligatoria recordando que se debe avisar a Conserjería para que acerquen el recurso al aula.
* **Vista de Horarios Semanal interactiva:** En cualquier momento se puede consultar el cuadrante de la semana de un aula concreta. Esto es ideal no solo para comprobar reservas, sino para ver quién está ocupando un aula y poder pedirle un cambio o que te la preste un día concreto.
* **Reutilización Anual:** Incorpora un botón de "Formateo" en el panel de administración que permite borrar absolutamente todas las reservas con un solo clic en septiembre, dejando el sistema limpio para un nuevo curso.
* **Exportación de Registros (CSV):** Incluye una herramienta para descargar todo el histórico de reservas a un archivo Excel. Perfecto para justificar el uso de los equipos ante Dirección, Inspección o para la certificación del plan CoDiCe TIC.

---

## 🛠️ FASE 1: Crear la Base de Datos gratuita (Firebase)

El sistema utiliza Firebase (de Google) para almacenar las reservas. El plan gratuito es más que suficiente para el volumen de cualquier centro educativo.

1. Accede a [firebase.google.com](https://firebase.google.com?utm_source=gemini) e inicia sesión con una cuenta de Google (preferiblemente del centro).
2. Haz clic en **"Ir a la consola"** y luego en **"Crear un proyecto"**. Ponle nombre (ej. GestorAulasTIC) y finaliza el asistente.
3. En el menú lateral izquierdo, ve a **"Compilación"** (o Build) y selecciona **"Firestore Database"**.
4. Haz clic en **"Crear base de datos"**. Selecciona un servidor europeo (ej. `europe-west1`) y elige **Comenzar en modo de prueba**.
5. **CRÍTICO - Reglas de seguridad:** Para que la aplicación funcione a largo plazo, ve a la pestaña **Reglas** de Firestore, borra el contenido y pega exactamente esto:
```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}

```


6. Haz clic en **Publicar**.

## 🔑 FASE 2: Obtener las "Llaves" de tu base de datos

1. En el menú izquierdo de Firebase, pulsa el icono del engranaje (Configuración del proyecto).
2. En la sección "Tus aplicaciones", pulsa el icono web (`</>`).
3. Registra la aplicación con un nombre y haz clic en continuar.
4. Aparecerá un bloque de código. Localiza la sección que empieza por `const firebaseConfig = { ... }`. Cópiala entera.

## 💻 FASE 3: Configurar el código de la aplicación

1. Abre el archivo `index.html` con un editor de texto (Bloc de Notas, Visual Studio Code o desde el propio editor de GitHub).
2. Busca la línea `// TUS LLAVES DE FIREBASE (No tocar)` (línea 160 aprox).
3. Borra el bloque `const firebaseConfig = { ... };` que hay allí y **pega el tuyo** (el que copiaste en el paso anterior).
4. Guarda el archivo.
5. Sube el archivo a un servicio de alojamiento gratuito como **GitHub Pages**. Ese enlace generado será el que usarán los profesores de tu centro (lo puedes incrustar en Teams, enviar por correo o crear accesos directos).

## ⚙️ FASE 4: Configurar la aplicación (Panel de Control)

La primera vez que abras tu enlace web, la aplicación cargará unos datos de ejemplo. Debes personalizarlos para tu centro:

1. Ve a tu aplicación y haz clic en el **icono de la rueda dentada** arriba a la derecha.
2. Introduce el PIN por defecto: **`240727`** (cámbialo después por seguridad).
3. Se abrirá la pestaña oculta de **Administración**.
4. **Configuración General:** Cambia el nombre del IES, el PIN y establece las fechas de inicio y fin del curso actual.
5. **Días Festivos:** Añade los días no lectivos de tu provincia o comunidad para que el sistema bloquee automáticamente las reservas en esas fechas.
6. **Gestión de Aulas:**
* Borra las aulas de ejemplo pulsando la papelera roja.
* Utiliza el formulario inferior para crear las aulas de tu centro.
* *Tipos de recurso:* **Aula Fija** (sala normal), **Carro Móvil** (activa el aviso a conserjería) o **Especial** (bibliotecas, audiovisuales, etc.).



## 👨‍🏫 FASE 5: Cómo usan los profesores la aplicación

El uso para el claustro es extremadamente rápido e intuitivo:

1. **Buscar aula:** En la pestaña "Buscar y Reservar", seleccionan la fecha en el calendario, la hora de la clase, el nivel educativo y el número de alumnos.
2. **Seleccionar:** El sistema descarta las aulas ocupadas. Si un aula tiene menos equipos que alumnos, mostrará un aviso de aforo ajustado. El profesor hace clic en "Seleccionar" en la opción deseada.
3. **Confirmar:** Introduce su Nombre, Curso y Materia. Finalmente elige la duración:
* **Solo este día puntual:** Únicamente reserva esa fecha.
* **Fijo para todo el curso:** Reserva ese mismo día de la semana a esa hora hasta junio.


4. **Comprobar o coordinar:** En la pestaña "Ver Horarios", seleccionando el aula, pueden confirmar que su reserva aparece correctamente. También sirve para consultar quién ocupa un aula si necesitan pedirle un cambio de hora a un compañero.
5. **Errores:** Si se equivocan al reservar, el profesor no puede borrarla. Debe contactar con el Coordinador TIC para que la elimine.

## 🧹 Mantenimiento Avanzado (Solo Coordinador TIC)

Dentro del panel de la rueda dentada encontrarás la "Zona de Peligro", que cuenta con dos herramientas fundamentales:

* **Exportar a CSV:** Descarga un Excel completo con todas las reservas registradas. Ideal para tener un archivo de uso de las aulas a final de trimestre o curso.
* **Borrar TODAS las reservas:** Al finalizar el año escolar en junio, este botón formateará por completo la base de datos dejándola a cero, lista para reutilizar la aplicación el curso siguiente sin esfuerzo.

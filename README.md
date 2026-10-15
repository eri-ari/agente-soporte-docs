# 🤖 Agente de Soporte Inteligente con Gemini & Google Sheets

[![Licencia MIT](https://img.shields.io/badge/Licencia-MIT-blue.svg)](LICENSE)
[![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-Automated-purple.svg)](.github/workflows/run_agent.yml)
[![Google Gemini](https://img.shields.io/badge/Google_Gemini-2.5_Flash-orange.svg)](https://ai.google.dev/)
[![Propósito](https://img.shields.io/badge/Fines-Educativos_y_de_Práctica-green.svg)](#-aviso-de-proyecto-educativo-y-sin-fines-de-lucro)

> **Aplicación de código abierto diseñada para procesar y clasificar comentarios de clientes en tiempo real dentro de Google Sheets utilizando la IA de Google Gemini, automatizada en la nube mediante GitHub Actions.**

---

## 🎓 Aviso de Proyecto Educativo y Sin Fines de Lucro

Este repositorio y su software han sido creados con **fines exclusivamente académicos, didácticos y de práctica técnica**.
- **No tiene fines comerciales ni lucrativos.**
- No se recopilan, venden ni almacenan datos personales en bases de datos externas.
- Cumple con la [Política de Datos de Usuario de los Servicios de API de Google](docs/privacy.html#datos-google) y sus requisitos de uso limitado (*Limited Use*).

---

## 🌐 Publicación en GitHub Pages y Verificación en Google Cloud

Para completar la **configuración de la marca (Pantalla de consentimiento de OAuth)** en Google Cloud Console, este repositorio incluye un sitio web estático listo para desplegarse en **GitHub Pages** ubicado en la carpeta [`docs/`](docs/):

- **Página principal de la aplicación:** `https://eri-ari.github.io/agente-soporte-about/`
- **Vínculo a la política de privacidad:** `https://eri-ari.github.io/agente-soporte-about/privacy.html`
- **Vínculo a las condiciones del servicio:** `https://eri-ari.github.io/agente-soporte-about/terms.html`

### Cómo activar GitHub Pages en tu repositorio:
1. Sube tu código a un repositorio público en GitHub.
2. En GitHub, ve a **Settings** > **Pages** (en el menú lateral).
3. En **Build and deployment** > **Branch**:
   - Selecciona la rama `main` (o `master`).
   - Selecciona la carpeta `/docs`.
   - Haz clic en **Save**.
4. ¡Listo! En 1-2 minutos tu página estará activa y podrás colocar las URLs en Google Cloud Console.

---

## 🚀 Arquitectura y Funcionamiento en la Nube (GitHub Actions)

El flujo opera de forma 100% desatendida en la nube:

```
[Google Sheets: Hoja 'Comentarios']
           ▲
           │ 1. Lee filas pendientes (Columna F != "SI")
           ▼
[GitHub Actions Runner (Cron / Manual)]
           │ 2. Envía comentario a Gemini
           ▼
[Google Gemini AI (gemini-2.5-flash)]
           │ 3. Retorna JSON: {clasificacion, sentimiento}
           ▼
[GitHub Actions Runner]
           │ 4. Escribe Columna D (Clasif), E (Sentim) y F ("SI")
           ▼
[Google Sheets Actualizado]
```

---

## ⚙️ Configuración para GitHub Actions

Para que el programa se ejecute automáticamente sin necesidad de una computadora encendida:

### 1. Crear una Cuenta de Servicio (Service Account) en Google Cloud
1. Entra a [Google Cloud Console](https://console.cloud.google.com/).
2. Asegúrate de tener activada la **Google Sheets API** y la **Google Drive API** en tu proyecto.
3. Ve a **IAM y administración** > **Cuentas de servicio** > **Crear cuenta de servicio**.
4. Asígnale un nombre (ej. `agente-soporte-sheets`) y finaliza.
5. Haz clic en la cuenta de servicio creada > Pestaña **Claves** > **Agregar clave** > **Crear clave nueva** > Formato **JSON**.
6. Se descargará un archivo `.json`. **Copia su contenido completo**.
7. **Importante:** Copia el correo de la cuenta de servicio (ej. `agente-soporte-sheets@tu-proyecto.iam.gserviceaccount.com`), abre tu hoja de Google Sheets, haz clic en **Compartir** y dale permisos de **Editor** a ese correo.

### 2. Configurar los Secretos en GitHub
1. En tu repositorio de GitHub, ve a **Settings** > **Secrets and variables** > **Actions**.
2. Haz clic en **New repository secret** y agrega los siguientes secretos:
   - `GEMINI_API_KEY`: Tu API Key obtenida de [Google AI Studio](https://aistudio.google.com/).
   - `GCP_SA_KEY`: Pega el contenido íntegro del archivo JSON de la Service Account que descargaste.
   - `SPREADSHEET_NAME` *(Opcional)*: Nombre de la hoja (por defecto es `Comentarios`).

### 3. Ejecución
El flujo se ejecutará:
- **Automáticamente** cada hora según el archivo [`.github/workflows/run_agent.yml`](.github/workflows/run_agent.yml).
- **Manualmente** cuando quieras yendo a la pestaña **Actions** > **Ejecución Automática del Agente de Soporte** > **Run workflow**.

---

## 💻 Ejecución Local (Desarrollo y Pruebas)

Si deseas probarlo localmente en tu computadora:

1. Clona el repositorio e instala dependencias:
   ```bash
   pip install -r requirements.txt
   ```
2. Crea un archivo `.env` basado en `.env.example`:
   ```env
   GEMINI_API_KEY=tu_clave_de_gemini
   SPREADSHEET_NAME=Comentarios
   ```
3. Ejecutar una sola pasada:
   ```bash
   python agente_soporte.py --once
   ```
4. O ejecutar en bucle continuo programado (cada 2 minutos):
   ```bash
   python agente_soporte.py --loop
   ```

---

## 📄 Licencia

Este proyecto se distribuye bajo la licencia **MIT**. Para más detalles, consulta los [Términos de Servicio](docs/terms.html) y la [Política de Privacidad](docs/privacy.html).

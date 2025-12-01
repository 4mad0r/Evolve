# Práctica 1 – OSINT  
**Autor:** Ignacio Amador López  

## 📌 Descripción  
Esta práctica consiste en la realización de un ejercicio de **inteligencia de fuentes abiertas (OSINT)** para investigar a un usuario relacionado con un incidente de *spear phishing* dentro de la empresa Paterva.  
A partir de una configuración inicial de **Maltego**, se solicita identificar información relevante del usuario objetivo, así como obtener dos *flags* dejadas durante su actividad.

Además, como segundo ejercicio, se debe realizar una investigación OSINT sobre el profesor **Yuba González Parrilla**, recopilando información pública accesible mediante fuentes abiertas.

---

## 🧪 Ejercicio 1 – Investigación del usuario sospechoso  

Tras analizar correos filtrados obtenidos mediante transformadas de Maltego, se identifica como objetivo al usuario:

### 👤 Usuario investigado  
**Greg Hoglund**

### 🔍 Resultados obtenidos  
- **Flag #1:** `{Evolve_flag_OSINT_Maltego}`  
- **Red social encontrada:** Instagram  
- El análisis del perfil de Instagram revela credenciales para un servidor FTP que contiene una segunda flag.

---

### 🗄️ Servidor FTP  
Accediendo al FTP indicado en una de las publicaciones:

- **Servidor:** `ftp.ghoglound.com:21`  
- **Usuario:** `ghoglound.ghoglound.com`  
- **Contraseña:** `EVOLVE_ACADEMY17?_`

Dentro del servidor se encuentra la segunda flag:

- **Flag #2:** `{Aprobado_por_Manueh}`  

---

## 🛠️ Herramientas utilizadas  
- **Maltego** (transformadas y análisis de filtraciones)  
- **Sherlock** (enumeración de usuarios)  
- **WhatsMyName** (búsqueda multisitio de perfiles)  
- **Exiftool** (metadatos de imágenes)  
- **Whois** (verificación de dominios)  
- **OSINT manual** en redes sociales y buscadores  

---

## 📝 Conclusión  
La práctica permite aplicar técnicas OSINT reales para investigar perfiles, obtener información mediante transformadas, correlación de datos públicos y análisis de evidencias digitales.  
Se consigue identificar al objetivo, extraer las dos flags solicitadas y reunir información completa sobre un perfil real mediante fuentes abiertas.

---


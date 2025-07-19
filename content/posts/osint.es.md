+++
date = 2025-06-28
title = "Google Dorking para Pentesting con un Ejemplo Real"
slug = "google-dorking-vulnerabilidades-caso-real"
author = ["Gonci"]
description = "Cómo una simple búsqueda con Google Dorking me permitió descubrir datos sensibles expuestos (.env, .git, claves JWT) en un servidor real. Te explico el proceso y por qué estas técnicas son esenciales en OSINT y seguridad ética."
draft = false
tags = [
  "OSINT",
  "Google Dorking",
  "Ciberseguridad",
  "Exposición .env",
  "Exposición .git",
  "Exposición Secreto JWT",
  "Divulgación de vulnerabilidades",
  "Mala Configuración de Seguridad",
  "Seguridad Web"
]
+++

## Aviso Legal
Este post describe técnicas utilizadas únicamente con fines educativos y éticos. Toda la información descubierta estaba indexada públicamente por Google y era accesible sin ninguna autenticación o explotación. La intención es crear conciencia sobre configuraciones erróneas comunes en seguridad y ayudar a las organizaciones a mejorar sus defensas. Los problemas encontrados fueron reportados responsablemente a la empresa.

### ¿Qué es Google Dorking y por qué son importantes?
Google Dorking es la práctica de usar operadores avanzados de búsqueda para encontrar información específica indexada por motores de búsqueda, a veces datos sensibles que nunca deberían haber sido públicos. Para hackers éticos e investigadores de seguridad, es una herramienta poderosa para verificar qué tan seguros están realmente tus activos.

## El Dork que Usé
Para esta prueba, utilicé una consulta simple: `intitle:index.of ".git"`. Este dork busca directorios abiertos con carpetas `.git` expuestas.

![Google Dork Serach](/images/osint_search.png)

Sorprendentemente, el Google Dork me llevó no solo a una carpeta `.git` expuesta, sino también a un listado de directorio que revelaba el código fuente completo del backend y el archivo `.env` con credenciales sensibles en un panel de administración en vivo.

![Directory Listing and Exposed Data](/images/osint_exposed_data.png)

Esto fue lo que estaba accesible públicamente:
- **Archivo .env:** Contenía credenciales de MongoDB y una clave secreta JWT en uso.
- **Directorio .git:** El código fuente completo, historial de commits y configuraciones eran descargables.
- **Listado de directorios:** Cualquiera podía navegar por archivos y carpetas sin restricciones.
- **Ambiente de desarrollo:** El archivo .env mostraba ENV=development, indicando un despliegue en vivo mal configurado.
- **Credenciales Hardcodeadas:** Los archivos javascript no estaban siendo interpretados, por lo que se podía leer el contenido, mostrando contraseñas de MongoDB distintas a las del .env.

![Env File Exposed](/images/osint_env_certs.png)

**Importante:** Todo esto fue posible sin ningún hackeo, solo una búsqueda y lectura pública de archivos accesibles.

## Reportándolo de Manera Responsable
Obviamente, no utilicé ninguna credencial ni exploté el sistema. En cambio, contacté a la empresa con un informe claro recomendando:
1. Restringir el acceso a `.env` y `.git`.
2. Deshabilitar el listado de directorios.
3. Rotar los secretos y contraseñas de inmediato.
4. Revisar la configuración del despliegue.

![Final Report to Company](/images/osint_report.png)
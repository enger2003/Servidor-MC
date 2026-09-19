# Servidor-MC
Instrucciones para preparar un servidor de minecraft en linux

Antes de nada, estas instrucciones están preparadas para el montaje en un sistema "linux", en mi caso se va a montar en una Raspberri Pi 5, por lo que usaremos "ssh" para conectarnos de fora cómoda.

Trabajaremos con las opciones básicas a su vez que podremos gestionar algunos errores referentes a java y preparar scripts para poder modificar, apagar y encender el servidor de forma remota
```
.
├── 📄 README.md                <-- Guía rápida e índice
├── 📄 .gitignore              <-- Archivos a excluir (logs, mundos)
├── 📄 eula.txt                <-- Aceptación de la EULA (EULA=true)
├── ⚙️ start.sh                <-- Script de inicio para Linux/Mac
├── 📁 config/                 <-- Plantillas de configuración
│   ├── 🔧 server.properties  <-- Configuración principal
└── 📁 docs/                   <-- Documentación detallada
    ├── 📖 instalacion-java.md
    ├── 📖 abrir-puertos.md
```

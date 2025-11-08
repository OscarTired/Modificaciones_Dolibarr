# DOLIBARR - Modificaciones y Actualizaciones del Sistema

> Repositorio de archivos modificados, ajustes y actualizaciones del sistema ERP DOLIBARR implementadas según las necesidades específicas de la empresa.

---

## Estado del Proyecto

![Status](https://img.shields.io/badge/Status-Active-success?style=flat-square)
![Version](https://img.shields.io/badge/Version-Continuous%20Updates-blue?style=flat-square)
![License](https://img.shields.io/badge/License-Proprietary-red?style=flat-square)
![Last Updated](https://img.shields.io/badge/Last%20Updated-November%202025-informational?style=flat-square)

---

## Stack Tecnológico

![DOLIBARR](https://img.shields.io/badge/DOLIBARR-ERP%20System-1a73e8?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMjggMTI4Ij48dGV4dCB4PSI2NCIgeT0iNjQiIGZvbnQtc2l6ZT0iNTAiIGZpbGw9IiNmZmYiIHRleHQtYW5jaG9yPSJtaWRkbGUiIGR5PSIuM2VtIj5EPEwvdGV4dD48L3N2Zz4=)
![PHP](https://img.shields.io/badge/PHP-7.4%2B-777BB4?style=for-the-badge&logo=php)
![MySQL](https://img.shields.io/badge/MySQL-8.0%2B-005C84?style=for-the-badge&logo=mysql)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6%2B-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![HTML5](https://img.shields.io/badge/HTML5-Structure-E34C26?style=for-the-badge&logo=html5)
![CSS3](https://img.shields.io/badge/CSS3-Styling-1572B6?style=for-the-badge&logo=css3)

---

## Tecnologías y Herramientas Utilizadas

| Tecnología | Versión | Propósito |
|-----------|---------|----------|
| **DOLIBARR** | 20.0+ | Sistema ERP Base |
| **PHP** | 8.0+ | Backend/Lógica de Negocios |
| **MySQL** | 8.0+ | Base de Datos |
| **JavaScript** | ES6+ | Interactividad Frontend |
| **Apache** | 2.4+ | Servidor Web |
| **Git** | Latest | Control de Versiones |

---

## 📦 Contenido del Repositorio

```
📁 dolibarr-modifications/
├── 📁 core/
│   ├── 📄 Modificaciones de módulos principales
│   └── 📄 Ajustes de funcionalidades core
├── 📁 custom/
│   ├── 📄 Módulos personalizados
│   └── 📄 Extensiones custom
├── 📁 database/
│   ├── 📄 Scripts SQL
│   ├── 📄 Cambios de estructura BD
│   └── 📄 Migraciones
├── 📁 modules/
│   ├── 📄 Módulos específicos
│   └── 📄 Ajustes por módulo
├── 📁 templates/
│   ├── 📄 Plantillas modificadas
│   └── 📄 Cambios UI/UX
├── 📁 scripts/
│   ├── 📄 Scripts de automación
│   └── 📄 Herramientas de utilidad
└── 📄 README.md
```

---

## Características Principales

### Tipos de Modificaciones

- ✅ **Nuevas Columnas en BD** - Expansión de esquema con campos adicionales
- ✅ **Cambios de Formato** - Ajustes en presentación y estructura de datos
- ✅ **Optimizaciones** - Mejoras de rendimiento y eficiencia
- ✅ **Funcionalidades Extendidas** - Nuevas características según necesidades
- ✅ **Integraciones Personalizadas** - Conexiones con sistemas externos
- ✅ **Reportes Customizados** - Reportes específicos del negocio
- ✅ **Validaciones Mejoradas** - Reglas de negocio específicas

---

## Competencias Técnicas

![Backend](https://img.shields.io/badge/Backend-PHP%20%7C%20SQL-blue?style=flat-square)
![Frontend](https://img.shields.io/badge/Frontend-JavaScript%20%7C%20HTML%20%7C%20CSS-green?style=flat-square)
![Database](https://img.shields.io/badge/Database-MySQL%20%7C%20Design-yellow?style=flat-square)
![DevOps](https://img.shields.io/badge/DevOps-Git%20%7C%20Deployment-purple?style=flat-square)
![ERP](https://img.shields.io/badge/ERP-DOLIBARR%20Customization-red?style=flat-square)

---

## 📌 Información Importante

> [!IMPORTANT]
> Las modificaciones están diseñadas específicamente para cumplir con los requisitos operacionales internos.

> [!WARNING]
> **Antes de aplicar cambios:**
> - Realiza backups completos de la base de datos
> - Prueba en ambiente de staging primero
> - Revisa el changelog asociado a cada modificación
> - Verifica compatibilidad con tu versión DOLIBARR

> [!NOTE]
> **Notas de Desarrollo:**
> - Todas las modificaciones mantienen compatibilidad con el core de DOLIBARR
> - Se sigue la estructura de carpetas estándar de DOLIBARR
> - Los cambios son documentados archivo por archivo

---

## 📂 Estructura de Carpetas Detallada

### `/core` - Modificaciones Core
Contiene ajustes y modificaciones de los módulos fundamentales de DOLIBARR.

### `/custom` - Módulos Personalizados
Extensiones y funcionalidades custom desarrolladas específicamente.

### `/database` - Cambios de Base de Datos
Scripts SQL para nuevas columnas, tablas, índices y migraciones de datos.

### `/modules` - Módulos Específicos
Modificaciones por módulo (CRM, Facturación, Inventario, etc.).

### `/templates` - Plantillas y UI
Cambios en templates Twig y estilos CSS customizados.

### `/scripts` - Utilidades y Automatización
Scripts PHP/Bash para tareas administrativas y batch processing.

---

## 🔄 Flujo de Trabajo

```
1. Identificar Necesidad
   ↓
2. Analizar Impacto
   ↓
3. Implementar Cambio
   ↓
4. Probar (Staging)
   ↓
5. Documentar
   ↓
6. Commit y Deploy
```

---

## Documentación por Archivo

Cada archivo modificado incluye:

- **Encabezado Descriptivo** - Qué se modificó y por qué
- **Cambios Realizados** - Descripción del cambio específico
- **Fecha de Implementación** - Cuándo se realizó
- **Versión DOLIBARR** - Versión compatible
- **Autor/Responsable** - Quién implementó el cambio
- **Notas Técnicas** - Detalles técnicos relevantes

---

## Cómo Usar Este Repositorio

### 1️⃣ Clonar el Repositorio
```bash
git clone [tu-repositorio-url]
cd dolibarr-modifications
```

### 2️⃣ Seleccionar Cambios Aplicables
Revisa la carpeta de cambios que necesites implementar.

### 3️⃣ Realizar Backup
```bash
# Backup de BD
mysqldump -u usuario -p dolibarr > backup_$(date +%Y%m%d).sql

# Backup de archivos
cp -r /var/www/dolibarr /var/www/dolibarr.backup
```

### 4️⃣ Aplicar Cambios
Copia los archivos a tu instalación de DOLIBARR siguiendo la estructura.

### 5️⃣ Validar Implementación
- Verifica que DOLIBARR inicie correctamente
- Prueba las funcionalidades afectadas
- Revisa logs de error

### 6️⃣ Documentar Cambios
Actualiza el registro de modificaciones implementadas.

---

## 📊 Estadísticas del Proyecto

![Files](https://img.shields.io/badge/Files-Continuous-blue?style=flat-square)
![Last Commit](https://img.shields.io/badge/Last%20Commit-Recent-green?style=flat-square)
![Maintainance](https://img.shields.io/badge/Maintenance-Active-success?style=flat-square)

---

## 🔐 Consideraciones de Seguridad

> [!CAUTION]
> **Seguridad y Cumplimiento:**
> - No publiques credenciales o datos sensibles
> - Cumple con políticas de seguridad de la empresa
> - Mantén acceso restringido a personas autorizadas
> - Registra todos los cambios en control de versiones

---

## Contacto y Soporte

Para preguntas, problemas o sugerencias relacionadas con las modificaciones:

- **Equipo de Desarrollo**: oscarwork77@gmail.com

---

## 📖 Referencias Útiles

- [DOLIBARR Official Documentation](https://wiki.dolibarr.org)
- [DOLIBARR GitHub](https://github.com/Dolibarr/dolibarr)
- [DOLIBARR Community Forum](https://www.dolibarr.org/forum)
- [MySQL Documentation](https://dev.mysql.com/doc)
- [PHP Documentation](https://www.php.net/docs.php)

---

## 📋 Notas Finales

**Última Actualización:** Noviembre 2025  
**Mantenido por:** oscarwork77@gmail.com 
**Licencia:** GPL v3+

---

*Usa este repo de forma responsablemente y de acuerdo con las políticas.*

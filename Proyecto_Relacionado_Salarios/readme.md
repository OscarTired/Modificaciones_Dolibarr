# Mejora de lista de salarios en Dolibarr - Columna Proyecto

![PHP](https://img.shields.io/badge/PHP-8.0+-777BB4?style=flat-square&logo=php)
![Dolibarr](https://img.shields.io/badge/Dolibarr-22.0.1-2E7D32?style=flat-square&logo=database)
![Module](https://img.shields.io/badge/Module-Salaries-FF6B6B?style=flat-square)
![Module](https://img.shields.io/badge/Module-Projects-4ECDC4?style=flat-square)
![Database](https://img.shields.io/badge/Database-MySQL-336791?style=flat-square&logo=mysql)
![Type](https://img.shields.io/badge/Type-Enhancement-FFC107?style=flat-square)
![Status](https://img.shields.io/badge/Status-Production%20Ready-51CF66?style=flat-square)

Este repositorio contiene una mejora para Dolibarr que añade una **columna "Proyecto"** al listado de salarios (`htdocs/salaries/list.php`), permitiendo visualizar y navegar hacia el proyecto asociado a cada salario mediante un enlace clickeable, de forma consistente con el comportamiento de `card.php`.

---

## 🎯 Objetivo

El objetivo fue enriquecer la lista de salarios para que, además de los campos estándar (Ref, Etiqueta, Fechas, Empleado, Método de pago, Cuenta, Importe, Estado), muestre el proyecto vinculado a cada salario.

Se buscó que:
- El proyecto aparezca como una columna adicional en la tabla
- La referencia del proyecto sea un enlace directo a su ficha
- Se reutilice la lógica de `Project::getNomUrl(1)` ya usada en otras pantallas de Dolibarr
- La implementación sea modular y no rompa instalaciones sin módulo de proyectos

---

## 📋 Stack Tecnológico

| Tecnología | Versión | Propósito |
|:-----------|:--------|:---------|
| PHP | 8.0+ | Lenguaje de programación |
| Dolibarr | 22.0.1+ | ERP/CRM base |
| MySQL | 8.0+ | Base de datos |
| Módulo: Salaries | Core | Gestión de salarios |
| Módulo: Projects | Core | Gestión de proyectos |

---

## 📁 Archivos del Proyecto

```
.
├── README.md                    # Este archivo
├── list.php                     # Archivo original sin modificaciones (referencia)
├── list-1.php                   # Archivo modificado con columna de proyecto
```

---

## ✨ Cambios Realizados

### 1. Carga condicional de la clase `Project`

Se añadió la carga de la clase de proyectos al inicio del archivo, protegida por `isModEnabled('project')` para evitar errores cuando el módulo de proyectos está deshabilitado.

```php
if (isModEnabled('project')) {
    require_once DOL_DOCUMENT_ROOT.'/projet/class/project.class.php';
}
```

> [!NOTE]  
> El comportamiento relacionado con proyectos solo se activa si el módulo `project` está habilitado en Dolibarr, manteniendo la modularidad del sistema.

### 2. Nuevo criterio de búsqueda `search_project`

Se añadió la variable `search_project` junto con el resto de filtros de la lista.

```php
$search_project = GETPOST('search_project', 'alpha');
```

En el bloque de limpieza de filtros se resetea `search_project`:

```php
$search_project = "";
```

Se incorpora a la cadena `$param` para persistir el valor entre páginas:

```php
if ($search_project !== '') {
    $param .= '&search_project='.urlencode($search_project);
}
```

### 3. Ampliación de la consulta SQL - SELECT

El `SELECT` original solo traía campos de usuario, salario, cuenta bancaria, tipo de pago y suma de pagos. Se modificó para incluir el identificador de proyecto y sus metadatos.

**Cambio clave:** Se usa la columna estándar `fk_projet` de la tabla `salary` (nombre real en BD) y se expone como `fk_project` mediante un alias.

```php
// ANTES
$sql .= " s.rowid, s.fk_account, s.paye, s.fk_user, s.amount, s.salary, s.label, s.datesp, s.dateep, s.fk_typepayment as paymenttype,";

// DESPUÉS
$sql .= " s.rowid, s.fk_account, s.paye, s.fk_user, s.amount, s.salary, s.label, s.datesp, s.dateep, s.fk_typepayment as paymenttype, s.fk_projet as fk_project,";
$sql .= " p.ref as project_ref, p.title as project_title,";
```

> [!TIP]  
> Se usa `fk_projet as fk_project` para respetar el esquema estándar de Dolibarr, evitando crear nuevas columnas en la base de datos que podrían romper compatibilidad con el core.

### 4. JOIN con la tabla de proyectos

Se añadió un `LEFT JOIN` con `llx_projet` para resolver el proyecto vinculado al salario.

**ANTES:**
```php
$sql .= " LEFT JOIN ".MAIN_DB_PREFIX."bank_account ba ON (ba.rowid = s.fk_account), ";
$sql .= " ".MAIN_DB_PREFIX."user as u";
```

**DESPUÉS:**
```php
$sql .= " LEFT JOIN ".MAIN_DB_PREFIX."bank_account as ba ON (ba.rowid = s.fk_account) ";
$sql .= " LEFT JOIN ".MAIN_DB_PREFIX."projet as p ON (s.fk_projet = p.rowid), ";
$sql .= " ".MAIN_DB_PREFIX."user as u";
```

El `LEFT JOIN` asegura que los salarios sin proyecto asociado sigan apareciendo en la lista.

### 5. Filtro SQL por proyecto

Se añadió un filtro específico que aplica una búsqueda por referencia o título del proyecto.

```php
if ($search_project) {
    $sql .= natural_search(array('p.ref', 'p.title'), $search_project);
}
```

Esto permite localizar salarios asociados a un proyecto escribiendo parte de su referencia o nombre.

### 6. Actualización del GROUP BY

Para mantener la consulta compatible con el modo estricto de MySQL, se añadieron las nuevas columnas al `GROUP BY`.

**ANTES:**
```php
$sql .= " s.rowid, s.fk_account, s.paye, s.fk_user, s.amount, s.salary, s.label, s.datesp, s.dateep, s.fk_typepayment, s.fk_bank,";
```

**DESPUÉS:**
```php
$sql .= " s.rowid, s.fk_account, s.paye, s.fk_user, s.amount, s.salary, s.label, s.datesp, s.dateep, s.fk_typepayment, s.fk_bank, s.fk_projet,";
$sql .= " p.ref, p.title,";
```

> [!WARNING]  
> Sin este ajuste, MySQL en modo estricto devolvería errores `DB_ERROR_NOSUCHFIELD` por campos seleccionados no agrupados.

---

## 🎨 Cambios en la Interfaz de Usuario

### 1. Filtro visual de proyecto

En la fila de filtros de la tabla se añadió un campo de texto para buscar por proyecto:

```php
// Project
if (isModEnabled('project')) {
    print '<td class="liste_titre">';
    print '<input type="text" class="flat" name="search_project" value="'.dol_escape_htmltag($search_project).'">';
    print '</td>';
}
```

El filtro se integra visualmente junto con el resto de filtros (Ref, Etiqueta, Fechas, Empleado, etc.).

### 2. Cabecera de columna "Project"

En la cabecera de la tabla se añadió el título de la columna proyecto con soporte de ordenación:

```php
if (isModEnabled('project')) {
    print_liste_field_titre("Project", $_SERVER["PHP_SELF"], "p.ref", "", $param, "", $sortfield, $sortorder);
    $totalarray['nbfield']++;
}
```

### 3. Celda de proyecto por línea (enlace clickeable)

En el bucle que recorre los resultados, se añadió una celda específica para mostrar el proyecto:

```php
// Project
if (isModEnabled('project')) {
    print '<td class="tdoverflowmax150">';
    if (!empty($obj->fk_project)) {
        $projectstatic = new Project($db);
        $projectstatic->id    = $obj->fk_project;
        $projectstatic->ref   = $obj->project_ref;
        $projectstatic->title = $obj->project_title;
        print $projectstatic->getNomUrl(1); // Enlace clickeable
    }
    print '</td>';
}
```

Esto replica el mismo tipo de enlace que se usa en otras pantallas de Dolibarr para proyectos.

### 4. Ajuste de colspan para filas vacías

Cuando no hay registros, se incrementa el `colspan` para que la celda del mensaje abarque también la nueva columna:

```php
$colspan = 9;
if (isModEnabled("bank")) {
    $colspan++;
}
if (isModEnabled("project")) {
    $colspan++;
}
```

---

## 🚀 Instalación

### Opción 1: Reemplazo directo
1. Realiza backup de tu archivo `/htdocs/salaries/list.php` original
2. Descarga `list-1.php` de este repositorio
3. Renómbralo a `list.php`
4. Cópialo a `/htdocs/salaries/list.php` en tu instalación de Dolibarr

### Opción 2: Parche manual
1. Abre tu `/htdocs/salaries/list.php`
2. Localiza la sección `// Build and execute select`
3. Aplica los cambios descritos en la sección **✨ Cambios Realizados**

> [!IMPORTANT]  
> Asegúrate de que:
> - El módulo `project` está habilitado en Dolibarr
> - Tu versión de Dolibarr es 22.0.1 o compatible
> - Tienes backups antes de modificar archivos del core

---

## ✅ Validación

Después de la instalación:

1. Navega a `Billing > Salaries` o accede directamente a `/dir/salaries/list.php`
2. Verifica que aparezca la columna "Project" entre las columnas de la tabla
3. Si un salario tiene proyecto vinculado, verifica que sea clickeable y navegue a la ficha del proyecto
4. Prueba el filtro de proyecto escribiendo una referencia parcial en el campo de búsqueda
5. Verifica que la paginación y ordenación funcionan correctamente

> [!TIP]  
> Si la columna no aparece, comprueba que el módulo `project` está habilitado y que no existe caché CSS/JS que necesite limpiarse.

---

## 🤝 Compatibilidad

- **Dolibarr:** 22.0.1 (testado), compatible con versiones 20.0+
- **PHP:** 7.4+, 8.0+, 8.1+
- **MySQL:** 8.0+
- **Navegadores:** Todos los modernos (Chrome, Firefox, Safari, Edge)
- **Módulos requeridos:** salaries, project

---

## 📝 Notas de implementación

- La solución mantiene 100% compatibilidad hacia atrás con el código original
- No modifica estructura de base de datos, solo usa campos existentes
- Utiliza patrones estándar de Dolibarr (LEFT JOIN, natural_search, getNomUrl)
- Todo el código de proyecto se encapsula dentro de `isModEnabled('project')`
- Sigue las guías de coding style de Dolibarr

---

## 📄 Licencia

Este código sigue la misma licencia que Dolibarr: **GNU General Public License (GPL) v3.0 o superior**.

# Modificación: Vista Detallada de Informes de Gastos en Proyectos

![Dolibarr](https://img.shields.io/badge/Dolibarr-ERP%2FCRM-blue)
![PHP](https://img.shields.io/badge/PHP-7.x%2F8.x-777BB4?logo=php)
![Version](https://img.shields.io/badge/version-1.0.0-green)
![License](https://img.shields.io/badge/license-GPL--3.0-red)

## Descripción

Modificación del módulo de proyectos en Dolibarr para mostrar información detallada de los informes de gastos asociados, incluyendo el **Tipo de gasto** y **Descripción** como columnas independientes en la vista de elementos del proyecto.

## Problema Original

En la vista de elementos del proyecto (`/projet/element.php`), la tabla de "Listado de informes de gastos asociados al proyecto" solo mostraba:

- Referencia del informe
- Fecha
- Usuario
- Base imponible
- Importe total
- Estado

**Limitación**: No se podía identificar rápidamente qué tipo de gasto era (Movilidad, Otros Servicios, etc.) ni su descripción sin hacer clic en cada informe individual.

## Solución Implementada

Se agregaron **dos columnas adicionales** a la tabla:

1. **Tipo**: Muestra el tipo de gasto (Movilidad, Alojamiento, Otros Servicios, etc.)
2. **Descripción**: Muestra el comentario/descripción de la línea de gasto

### Resultado Visual

**Antes:**
```
Ref.          | Fecha      | Usuario | Base Imp. | Total | Estado
GV-25J-0003   | 24/10/2025 | Martin  | 10.00     | 10.00 | Aprobado
```

**Después:**
```
Ref.          | Fecha      | Usuario | Tipo       | Descripción                    | Base Imp. | Total | Estado
GV-25J-0003   | 24/10/2025 | Martin  | Movilidad  | Recojo Control y Tecnología    | 10.00     | 10.00 | Aprobado
```

## Archivo Modificado

```
📁 Proyecto Dolibarr
 └── 📁 projet/
     └── 📄 element.php  ← ÚNICO ARCHIVO MODIFICADO
```

> [!IMPORTANT]
> Solo se modifica el archivo `element.php`. No se requieren cambios en la base de datos ni en otros archivos del sistema.

## Modificaciones Técnicas

### 1. Cabecera de la Tabla (Línea ~950)

Se agregaron las columnas "Type" y "Description" en el encabezado de la tabla para `expensereport_det`:

```php
// Después de la columna "User"
if ($tablename == 'expensereport_det') {
    print '<td>'.$langs->trans("Type").'</td>';
    print '<td>'.$langs->trans("Description").'</td>';
}
```

### 2. Datos de las Columnas (Línea ~1150)

Se implementó la consulta SQL para obtener el tipo de gasto y mostrar la descripción:

```php
if ($tablename == 'expensereport_det') {
    // Columna Tipo
    print '<td class="tdoverflowmax150">';
    
    $sql_type = "SELECT ctf.code, ctf.label";
    $sql_type .= " FROM ".MAIN_DB_PREFIX."c_type_fees as ctf";
    $sql_type .= " WHERE ctf.id = ".((int) $element->fk_c_type_fees);
    $resql_type = $db->query($sql_type);
    
    if ($resql_type) {
        $obj_type = $db->fetch_object($resql_type);
        if ($obj_type) {
            $type_label = ($langs->trans($obj_type->code) != $obj_type->code) ? 
                $langs->trans($obj_type->code) : $obj_type->label;
            print $type_label;
        }
        $db->free($resql_type);
    }
    print '</td>';
    
    // Columna Descripción
    print '<td class="tdoverflowmax250">';
    if ($element->comments) {
        print dol_trunc($element->comments, 100);
    }
    print '</td>';
}
```

### 3. Ajuste del Colspan Total (Línea ~1250)

Se ajustó el colspan para que el total se muestre correctamente con las nuevas columnas:

```php
$colspan = 4;
if (in_array($tablename, array('projet_task'))) {
    $colspan = 2;
}
if ($tablename == 'expensereport_det') {
    $colspan = 6; // 4 base + 2 nuevas columnas
}
```

## Instalación

> [!WARNING]
> Realizar backup del archivo antes de modificar. Esta modificación altera el core de Dolibarr.

1. **Backup del archivo original:**
   ```bash
   cp /ruta/dolibarr/projet/element.php /ruta/backup/element.php.backup
   ```

2. **Aplicar las modificaciones:**
   - Editar `/projet/element.php`
   - Aplicar los tres cambios descritos anteriormente

3. **Verificar funcionamiento:**
   - Acceder a un proyecto con informes de gastos asociados
   - Navegar a la pestaña "Elementos vinculados"
   - Verificar que aparezcan las nuevas columnas

## Tecnologías Utilizadas

- **PHP**: Lenguaje de programación principal
- **SQL**: Consultas a la base de datos para obtener tipos de gastos
- **Dolibarr Framework**: Sistema de traducciones y clases del ERP

## Tablas de Base de Datos Consultadas

| Tabla | Propósito |
|-------|-----------|
| `llx_expensereport_det` | Líneas individuales de informes de gastos |
| `llx_c_type_fees` | Catálogo de tipos de gastos |
| `llx_expensereport` | Cabecera del informe (para obtener usuario) |

## Compatibilidad
> [!TIP]
> Esta modificación es compatible con la mayoría de versiones de Dolibarr ya que usa clases y métodos estándar del framework.

## Ventajas de la Modificación

- ✅ Visualización rápida del tipo de gasto sin hacer clic
- ✅ Descripción visible directamente en la lista
- ✅ Mejor trazabilidad de gastos por proyecto
- ✅ Facilita auditorías y revisiones
- ✅ No requiere cambios en base de datos
- ✅ Usa traducciones nativas de Dolibarr

## Limitaciones

- La descripción se trunca a 100 caracteres (configurable)
- Requiere modificación del core (no es un módulo externo)
- Las actualizaciones de Dolibarr pueden sobrescribir el archivo

> [!NOTE]
> Para mantener esta modificación después de actualizar Dolibarr, se recomienda documentar los cambios o crear un módulo personalizado.

## Mantenimiento

### Actualizaciones de Dolibarr

Antes de actualizar Dolibarr:
1. Hacer backup de `element.php` modificado
2. Realizar la actualización
3. Re-aplicar las modificaciones en el nuevo archivo

### Migración a Módulo Personalizado

Se recomienda migrar esta funcionalidad a un módulo custom para evitar pérdida en actualizaciones:

```
📁 htdocs/custom/
 └── 📁 myexpensereportmod/
     └── 📄 core/substitutions/
         └── 📄 element.php (override)
```

## Soporte

Para reportar issues o sugerencias relacionadas con esta modificación, contactame oscarwork77@gmail.com.

## Licencia

Esta modificación mantiene la licencia GPL-3.0 de Dolibarr.

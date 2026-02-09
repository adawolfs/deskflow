# Archivos y Directorios Creados por Deskflow

Este documento describe todos los archivos y directorios que Deskflow crea durante la instalación, configuración y ejecución.

## Resumen General

Deskflow crea varios archivos y directorios para almacenar:
- Configuración y ajustes del usuario
- Configuración del servidor
- Archivos de registro (logs)
- Certificados TLS/SSL y datos de seguridad
- Estado de la aplicación

Las ubicaciones de estos archivos varían según el sistema operativo.

## Directorios Base Específicos por Plataforma

### Windows
- **Directorio de Configuración**: `%APPDATA%\Roaming\Deskflow\` (típicamente `C:\Users\<nombre_usuario>\AppData\Roaming\Deskflow\`)
- **Modo Portable**: `<ruta-instalación>\settings\` (cuando existe `Deskflow.conf` en el directorio de instalación)
- **ProgramData**: `C:\ProgramData\Deskflow\` (usado para archivos del sistema cuando no está en modo portable)

### macOS
- **Directorio de Configuración**: `~/Library/Deskflow/`
- **Directorio del Sistema**: `/Library/Deskflow/` (respaldo para lectura)

### Linux
- **Directorio de Configuración**: `$XDG_CONFIG_HOME/Deskflow/` o `~/.config/Deskflow/`
- **Directorio de Estado**: `$XDG_STATE_HOME/` (para archivos de estado)
- **Directorio del Sistema**: `/etc/Deskflow/` (respaldo para lectura)

## Archivos y Directorios Creados

### 1. Archivos de Configuración

#### Archivo Principal de Configuración
**Archivo**: `Deskflow.conf` (formato INI)
**Ubicación**: 
- Windows: `%APPDATA%\Roaming\Deskflow\Deskflow.conf` o `<ruta-instalación>\settings\Deskflow.conf`
- macOS: `~/Library/Deskflow/Deskflow.conf`
- Linux: `~/.config/Deskflow/Deskflow.conf`

**Propósito**: Almacena todas las configuraciones de la interfaz gráfica, incluyendo configuración de cliente/servidor, ajustes de red y preferencias del usuario.
**Creación**: Automáticamente en el primer inicio si no se encuentra

#### Archivo de Configuración del Servidor
**Archivo**: `deskflow-server.conf` (formato texto)
**Ubicación**: Mismo directorio que `Deskflow.conf`

**Propósito**: Almacena el diseño de pantallas del servidor y la configuración (qué computadoras están conectadas y sus posiciones)
**Creación**: Al ejecutar en modo servidor y guardar la configuración

#### Archivo de Estado de la Aplicación
**Archivo**: `deskflow.state`
**Ubicación**: 
- Linux: `$XDG_STATE_HOME/deskflow.state`
- Otras plataformas: Mismo directorio que `Deskflow.conf`

**Propósito**: Almacena el estado de ejecución de la aplicación
**Creación**: Durante la ejecución de la aplicación

### 2. Archivos de Registro (Logs)

#### Archivo de Registro de Usuario
**Archivo**: `Deskflow.log`
**Ubicación**: Directorio home del usuario (`~/<AppId>.log`)

**Propósito**: Registro general de la aplicación
**Creación**: Cuando el registro está habilitado (configurable en ajustes)
**Rotación**: Rota cuando alcanza 1MB de tamaño

#### Archivo de Registro del Daemon (Windows)
**Archivo**: `deskflow-daemon.log`
**Ubicación**: Mismo directorio que `Deskflow.conf`

**Propósito**: Registro del proceso daemon en Windows
**Creación**: Cuando el daemon está ejecutándose
**Rotación**: Rota cuando alcanza 1MB de tamaño

#### Archivo de Registro Personalizado
**Archivo**: Ruta especificada por el usuario
**Ubicación**: Configurable a través de la interfaz gráfica

**Propósito**: Ubicación personalizada para la salida de registros
**Creación**: Cuando se configura una ruta de registro personalizada
**Rotación**: Rota cuando alcanza 1MB de tamaño

### 3. Archivos TLS/Seguridad

#### Directorio TLS
**Directorio**: `tls/`
**Ubicación**: Subdirectorio del directorio de configuración (ej., `~/.config/Deskflow/tls/`)

**Propósito**: Contiene todos los certificados y archivos relacionados con TLS
**Creación**: Automáticamente cuando TLS está habilitado

#### Certificado SSL
**Archivo**: `Deskflow.pem` (o `<AppId>.pem`)
**Ubicación**: `<directorio-configuración>/tls/Deskflow.pem`

**Propósito**: Certificado SSL/TLS auto-firmado para conexiones encriptadas
**Creación**: Generado automáticamente en el primer uso si no está presente
**Formato**: Formato PEM (contiene tanto el certificado como la clave privada)

#### Base de Datos de Servidores Confiables
**Archivo**: `trusted-servers`
**Ubicación**: `<directorio-configuración>/tls/trusted-servers`

**Propósito**: Almacena huellas digitales (fingerprints) de certificados de servidores confiables (para clientes)
**Creación**: Cuando un cliente confía en la huella digital de un servidor
**Formato**: Archivo de texto con entradas de huellas digitales

#### Base de Datos de Clientes Confiables
**Archivo**: `trusted-clients`
**Ubicación**: `<directorio-configuración>/tls/trusted-clients`

**Propósito**: Almacena huellas digitales (fingerprints) de certificados de clientes confiables (para servidores)
**Creación**: Cuando un servidor confía en la huella digital de un cliente
**Formato**: Archivo de texto con entradas de huellas digitales

### 4. Archivos Temporales y de Prueba

#### Directorios de Prueba (Solo Desarrollo)
**Directorio**: `/tmp/test/`
**Ubicación**: Directorio temporal del sistema

**Propósito**: Usado por pruebas unitarias para probar operaciones de archivos
**Creación**: Solo durante la ejecución de pruebas
**Limpieza**: Después de que las pruebas se completan

## Detalles de Creación de Archivos

### Creación Automática
La mayoría de archivos y directorios se crean automáticamente:

1. **Directorio de Configuración**: Creado en el primer inicio si no existe
2. **Directorio TLS**: Creado cuando TLS se habilita por primera vez
3. **Archivos de Configuración**: Creados con valores por defecto en el primer uso
4. **Archivos de Registro**: Creados cuando el registro está habilitado
5. **Certificados**: Auto-generados si faltan cuando TLS está habilitado

### Creación Activada por el Usuario
Algunos archivos se crean por acciones del usuario:

1. **Configuración del Servidor**: Guardada cuando el usuario configura el diseño de pantallas
2. **Huellas Digitales Confiables**: Añadidas cuando el usuario acepta un certificado
3. **Archivos de Registro Personalizados**: Creados en rutas especificadas por el usuario

### Comportamiento Específico por Plataforma

#### Windows
- Usa el Registro de Windows como respaldo para configuraciones
- Soporta "modo portable" cuando `settings/Deskflow.conf` existe en el directorio de instalación
- El modo servicio requiere permisos elevados y usa el directorio ProgramData

#### macOS
- Requiere permisos de accesibilidad para crear archivos en ubicaciones protegidas
- Puede usar atributos de cuarentena en archivos descargados

#### Linux
- Sigue la especificación XDG Base Directory
- Respeta las variables de entorno `XDG_CONFIG_HOME` y `XDG_STATE_HOME`

## Permisos de Archivos

Todos los archivos y directorios creados usan permisos de usuario por defecto:
- Archivos de configuración: Solo lectura/escritura de usuario
- Archivos de registro: Solo lectura/escritura de usuario
- Certificados TLS y claves: Solo lectura/escritura de usuario (datos sensibles)

## Limpieza

Para eliminar completamente los datos de Deskflow:

### Windows
1. Eliminar: `%APPDATA%\Roaming\Deskflow\`
2. Eliminar: `C:\ProgramData\Deskflow\` (si existe)
3. Eliminar: `<ruta-instalación>\settings\` (si está en modo portable)
4. Eliminar claves del registro: `HKCU\Software\Deskflow\`

### macOS
1. Eliminar: `~/Library/Deskflow/`
2. Eliminar: `/Library/Deskflow/` (si existe)

### Linux
1. Eliminar: `~/.config/Deskflow/`
2. Eliminar: `$XDG_STATE_HOME/deskflow.state`
3. Eliminar: `/etc/Deskflow/` (si existe, requiere root)

## Función de Diagnóstico "Limpiar Configuración"

La aplicación proporciona una función de diagnóstico "Limpiar Configuración" que:
1. Elimina recursivamente todo el directorio de configuración
2. Recrea un directorio de configuración vacío
3. Para modo portable: Crea un archivo `Deskflow.conf` vacío

**Advertencia**: Esto elimina TODAS las configuraciones, ajustes, registros y certificados.

## Tabla Resumen

| Archivo/Directorio | Propósito | Auto-Creado | Específico de Plataforma |
|:--------------|:--------|:------------|:-----------------|
| `Deskflow.conf` | Configuración principal | Sí | La ruta varía |
| `deskflow-server.conf` | Configuración del servidor | Al guardar | No |
| `deskflow.state` | Estado de ejecución | Sí | Linux usa XDG |
| `*.log` | Archivos de registro | Cuando está habilitado | La ruta varía |
| Directorio `tls/` | Archivos TLS | Cuando TLS está habilitado | No |
| `Deskflow.pem` | Certificado SSL | Sí | No |
| `trusted-servers` | Huellas de servidores | Al confiar | No |
| `trusted-clients` | Huellas de clientes | Al confiar | No |

## Consideraciones de Seguridad

- **Certificados**: El archivo `Deskflow.pem` contiene tanto el certificado como la clave privada. Proteja este archivo con permisos apropiados.
- **Huellas Digitales**: Los archivos trusted-servers y trusted-clients previenen ataques de intermediario (man-in-the-middle). No los modifique manualmente a menos que entienda las implicaciones de seguridad.
- **Archivos de Registro**: Pueden contener información sensible. Configure los niveles de registro apropiadamente y asegure las ubicaciones de los archivos de registro.

## Referencias

Para más información sobre opciones de configuración, consulte:
- [Documentación de Configuración](configuration.md)
- Implementación de configuración: `src/lib/common/Settings.cpp`
- Utilidades TLS: `src/lib/gui/TlsUtility.cpp`
- Configuración del servidor: `src/lib/gui/config/ServerConfig.cpp`

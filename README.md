<div align="center">

# 🛍️ Floriend Vermond
### Enterprise Relational Database System — Oracle 21c XE

<br/>

<img src="https://img.shields.io/badge/Oracle-Database%2021c%20XE-F80000?style=for-the-badge&logo=oracle&logoColor=white" />
<img src="https://img.shields.io/badge/PL%2FSQL-Triggers%20%26%20Packages-4479A1?style=for-the-badge&logo=oracle&logoColor=white" />
<img src="https://img.shields.io/badge/Status-Completed-16A34A?style=for-the-badge" />
<img src="https://img.shields.io/badge/Version-1.0.0-2563EB?style=for-the-badge" />

<br/>

<img src="https://img.shields.io/badge/Universidad-Fid%C3%A9litas-1E3A8A?style=for-the-badge" />
<img src="https://img.shields.io/badge/SC--503-Administraci%C3%B3n%20de%20BD-065F46?style=for-the-badge" />
<img src="https://img.shields.io/badge/I%20Cuatrimestre-2026-111827?style=for-the-badge" />
<img src="https://img.shields.io/badge/Profesor-Michael%20Robles-7F1D1D?style=for-the-badge" />

<br/>

*Diseño e implementación completa de una base de datos relacional en Oracle para un sistema de e-commerce de moda, cubriendo modelado, seguridad, auditoría, encriptación, optimización y plan de respaldos.*

</div>
---
 
## 📑 Tabla de contenidos
 
- [Descripción](#-descripción)
- [Contexto del negocio](#-contexto-del-negocio)
- [Stack tecnológico](#-stack-tecnológico)
- [Arquitectura](#-arquitectura)
- [Modelo físico](#-modelo-físico--tablas)
- [Seguridad](#-seguridad-y-control-de-acceso)
- [Encriptación](#-encriptación-de-datos)
- [Auditoría](#-auditoría-unificada)
- [Optimización](#-índices-y-optimización)
- [Vistas materializadas](#-vistas-materializadas)
- [Triggers](#-triggers-plsql)
- [Respaldos RMAN](#-plan-de-respaldos-rman)
- [Monitoreo](#-plan-de-monitoreo)
- [Instalación](#-instalación-y-ejecución)
- [Equipo](#-equipo)
---
 
## 📌 Descripción
 
Este proyecto implementa una base de datos relacional completa para **Floriend Vermond**, una casa de moda costarricense que opera en e-commerce con prendas formales y casuales de marcas nacionales e internacionales.
 
La solución reemplaza un sistema desactualizado sin base de datos centralizada por una arquitectura Oracle que cubre desde el modelado lógico hasta la seguridad en producción.
 
| Pilar | Implementación |
|-------|---------------|
| 📐 Modelado | ERD con notación crow's foot + modelo relacional físico con 11 tablas |
| 🔐 Seguridad | Roles, perfiles, TDE AES-256, encriptación columnar DBMS_CRYPTO |
| 🔍 Auditoría | 5 políticas de auditoría unificada sobre eventos críticos del negocio |
| ⚡ Rendimiento | 14 índices B-tree, 4 vistas materializadas, reescritura de consultas |
| 💾 Respaldo | RMAN completo semanal + incremental diario, retención 30 días |
| 📊 Monitoreo | V$SESSION, V$SQLSTATS, DBA_SEGMENTS, buffer cache |
 
---
 
## 🏢 Contexto del negocio
 
### Problemas identificados
 
![Inventory](https://img.shields.io/badge/Problema-Inventario%20obsoleto-DC2626?style=flat-square)
![Orders](https://img.shields.io/badge/Problema-Monitoreo%20de%20pedidos%20nulo-DC2626?style=flat-square)
![Discounts](https://img.shields.io/badge/Problema-Descuentos%20manuales-DC2626?style=flat-square)
![Access](https://img.shields.io/badge/Problema-Sin%20control%20de%20accesos-DC2626?style=flat-square)
 
```
┌────────────────────────────────────────────────────────────────────┐
│  PROBLEMA                      IMPACTO                             │
├────────────────────────────────────────────────────────────────────┤
│  Inventario obsoleto           Sobreventas y pérdida de clientes   │
│  Monitoreo de pedidos nulo     Sin trazabilidad del ciclo completo │
│  Descuentos manuales           Errores en cobros a clientes        │
│  Accesos sin roles             Datos de admin expuestos            │
└────────────────────────────────────────────────────────────────────┘
```
 
---
 
## 🛠️ Stack tecnológico
 
## 🗄️ Base de datos
![Oracle](https://img.shields.io/badge/Oracle-Database%2021c%20XE-F80000?style=for-the-badge&logo=oracle&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-DDL%20%2F%20DML%20%2F%20DCL-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![PLSQL](https://img.shields.io/badge/PL%2FSQL-Packages%20%26%20Triggers-4479A1?style=for-the-badge)
 
## 🔐 Seguridad y cifrado
![TDE](https://img.shields.io/badge/TDE-AES--256%20Tablespace-065F46?style=for-the-badge)
![Crypto](https://img.shields.io/badge/DBMS__CRYPTO-AES--256%20Columnar-065F46?style=for-the-badge)
![Audit](https://img.shields.io/badge/Unified%20Audit-5%20Políticas-111827?style=for-the-badge)
 
## 🛠️ Herramientas
![SQLDeveloper](https://img.shields.io/badge/SQL%20Developer-IDE-F80000?style=for-the-badge&logo=oracle&logoColor=white)
![RMAN](https://img.shields.io/badge/RMAN-Backup%20%26%20Recovery-1E3A8A?style=for-the-badge)
![DBMS_STATS](https://img.shields.io/badge/DBMS__STATS-Query%20Optimizer-7F1D1D?style=for-the-badge)
 
---
 
## 🏗️ Arquitectura
 
### Esquemas del sistema
 
```
┌──────────────────────────────────────────────────────────────────────┐
│                       ORACLE DATABASE 21c XE                         │
│                                                                      │
│  ┌─────────────────────┐  ┌────────────────────┐  ┌──────────────┐  │
│  │  ESQ_DIMENSIONES    │  │ ESQ_TRANSACCIONAL  │  │  ESQ_ADMIN   │  │
│  │                     │  │                    │  │              │  │
│  │  DIM_UBICACION      │◄─│  PRODUCTO          │  │  PKG_SEGU-   │  │
│  │  DIM_CLIENTE        │◄─│  INVENTARIO        │  │  RIDAD       │  │
│  │  DIM_EMPLEADO       │◄─│  PEDIDO_HECHO      │  │              │  │
│  │  DIM_PROVEEDOR      │  │  DETALLE_PEDIDO    │  │  Roles y     │  │
│  │  DIM_MARCA          │◄─│  DEVOLUCION        │  │  perfiles    │  │
│  │  DIM_CATEGORIA      │◄─│  DEVOL_DETALLE     │  │              │  │
│  │  DIM_DESCUENTO      │◄─│                    │  │  Auditoría   │  │
│  └─────────────────────┘  └────────────────────┘  └──────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```
 
### Tablespaces
 
| Tablespace | Tipo | Tamaño | Propósito |
|-----------|:----:|:------:|-----------|
| `TS_FLORIEND_DATOS` | Permanente | 64 MB | Tablas dimensionales y transaccionales |
| `TS_FLORIEND_INDICES` | Permanente | 64 MB | Índices del sistema |
| `TS_FLORIEND_TEMPORAL` | Temporal | 64 MB | Operaciones temporales |
| `TS_USUARIOS_DATOS` | Permanente | 32 MB | Datos de usuarios operacionales |
| `TS_AUDIT_DATOS` | Permanente | 32 MB | Registros de auditoría |
 
---
 
## 🗂️ Modelo físico — tablas
 
### ESQ_DIMENSIONES
 
<details>
<summary><b>DIM_UBICACION</b></summary>
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| UBICACION_ID | NUMBER (Identity) | NO | PK |
| PAIS | VARCHAR2(50) | NO | — |
| PROVINCIA | VARCHAR2(50) | NO | — |
| CANTON | VARCHAR2(50) | NO | — |
| DISTRITO | VARCHAR2(50) | NO | — |
| CODIGO_POSTAL | VARCHAR2(10) | SÍ | — |
 
</details>
<details>
<summary><b>DIM_CLIENTE</b></summary>
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| CLIENTE_ID | NUMBER (Identity) | NO | PK |
| NOMBRE | VARCHAR2(100) | NO | — |
| APELLIDO | VARCHAR2(100) | NO | — |
| CORREO | VARCHAR2(150) | NO | UQ |
| TELEFONO | VARCHAR2(20) | SÍ | — |
| UBICACION_ID | NUMBER | NO | FK → DIM_UBICACION |
| FECHA_REGISTRO | DATE | NO | DEFAULT SYSDATE |
 
</details>
<details>
<summary><b>DIM_EMPLEADO</b></summary>
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| EMPLEADO_ID | NUMBER (Identity) | NO | PK |
| NOMBRE | VARCHAR2(100) | NO | — |
| APELLIDO | VARCHAR2(100) | NO | — |
| CORREO | VARCHAR2(150) | NO | UQ |
| CARGO | VARCHAR2(50) | NO | — |
| TELEFONO | VARCHAR2(20) | SÍ | — |
| UBICACION_ID | NUMBER | NO | FK → DIM_UBICACION |
| FECHA_INGRESO | DATE | NO | — |
 
</details>
<details>
<summary><b>DIM_PROVEEDOR</b></summary>
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| PROVEEDOR_ID | NUMBER (Identity) | NO | PK |
| NOMBRE | VARCHAR2(150) | NO | — |
| CORREO | VARCHAR2(150) | SÍ | — |
| TELEFONO | VARCHAR2(20) | SÍ | — |
| UBICACION_ID | NUMBER | NO | FK → DIM_UBICACION |
 
</details>
<details>
<summary><b>DIM_MARCA / DIM_CATEGORIA / DIM_DESCUENTO</b></summary>
**DIM_MARCA**
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| MARCA_ID | NUMBER (Identity) | NO | PK |
| NOMBRE | VARCHAR2(100) | NO | — |
| DESCRIPCION | VARCHAR2(255) | SÍ | — |
 
**DIM_CATEGORIA**
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| CATEGORIA_ID | NUMBER (Identity) | NO | PK |
| NOMBRE | VARCHAR2(100) | NO | — |
| DESCRIPCION | VARCHAR2(255) | SÍ | — |
 
**DIM_DESCUENTO**
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| DESCUENTO_ID | NUMBER (Identity) | NO | PK |
| NOMBRE | VARCHAR2(100) | NO | — |
| PORCENTAJE | NUMBER(5,2) | NO | CHK: 0.01–100 |
| FECHA_INICIO | DATE | NO | CHK válido |
| FECHA_FIN | DATE | NO | CHK >= INICIO |
 
</details>
### ESQ_TRANSACCIONAL
 
<details>
<summary><b>PRODUCTO / INVENTARIO</b></summary>
**PRODUCTO**
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| PRODUCTO_ID | NUMBER (Identity) | NO | PK |
| NOMBRE | VARCHAR2(150) | NO | — |
| DESCRIPCION | VARCHAR2(255) | SÍ | — |
| PRECIO | NUMBER(10,2) | NO | CHK > 0 |
| CANTIDAD_DISP | NUMBER | NO | CHK >= 0 |
| MARCA_ID | NUMBER | NO | FK → DIM_MARCA |
| CATEGORIA_ID | NUMBER | NO | FK → DIM_CATEGORIA |
| DESCUENTO_ID | NUMBER | SÍ | FK → DIM_DESCUENTO |
 
**INVENTARIO**
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| INVENTARIO_ID | NUMBER (Identity) | NO | PK |
| PRODUCTO_ID | NUMBER | NO | FK + UQ → PRODUCTO |
| CANTIDAD | NUMBER | NO | CHK >= 0 |
| FECHA_ULTIMO_MOV | DATE | NO | DEFAULT SYSDATE |
 
</details>
<details>
<summary><b>PEDIDO_HECHO / DETALLE_PEDIDO</b></summary>
**PEDIDO_HECHO**
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| PEDIDO_ID | NUMBER (Identity) | NO | PK |
| CLIENTE_ID | NUMBER | NO | FK → DIM_CLIENTE |
| EMPLEADO_ID | NUMBER | SÍ | FK → DIM_EMPLEADO |
| FECHA_PEDIDO | DATE | NO | DEFAULT SYSDATE |
| TOTAL | NUMBER(10,2) | NO | CHK > 0 |
 
**DETALLE_PEDIDO**
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| DETALLE_ID | NUMBER (Identity) | NO | PK |
| PEDIDO_ID | NUMBER | NO | FK → PEDIDO_HECHO |
| PRODUCTO_ID | NUMBER | NO | FK → PRODUCTO |
| CANTIDAD | NUMBER | NO | CHK > 0 |
| PRECIO_UNIT | NUMBER(10,2) | NO | CHK > 0 |
 
</details>
<details>
<summary><b>DEVOLUCION / DEVOLUCION_DETALLE</b></summary>
**DEVOLUCION**
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| DEVOLUCION_ID | NUMBER (Identity) | NO | PK |
| PEDIDO_ID | NUMBER | NO | FK → PEDIDO_HECHO |
| FECHA_DEVOLUCION | DATE | NO | DEFAULT SYSDATE |
| MOTIVO | VARCHAR2(255) | SÍ | — |
 
**DEVOLUCION_DETALLE**
| Columna | Tipo | Nulo | Restricción |
|---------|------|:----:|:-----------:|
| DEVDET_ID | NUMBER (Identity) | NO | PK |
| DEVOLUCION_ID | NUMBER | NO | FK → DEVOLUCION |
| PRODUCTO_ID | NUMBER | NO | FK → PRODUCTO |
| CANTIDAD | NUMBER | NO | CHK > 0 |
 
</details>
---
 
## 🔐 Seguridad y control de acceso
 
![MinPrivilege](https://img.shields.io/badge/Principio-Mínimo%20Privilegio-065F46?style=for-the-badge)
![Roles](https://img.shields.io/badge/Roles-3%20definidos-1E3A8A?style=for-the-badge)
![Profiles](https://img.shields.io/badge/Perfiles-3%20definidos-7F1D1D?style=for-the-badge)
 
### Perfiles de seguridad
 
| Parámetro | PERFIL_ADMIN | PERFIL_VENTAS | PERFIL_CONSULTA |
|-----------|:------------:|:-------------:|:---------------:|
| SESSIONS_PER_USER | 2 | 1 | 1 |
| FAILED_LOGIN_ATTEMPTS | 3 | 5 | 4 |
| PASSWORD_LIFE_TIME | 30 días | 60 días | 90 días |
| PASSWORD_LOCK_TIME | 1 día | 1 día | Por defecto |
 
### Usuarios operacionales
 
| Usuario | Cuota | Perfil | Rol |
|---------|:-----:|--------|-----|
| `ADMIN_FLORIEND` | 50 MB | PERFIL_ADMIN | ROL_ADMIN |
| `VENTAS_FLORIEND` | 20 MB | PERFIL_VENTAS | ROL_VENTAS |
| `CONSULTA_FLORIEND` | 10 MB | PERFIL_CONSULTA | ROL_CONSULTA |
 
```sql
-- ROL_VENTAS: operaciones de venta
GRANT CREATE SESSION                                    TO ROL_VENTAS;
GRANT SELECT  ON ESQ_DIMENSIONES.DIM_CLIENTE           TO ROL_VENTAS;
GRANT SELECT  ON ESQ_TRANSACCIONAL.PRODUCTO            TO ROL_VENTAS;
GRANT INSERT  ON ESQ_TRANSACCIONAL.PEDIDO_HECHO        TO ROL_VENTAS;
GRANT INSERT  ON ESQ_TRANSACCIONAL.DETALLE_PEDIDO      TO ROL_VENTAS;
GRANT UPDATE (CANTIDAD_DISP) ON ESQ_TRANSACCIONAL.PRODUCTO TO ROL_VENTAS;
 
-- ROL_CONSULTA: solo lectura
GRANT CREATE SESSION                                    TO ROL_CONSULTA;
GRANT SELECT  ON ESQ_DIMENSIONES.DIM_CLIENTE           TO ROL_CONSULTA;
GRANT SELECT  ON ESQ_TRANSACCIONAL.PRODUCTO            TO ROL_CONSULTA;
```
 
---
 
## 🔒 Encriptación de datos
 
![TDE](https://img.shields.io/badge/TDE-AES--256%20Tablespace-065F46?style=for-the-badge)
![Column](https://img.shields.io/badge/DBMS__CRYPTO-AES--256%20CBC-065F46?style=for-the-badge)
 
### TDE — Transparent Data Encryption
 
Aplicado sobre `TS_FLORIEND_DATOS`. Si alguien accede al archivo `.dbf` directamente sin pasar por Oracle, los datos son ilegibles.
 
```sql
ADMINISTER KEY MANAGEMENT SET KEY
  IDENTIFIED BY "WalletPass#2025!" WITH BACKUP CONTAINER = CURRENT;
 
ALTER TABLESPACE TS_FLORIEND_DATOS ENCRYPTION ONLINE ENCRYPT;
```
 
### Encriptación columnar — DBMS_CRYPTO
 
Paquete `ESQ_ADMIN.PKG_SEGURIDAD` con AES-256/CBC/PKCS5 para columnas sensibles de clientes.
 
| Función | Entrada → Salida | Descripción |
|---------|:----------------:|-------------|
| `ENCRIPTAR(p_dato)` | VARCHAR2 → RAW | Encripta texto plano con AES-256/CBC |
| `DESENCRIPTAR(p_enc)` | RAW → VARCHAR2 | Desencripta y retorna el texto original |
 
---
 
## 🔎 Auditoría unificada
 
![Policies](https://img.shields.io/badge/Políticas-5%20configuradas-1E3A8A?style=for-the-badge)
![Trail](https://img.shields.io/badge/UNIFIED__AUDIT__TRAIL-Activo-065F46?style=for-the-badge)
 
| Política | Acciones auditadas | Justificación |
|---------|-------------------|---------------|
| `AUDIT_SESIONES_FLORIEND` | LOGON / LOGOFF | Detecta accesos fuera de horario |
| `AUDIT_DML_CRITICO` | INSERT/UPDATE/DELETE en tablas críticas | Trazabilidad de datos sensibles |
| `AUDIT_SEGURIDAD_ADMIN` | ALTER USER, DROP USER, GRANT, REVOKE | Detecta escaladas de privilegios |
| `AUDIT_INVENTARIO` | ALL sobre INVENTARIO | Investiga discrepancias de stock |
| `AUDIT_DESCUENTOS` | INSERT/UPDATE/DELETE en DIM_DESCUENTO | Rastrea cambios en política de precios |
 
```sql
SELECT EVENT_TIMESTAMP, DBUSERNAME, ACTION_NAME,
       OBJECT_SCHEMA, OBJECT_NAME, RETURN_CODE
FROM   UNIFIED_AUDIT_TRAIL
WHERE  EVENT_TIMESTAMP > SYSDATE - 1
ORDER  BY EVENT_TIMESTAMP DESC;
```
 
---
 
## ⚡ Índices y optimización
 
![Indexes](https://img.shields.io/badge/Índices-14%20implementados-1E3A8A?style=for-the-badge)
![ExplainPlan](https://img.shields.io/badge/EXPLAIN%20PLAN-Validado-065F46?style=for-the-badge)
![Stats](https://img.shields.io/badge/DBMS__STATS-Actualizado-7F1D1D?style=for-the-badge)
 
| Índice | Tabla | Columna(s) | Tipo |
|--------|-------|-----------|:----:|
| IDX_CLIENTE_CORREO | DIM_CLIENTE | CORREO | B-tree |
| IDX_PEDIDO_FECHA | PEDIDO_HECHO | FECHA_PEDIDO | B-tree |
| IDX_PEDIDO_CLIENTE_FECHA | PEDIDO_HECHO | (CLIENTE_ID, FECHA_PEDIDO) | Compuesto |
| IDX_PEDIDO_ANIO_MES | PEDIDO_HECHO | TO_CHAR(FECHA_PEDIDO,'YYYY-MM') | Basado en función |
| IDX_PRODUCTO_PRECIO | PRODUCTO | PRECIO | B-tree |
| IDX_PRODUCTO_STOCK | PRODUCTO | CANTIDAD_DISP | B-tree |
| IDX_DETALLE_PED_PROD | DETALLE_PEDIDO | (PEDIDO_ID, PRODUCTO_ID) | Compuesto |
 
### Problemas resueltos
 
| Problema | Causa | Solución | Mejora |
|---------|-------|---------|:------:|
| Búsqueda por correo | Subconsulta → FULL SCAN | JOIN directo con IDX_CLIENTE_CORREO | ~85% menos lecturas |
| Ventas por mes | TO_CHAR impide índice B-tree | Rango de fechas >= y < | INDEX RANGE SCAN |
| Reportes analíticos | JOINs en 4-5 tablas por consulta | Vistas materializadas | Elimina recálculo |
| Stock crítico | FULL SCAN en CANTIDAD_DISP | IDX_PRODUCTO_STOCK | INDEX RANGE SCAN |
 
```sql
EXEC DBMS_STATS.GATHER_SCHEMA_STATS('ESQ_DIMENSIONES',   CASCADE => TRUE);
EXEC DBMS_STATS.GATHER_SCHEMA_STATS('ESQ_TRANSACCIONAL', CASCADE => TRUE);
```
 
---
 
## 📊 Vistas materializadas
 
![MVs](https://img.shields.io/badge/Vistas%20Materializadas-4%20implementadas-1E3A8A?style=for-the-badge)
![Refresh](https://img.shields.io/badge/Refresh-ON%20DEMAND-7F1D1D?style=for-the-badge)
![Rewrite](https://img.shields.io/badge/Query%20Rewrite-ENABLED-065F46?style=for-the-badge)
 
| Vista | Esquema | Propósito |
|-------|---------|-----------|
| `MV_VENTAS_MES_CATEGORIA` | ESQ_TRANSACCIONAL | Ventas mensuales por categoría — elimina JOINs sobre 4 tablas |
| `MV_TOP_PRODUCTOS` | ESQ_TRANSACCIONAL | Ranking por unidades vendidas e ingresos totales |
| `MV_CLIENTES_VIP` | ESQ_DIMENSIONES | Métricas de compra por cliente para campañas de fidelización |
| `MV_INVENTARIO_CATEGORIA` | ESQ_TRANSACCIONAL | Stock consolidado por categoría |
 
---
 
## ⚙️ Triggers PL/SQL
 
![Triggers](https://img.shields.io/badge/Triggers-3%20implementados-1E3A8A?style=for-the-badge)
![StockControl](https://img.shields.io/badge/Control-Stock%20automático-065F46?style=for-the-badge)
 
| Trigger | Evento | Función |
|---------|--------|---------|
| `TRG_VALIDA_STOCK` | BEFORE INSERT en DETALLE_PEDIDO | Verifica stock — lanza ORA-20001 si no hay disponibilidad |
| `TRG_ACTUALIZA_STOCK` | AFTER INSERT en DETALLE_PEDIDO | Descuenta CANTIDAD_DISP y actualiza INVENTARIO |
| `TRG_RESTAURA_STOCK` | AFTER INSERT en DEVOLUCION | Restaura stock parcial o total según tipo de devolución |
 
---
 
## 💾 Plan de respaldos RMAN
 
![RMAN](https://img.shields.io/badge/Oracle-RMAN-F80000?style=for-the-badge&logo=oracle&logoColor=white)
![RPO](https://img.shields.io/badge/RPO-24%20horas-1E3A8A?style=for-the-badge)
![RTO](https://img.shields.io/badge/RTO-%3C%202%20horas-065F46?style=for-the-badge)
![Retention](https://img.shields.io/badge/Retención-30%20días-7F1D1D?style=for-the-badge)
 
| Job | Tipo | Frecuencia | Hora |
|-----|:----:|-----------|:----:|
| `01_backup_full` | Completo | Domingo | 00:00 |
| `02_backup_incremental` | Incremental Nivel 1 | Lunes–Sábado | 02:00 |
 
### Estrategias de recuperación
 
```
┌────────────────────────────────────────────────────────────────────┐
│  ESCENARIO                  ESTRATEGIA                             │
├────────────────────────────────────────────────────────────────────┤
│  Falla catastrófica         Backup completo + archive logs         │
│  Falla de tablespace        Offline → restaurar → recuperar solo   │
│  Corrupción de datos        Point-in-time recovery con archive log │
└────────────────────────────────────────────────────────────────────┘
```
 
---
 
## 📡 Plan de monitoreo
 
### Disponibilidad
 
```sql
SELECT INSTANCE_NAME, STATUS, STARTUP_TIME FROM V$INSTANCE;
 
SELECT t.TABLESPACE_NAME,
       ROUND((d.USED_BYTES / d.TOTAL_BYTES) * 100, 2) AS PCT_USED
FROM   DBA_TABLESPACES t
JOIN  (SELECT TABLESPACE_NAME,
              SUM(BYTES) AS TOTAL_BYTES,
              SUM(BYTES) - SUM(FREE_SPACE) AS USED_BYTES
       FROM   DBA_FREE_SPACE GROUP BY TABLESPACE_NAME) d
ON    t.TABLESPACE_NAME = d.TABLESPACE_NAME
WHERE (d.USED_BYTES / d.TOTAL_BYTES) > 0.80;
```
 
### Desempeño
 
```sql
-- Buffer Cache Hit Ratio (objetivo: > 90%)
SELECT ROUND((1 - (phy.VALUE / (cur.VALUE + con.VALUE))) * 100, 2) AS HIT_RATIO_PCT
FROM   V$SYSSTAT phy, V$SYSSTAT cur, V$SYSSTAT con
WHERE  phy.NAME = 'physical reads'
AND    cur.NAME = 'db block gets'
AND    con.NAME = 'consistent gets';
 
-- Top 10 consultas por CPU
SELECT SQL_ID, CPU_TIME/1000000 AS CPU_SEG, EXECUTIONS,
       SUBSTR(SQL_TEXT, 1, 80) AS CONSULTA
FROM   V$SQLSTATS
ORDER  BY CPU_TIME DESC FETCH FIRST 10 ROWS ONLY;
```
 
---
 
## 🚀 Instalación y ejecución
 
### Requisitos previos
 
![Oracle](https://img.shields.io/badge/Oracle-Database%2021c%20XE-F80000?style=flat-square&logo=oracle&logoColor=white)
![SQLDev](https://img.shields.io/badge/SQL%20Developer-Recomendado-F80000?style=flat-square)
![Privileges](https://img.shields.io/badge/Privilegios-SYSDBA-1E3A8A?style=flat-square)
 
### Pasos
 
**1. Conectarse como SYSDBA**
```sql
CONNECT sys as sysdba;
```
 
**2. Verificar que la instancia esté activa**
```sql
SELECT INSTANCE_NAME, STATUS FROM V$INSTANCE;
```
 
**3. Ejecutar el script principal**
```sql
@SC_503_SQL_G7.sql
```
 
El script ejecuta en orden:
 
```
1. Tablespaces              6. Roles y usuarios operacionales
2. Esquemas y usuarios      7. Políticas de auditoría
3. Tablas dimensionales     8. TDE + DBMS_CRYPTO
4. Tablas transaccionales   9. Triggers PL/SQL
5. Índices                 10. Vistas materializadas + datos de prueba
```
 
**4. Verificar instalación**
```sql
SELECT OWNER, OBJECT_NAME, OBJECT_TYPE
FROM   DBA_OBJECTS
WHERE  OWNER IN ('ESQ_DIMENSIONES','ESQ_TRANSACCIONAL','ESQ_ADMIN')
AND    OBJECT_TYPE = 'TABLE'
ORDER  BY OWNER, OBJECT_NAME;
```
 
> ⚠️ No ejecutar secciones del script de forma aislada — el orden respeta las dependencias de claves foráneas del modelo relacional.
 
---
 
## 🗃️ Datos de prueba incluidos
 
| Tabla | Registros | Descripción |
|-------|:---------:|-------------|
| DIM_UBICACION | 60 | CR, Panamá, Colombia, México, España, Italia, Francia, EE.UU. |
| DIM_CLIENTE | 60 | Clientes costarricenses con datos de contacto |
| DIM_EMPLEADO | 20 | Personal Floriend (admin, ventas, logística, RRHH) |
| DIM_PROVEEDOR | 15 | Proveedores nacionales e internacionales |
| DIM_MARCA | 10 | Floriend Home, Tico Style, Euro Chic, Urban CR, etc. |
| DIM_CATEGORIA | 8 | Formal, Casual, Deportivo, Playero, Fiesta, Juvenil, Ejecutivo, Eco-Friendly |
| DIM_DESCUENTO | 10 | Black Friday, Navidad, Bienvenida, etc. |
| PRODUCTO | 60 | Prendas formales, casuales, deportivas y eco-friendly |
| INVENTARIO | 60 | Un registro por producto |
| PEDIDO_HECHO | 55 | Pedidos enero 2025 – enero 2026 |
| DETALLE_PEDIDO | 80 | Líneas con producto, cantidad y precio |
| DEVOLUCION | 15 | Devoluciones con motivos reales |
 
---
 
## 📁 Estructura del repositorio
 
```
floriend-vermond-db/
│
├── SC_503_SQL_G7.sql              # Script SQL completo
├── SC_503_Informe_G7_Final.pdf    # Informe técnico del proyecto
└── README.md
```
 
---
 
## 👥 Equipo
 
| Nombre | Universidad |
|--------|:-----------:|
| Carlos Steven Avilés Roque | Universidad Fidélitas |
| Allan Fauricio Fonseca Batista | Universidad Fidélitas |
| Jimena Rivera Sancho | Universidad Fidélitas |
| Isaac Andrey Sánchez Delgado | Universidad Fidélitas |
 
---
 
<div align="center">

<img src="https://img.shields.io/badge/Universidad-Fid%C3%A9litas-1E3A8A?style=for-the-badge" />
<img src="https://img.shields.io/badge/Facultad-Ingenier%C3%ADa%20en%20Sistemas-065F46?style=for-the-badge" />
<img src="https://img.shields.io/badge/2026-I%20Cuatrimestre-111827?style=for-the-badge" />

</div>

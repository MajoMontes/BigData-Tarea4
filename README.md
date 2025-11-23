## 📚 Sistema de Gestión de Incidencias en Soporte Técnico – Base de Datos en MongoDB

### 📘 1. Descripción del Caso de Estudio

El caso aborda el diseño de un sistema para registrar y gestionar incidencias reportadas por los empleados de una organización. Las incidencias pueden corresponder a problemas de hardware, fallos de software, solicitudes de mantenimiento o inconvenientes en la red. MongoDB se selecciona debido a su capacidad para almacenar descripciones de diferentes longitudes, estados de seguimiento variables y prioridades configurables. El sistema permite registrar el ticket de cada incidente, identificar al empleado que reporta, clasificar la categoría y actualizar el estado según avance la solución. Esto facilita consultas como incidencias abiertas, casos por prioridad y análisis de recurrencia por áreas.

---

## 2. Estructura de la Base de Datos

#### Nombre de la base de datos
`soporte_db`

#### 🗂 Colecciones principales

La base de datos está compuesta por dos colecciones esenciales:

---

#### **1. Colección: empleados**

| Campo   | Tipo      | Descripción                           |
|---------|-----------|---------------------------------------|
| _id     | ObjectId  | Identificador único generado por MongoDB |
| nombre  | string    | Nombre del empleado                   |
| area    | string    | Departamento o área a la que pertenece |
| email   | string    | Correo institucional                  |

---

#### 2. **Colección: incidencias**

| Campo        | Tipo      | Descripción                                      |
|--------------|-----------|--------------------------------------------------|
| _id          | ObjectId  | Identificador único                              |
| ticket_id    | string    | Código del ticket                                |
| empleado_id  | ObjectId  | Referencia al empleado que reporta el incidente  |
| categoria    | string    | Tipo de incidencia (hardware, red, software, etc.) |
| descripcion  | string    | Detalle del problema                             |
| estado       | string    | Abierto / En proceso / Cerrado                   |
| fecha_creado | date      | Fecha de registro                                |
| prioridad    | string    | Alta / Media / Baja                              |

---

## 3. Creación de la Base de Datos

🖥  La creación de la base de datos se realizó empleando:

- **MongoDB 8.2**
- **MongoDB Compass**

#### 📌 Proceso general
1. Creación de la base de datos: 
`soporte_db`

2. Creación de las colecciones:  
   - `empleados`  
   - `incidencias`
   
3. Importación de los archivos JSON mediante:  
   **Collection → Import Data → JSON File**

![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/4f369a0c535b789cd314dab36330b4ca44540f20/Evidencia/Base%20de%20Datos/Base%20de%20tados.png)
---

## 4. Importación de Datos

#### Colección: empleados
Se importaron datos de empleados pertenecientes a diferentes áreas de la organización, permitiendo relacionar incidencias según el departamento afectado.

![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Base%20de%20Datos/Import%20empleados.png)

#### Colección: incidencias
Se importaron tickets de soporte con información completa sobre categoría, estado, prioridad y fecha de creación.

![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Base%20de%20Datos/Import%20incidencias.png)

Estos datos permiten realizar análisis estadísticos y pruebas funcionales dentro del sistema.

---

## 5. Consultas Realizadas en MongoDB

A continuación se presentan las consultas realizadas, clasificadas por su tipo y acompañadas de su correspondiente explicación académica.

---

### Consultas Básicas

#### **1.  	Insertar nuevo empleado**

```js
// Insertar nuevo empleado
db.empleados.insertOne({
    "nombre": "Laura Sánchez",
    "area": "Contabilidad",
    "email": "laura.sanchez@empresa.com"
})
```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20b%C3%A1sicas/Consulta1.png)

`**Explicación:** Crea un nuevo documento en la colección "empleados" con los datos especificados. MongoDB genera automáticamente un _id único para identificar al nuevo empleado.

---
#### ** 2.	Seleccionar incidencias con límite**

```js
// Seleccionar incidencias con límite
db.incidencias.find().limit(5)
```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20b%C3%A1sicas/Consulta2.png)

`**Explicación:** Recupera solo los primeros 5 documentos de la colección "incidencias".
- Cómo funciona: find() obtiene todos los documentos y limit(5) restringe el resultado a 5. Revisar rápidamente una muestra de incidencias sin cargar toda la base.

---
#### ** 3.	Actualizar estado de una incidencia**

```js
// Actualizar estado de una incidencia
db.incidencias.updateOne(
    {"ticket_id": "TKT-001"},
    {$set: {"estado": "Cerrado"}}
)

```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20b%C3%A1sicas/Consulta3.png)

`**Explicación:** Modifica únicamente el campo "estado" del ticket TKT-001 a "Cerrado".
- Cómo funciona: Busca el documento con ticket_id: "TKT-001" y actualiza solo el campo especificado. Marcar incidencias como resueltas cuando finaliza el soporte.

---
#### ** 4.	Eliminar empleados de un área específica**

```js
// Eliminar empleados de un área específica
db.empleados.deleteMany({"area": "Legal"})

```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20b%C3%A1sicas/Consulta4.png)

`**Explicación:** Elimina todos los empleados que pertenecen al área Legal.
- Cómo funciona: Busca todos los documentos donde área: "Legal" y los elimina permanentemente. Limpiar la base cuando un departamento completo se da de baja.

---
### Consultas con Filtros y Operadores

#### **1.	Incidencias con prioridad Alta (Filtro Básico)**

```js
// Incidencias con prioridad Alta
db.incidencias.find({"prioridad": "Alta"})
```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20con%20filtros%20y%20operadores/consulta1.png)

`**Explicación:** Filtra y muestra solo las incidencias marcadas como prioridad "Alta". 
- Como funciona: Lista todos los tickets urgentes que requieren atención inmediata.  Sirve para que el equipo de soporte priorice los casos más críticos.


---
#### ** 2.	Incidencias creadas después del 1 de junio 2024 (Operador de comparación)**

```js
// Incidencias creadas después del 1 de junio de 2024
db.incidencias.find({
    "fecha_creado": {$gte: new Date("2024-06-01")}
})
```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20con%20filtros%20y%20operadores/consulta2.png)

`**Explicación:** Busca incidencias reportadas desde el 1 de junio de 2024 en adelante.
- Operador: $gte significa "greater than or equal" (mayor o igual que). Es útil para analizar incidencias recientes o hacer reportes mensuales.


---
#### ** 3.	Incidencias abiertas o en proceso con prioridad Alta (Operadores lógicos ($and, $or)**

```js
// Incidencias abiertas o en proceso con prioridad Alta
db.incidencias.find({
    $and: [
        {"prioridad": "Alta"},
        {$or: [
            {"estado": "Abierto"},
            {"estado": "En proceso"}
        ]}
    ]
})

```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20con%20filtros%20y%20operadores/consulta3.png)

`**Explicación:** Encuentra incidencias urgentes que aún no han sido cerradas.
- Cómo funciona: Combina $and (debe cumplir ambas condiciones) con $or (puede tener cualquiera de estos estados). Es práctico en seguimiento de casos críticos pendientes de resolver.

---
#### ** 4.	Empleados de ventas o marketing (Operador $in)**

```js
// Empleados de Ventas o Marketing
db.empleados.find({
    "area": {$in: ["Ventas", "Marketing"]}
})

```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20con%20filtros%20y%20operadores/consulta4.png)

`**Explicación:** Selecciona empleados que pertenecen a Ventas O Marketing.
- Operador: $in verifica si el valor está en una lista de opciones.
Se utiliza para filtrar personal por departamentos específicos para reportes o comunicaciones.

---
### Consultas de agregación para calcular estadísticas

#### **1.	Contar Incidencias por categoría (Conteo y agrupación)**

```js
// Contar incidencias por categoría
db.incidencias.aggregate([
    {$group: {
        _id: "$categoria",
        total: {$sum: 1}
    }}
])
```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20de%20agregaci%C3%B3n%20para%20calcular%20estad%C3%ADsticas/consulta1.png)

`**Explicación:** Agrupa las incidencias por tipo (hardware, software, etc.) y cuenta cuántas hay de cada una.
- Cómo funciona: Muestra qué categoría tiene más tickets reportados. Identifica áreas problemáticas en la organización.

---
#### ** 2.	Incidencias por área del empleado (Estadísticas por áreas)**

```js
// Incidencias por área del empleado
db.incidencias.aggregate([
    {
        $lookup: {
            from: "empleados",
            localField: "empleado_id",
            foreignField: "_id",
            as: "empleado_info"
        }
    },
    {$unwind: "$empleado_info"},
    {$group: {
        _id: "$empleado_info.area",
        total_incidencias: {$sum: 1},
        promedio_prioridad_alta: {
            $avg: {
                $cond: [{$eq: ["$prioridad", "Alta"]}, 1, 0]
            }
        }
    }},
    {$sort: {"total_incidencias": -1}}
])
```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20de%20agregaci%C3%B3n%20para%20calcular%20estad%C3%ADsticas/consulta2.png)

`**Explicación:** Al cruzar los datos de incidencias con la información de empleados, esta consulta identifica qué departamentos generan mayor volumen de tickets y con qué nivel de urgencia. Los resultados muestran patrones específicos por área, revelando si ciertos departamentos enfrentan problemas técnicos recurrentes o si existen brechas de capacitación. Un alto porcentaje de prioridades altas en un área específica puede señalar problemas críticos que afectan la productividad. Este análisis facilita la toma de decisiones para implementar soluciones dirigidas, como capacitación especializada, mejora de equipos o reasignación de recursos de soporte hacia las áreas más críticas.

---
#### ** 3.	Incidencias por mes (Análisis de tendencias temporales)**

```js
// Incidencias por mes
db.incidencias.aggregate([
    {
        $group: {
            _id: {
                $month: "$fecha_creado"
            },
            total: {$sum: 1},
            abiertas: {
                $sum: {$cond: [{$eq: ["$estado", "Abierto"]}, 1, 0]}
            },
            cerradas: {
                $sum: {$cond: [{$eq: ["$estado", "Cerrado"]}, 1, 0]}
            }
        }
    },
    {$sort: {"_id": 1}}
])

```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20de%20agregaci%C3%B3n%20para%20calcular%20estad%C3%ADsticas/consulta3.png)

`**Explicación:** El análisis temporal de incidencias por mes permite identificar tendencias estacionales y medir la eficiencia del equipo de soporte. Al comparar los tickets abiertos versus cerrados en cada mes, se evalúa la capacidad de respuesta y la efectividad en la resolución de problemas. Los picos en ciertos meses pueden relacionarse con factores específicos como implementación de nuevos sistemas, aumento de carga laboral o problemas estacionales. Esta información es valiosa para la planificación anticipada de recursos, establecimiento de metas realistas y mejora continua de los procesos de soporte técnico.

---
#### ** 4.	Empleados con más incidencias reportadas**

```js
// Empleados con más incidencias reportadas
db.incidencias.aggregate([
    {
        $lookup: {
            from: "empleados",
            localField: "empleado_id",
            foreignField: "_id",
            as: "empleado_info"
        }
    },
    {$unwind: "$empleado_info"},
    {$group: {
        _id: {
            empleado_id: "$empleado_id",
            nombre: "$empleado_info.nombre",
            area: "$empleado_info.area"
        },
        total_incidencias: {$sum: 1},
        incidencias_abiertas: {
            $sum: {$cond: [{$eq: ["$estado", "Abierto"]}, 1, 0]}
        }
    }},
    {$sort: {"total_incidencias": -1}},
    {$limit: 10}
])

```

** Vizualización de resultado**
![imagen](https://github.com/MajoMontes/BigData-Tarea4/blob/85979c7ba96fd1617cdc4aa75c4c2c285bacbe98/Evidencia/Consultas%20de%20agregaci%C3%B3n%20para%20calcular%20estad%C3%ADsticas/consulta4.png)

`**Explicación:** Identificar a los empleados que reportan mayor cantidad de incidencias permite un enfoque proactivo en la gestión del soporte. Este análisis distingue entre usuarios con problemas genuinamente recurrentes y aquellos que podrían necesitar capacitación adicional. Un alto número de incidencias abiertas sugiere problemas no resueltos que requieren atención prioritaria. La información obtenida facilita la implementación de medidas preventivas, como mantenimiento específico de equipos, sesiones de entrenamiento personalizado o revisión de procesos particulares que afectan a usuarios frecuentes, mejorando así la experiencia general y reduciendo la carga de trabajo del soporte.

---
### 📌 Conclusión

La realización de esta fase permitió aplicar de forma práctica los fundamentos de MongoDB en un caso de uso real. Se diseñó una base de datos NoSQL adecuada al escenario seleccionado, definiendo colecciones, documentos y campos conforme a las necesidades del sistema. Posteriormente, se implementó la base de datos en MongoDB, incorporando más de 100 documentos y ejecutando operaciones esenciales como inserción, actualización, eliminación y consultas avanzadas mediante filtros y operadores. Además, el uso de pipelines de agregación permitió obtener estadísticas relevantes y analizar el comportamiento de los datos. En conjunto, esta fase fortaleció la comprensión del modelado NoSQL y evidenció la eficiencia de MongoDB para manejar información flexible y orientada a consultas dinámicas.

####✒️ Autora

- María José Montes

####✒️ Información Académica 
- Programa: Ingeniería de Sistemas – UNAD  
- Curso: Big Data 
- Código 202016911
- 2025

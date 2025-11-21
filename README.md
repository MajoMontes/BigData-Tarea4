## Base de Datos de Catálogo de Comercio Electrónico (MongoDB)

#### Visión General del Proyecto
Este repositorio contiene la implementación de una base de datos desarrollada en MongoDB, orientada a la gestión de productos, clientes y pedidos dentro de un entorno de comercio electrónico.  
El objetivo principal es modelar la estructura funcional de un sistema comercial digital, aplicando procesos de creación, importación y consulta de datos mediante MongoDB Compass.

---

## 1. Caso de Estudio

#### 📘 Descripción del Caso
El caso de estudio plantea el desarrollo de un sistema básico de comercio electrónico enfocado en la administración de:

- **Productos** disponibles en el catálogo  
- **Clientes** registrados en la plataforma  
- **Pedidos** generados por los usuarios  

Este modelo permite realizar operaciones como:

- Búsqueda de productos por SKU, nombre y categoría  
- Filtrado por rangos de precios  
- Consulta de inventarios  
- Identificación de clientes por país u otros atributos  
- Revisión de pedidos y sus valores  
- Obtención de estadísticas básicas mediante agregaciones  

El propósito es representar de forma clara, organizada y eficiente el funcionamiento esencial de un sistema de e-commerce.

---

## 2. Estructura de la Base de Datos

#### 🏷 Nombre de la base de datos
`ecommerce_db`

#### 🗂 Colecciones principales
La base de datos está compuesta por tres colecciones:

---

#### **1. Colección: productos**
![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/fe29c6b7406b8c9817f844d9b646cfae7a4972f8/Evidencia/EstructuraProductos.png)

---

#### **2. Colección: clientes**
![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/67e7048692198b155510130224480d0867222593/Evidencia/EstructuraCliente.png)

---

#### **3. Colección: pedidos**
![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/fe29c6b7406b8c9817f844d9b646cfae7a4972f8/Evidencia/EstructuraPedidos.png)

---

## 3. Creación de la Base de Datos

#### 🖥 Herramientas Utilizadas
- **MongoDB 8.2**  
- **MongoDB Compass**

#### 📌 Pasos realizados
1. Creación de la base de datos denominada:  
   `ecommerce_db`
2. Creación de las colecciones:  
   - productos  
   - clientes  
   - pedidos  
3. Importación de los documentos JSON mediante MongoDB Compass utilizando:  
   **Collection → Import Data → JSON File**

Cada archivo JSON importado se encuentra dentro de este repositorio.

![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/67e7048692198b155510130224480d0867222593/Evidencia/Base%20de%20Datos.png)

---

## 4. Importación de Datos

#### 1. Colección: productos  
Se importó un archivo JSON con información del catálogo de productos, incluyendo precios, categorías y existencias.

![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/67e7048692198b155510130224480d0867222593/Evidencia/Productos.png)

#### 2. Colección: clientes  
Se importó un archivo JSON con los datos de los clientes registrados, incluyendo información personal y de contacto.

![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/67e7048692198b155510130224480d0867222593/Evidencia/Clientes.png)

#### 3. Colección: pedidos  
Se importó un archivo JSON con los pedidos realizados por los clientes, cada uno con sus productos asociados y valor total.

![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/67e7048692198b155510130224480d0867222593/Evidencia/Pedidos.png)

Todos los archivos JSON utilizados están organizados dentro de la carpeta correspondiente en este repositorio.

---

## 5. 📌 Consultas Realizadas en MongoDB

---


#### **1. Filtrar productos por rango de precios**

`db.productos.find({ precio: { $gte: 50000, $lte: 200000 } });`

![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/67e7048692198b155510130224480d0867222593/Evidencia/Consulta1.png)

`**Explicación:** Esta consulta busca todos los productos cuyo precio esté entre 50.000 y 200.000, incluyendo ambos valores.
- $gte significa mayor o igual que.
- $lte significa menor o igual que.

Es útil cuando quieres mostrar productos dentro de un presupuesto específico.

---

#### **2.	Buscar Productos con Stock menor a 20**

`db.productos.find({ cantidad: { $lt: 20 } });`

![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/67e7048692198b155510130224480d0867222593/Evidencia/Consulta2.png)

`**Explicación:** Esta consulta devuelve los productos cuya cantidad (o stock) disponible es menor a 20.
- $lt significa menor que.

Sirve para identificar productos que están próximos a agotarse.

---

#### **3. Buscar Clientes de Colombia**

`db.clientes.find({ pais: "Colombia" });`

![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/67e7048692198b155510130224480d0867222593/Evidencia/Consulta3.png)

`**Explicación:** Busca todos los documentos en la colección clientes donde el campo pais sea exactamente "Colombia".
No usa operadores porque es una comparación directa.

---

#### **4.	Buscar Pedidos con valor total mayor a 100000**

`db.pedidos.find({ valor_total: { $gt: 100000 } });`

![image alt](https://github.com/MajoMontes/BigData-Tarea4/blob/67e7048692198b155510130224480d0867222593/Evidencia/C4.png)

`**Explicación:** Devuelve todos los pedidos cuyo valor_total es mayor que 100.000.
- $gt significa mayor que.

Es útil cuando deseas encontrar compras grandes o de alto valor.

---
### Conclusión
Este proyecto implementa un modelo básico pero funcional de un sistema de comercio electrónico utilizando MongoDB. Incluye creación de base de datos, importación de datos, consultas con operadores y estructura completa de productos, clientes y pedidos, reflejando el funcionamiento principal de un catálogo digital.



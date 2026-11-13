# UNIDAD-3-BASE-DE-DATOS-EJERCICIO-2
El diagrama muestra un Modelo Relacional (MR) para la gestión de un sistema aerocomercial o de aerolínea, compuesto por 6 tablas interconectadas que administran la información de aviones, vuelos, tripulación, reservas y pasajeros.

A continuación, se detalla la estructura y las relaciones de cada tabla:

1. Tablas y Estructura
AVION

NUMERO_MATRICULA (PK)

AÑO_FABRICACION

MODELO

AUTONOMIA

CAPACIDAD_PASAJEROS

VUELO

ID_VUELO (PK)

HORA_LLEGADA

ORIGEN

DESTINO

HORA_SALIDA

NUMERO_MATRICULA (FK) — Relacionado con la tabla AVION

ASIGNACION_VUELO (Tabla intermedia / asociativa entre Vuelo y Empleado)

ID_VUELO (PK / FK)

ROL

NUMERO_EMPLEADO (PK / FK)

EMPLEADO

NUMERO_EMPLEADO (PK)

FECHA_INGRESO

CATEGORIA_PROFESIONAL

DNI

NOMBRE

RESERVA (Tabla intermedia / asociativa entre Vuelo y Pasajero)

ID_RESERVA (PK)

NUMERO_ASIENTO

ESTADO

ID_VUELO (PK / FK)

ID_PASAJERO (PK / FK)

PASAJERO

ID_PASAJERO (PK)

NOMBRE

NACIONALIDAD

DNI

FECHA_NACIMIENTO

2. Relaciones Principales
Avión y Vuelo: Un avión (identificado por su matrícula) puede realizar múltiples vuelos a lo largo del tiempo, mientras que un vuelo específico es operado por un único avión (relación de uno a muchos).

Vuelo y Empleado (Asignación): Existe una relación de muchos a muchos resuelta por la tabla ASIGNACION_VUELO, lo que permite que un vuelo cuente con varios empleados asignados (tripulación) y que un empleado participe en múltiples vuelos desempeñando un rol determinado.

Vuelo, Pasajero y Reserva: La tabla RESERVA actúa como una entidad asociativa de muchos a muchos entre VUELO y PASAJERO. Permite registrar qué pasajeros han reservado asientos en determinados vuelos, especificando el número de asiento asignado y el estado de dicha reserva.
<img width="1939" height="709" alt="EJERCICIO 2-MR drawio" src="https://github.com/user-attachments/assets/b67c8e1c-2e81-4825-b24f-729e370ab30a" />

La imagen muestra el proceso completo de **normalización de bases de datos** aplicado de forma secuencial —desde una tabla inicial desnormalizada hasta alcanzar la Tercera Forma Normal (3NF)— para un sistema de gestión de vuelos, reservas, aviones y tripulación.

A continuación, se detalla lo que se realiza en cada fase representada en el esquema:

---

### 1. Fase de Normalización N°0 (Tabla Desnormalizada)

* **Estado inicial:** Se parte de una **única tabla grande y plana** que agrupa toda la información de forma redundante (datos de reservas, asientos, estados, detalles de vuelos, horarios, origen, destino, especificaciones de los aviones, datos personales de los pasajeros, datos de los empleados y roles asignados).
* **Problema:** Presenta una fuerte redundancia de datos (por ejemplo, los datos de un avión o los atributos de un empleado se repiten masivamente en cada fila donde participa), lo que genera anomalías en las operaciones de inserción, actualización y borrado.

### 2. Primera Forma Normal (1NF)

* **Condición:** Se exige que todos los atributos contengan valores atómicos (indivisibles) y que cada tabla disponga de una clave primaria identificativa.
* **Proceso aplicado:** La tabla original se desglosa verticalmente para separar los datos que pertenecen a entidades independientes y eliminar los grupos repetitivos:
* **TABLA PASAJERO:** Extrae la información individual de cada pasajero utilizando `ID_PASAJERO` como Clave Primaria (PK).
* **TABLA EMPLEADO:** Agrupa los datos del personal de la aerolínea con `NUMERO_EMPLEADO` como PK.
* *(Nota: En esta etapa transitoria, el resto de los datos operativos y de vuelo aún se procesan en estructuras preliminares antes de aislar completamente las dependencias).*



### 3. Segunda Forma Normal (2NF)

* **Condición:** La base de datos debe estar en 1NF y **todos los atributos que no forman parte de la clave deben depender por completo de toda la clave primaria** (eliminando dependencias parciales en tablas con claves compuestas).
* **Proceso aplicado:** Se aíslan las tablas relacionales y asociativas clave:
* **TABLA RESERVA:** Relaciona los datos específicos de la reserva (asiento, estado) utilizando una clave compuesta o identificadores clave entre el vuelo y el pasajero.
* **TABLA ASIGNACION_VUELO:** Se establece como una entidad intermedia compuesta por `ID_VUELO` y `NUMERO_EMPLEADO` (ambas PK/FK) junto con el atributo `ROL`, eliminando dependencias parciales respecto a la tripulación.



### 4. Tercera Forma Normal (3NF)

* **Condición:** Debe estar en 2NF y **no poseer dependencias transitivas** (es decir, ningún atributo que no sea clave debe depender de otro atributo que tampoco sea clave; todo atributo no clave debe depender exclusivamente de la clave primaria).
* **Proceso aplicado:** Se estructuran de manera definitiva y normalizada todas las entidades del sistema aerocomercial:
* **TABLA RESERVA:** Gestiona limpiamente las reservas vinculando `ID_RESERVA` (PK) con el vuelo y el pasajero correspondiente.
* **TABLA VUELO:** Contiene exclusivamente los datos propios del vuelo (`ID_VUELO` como PK, horarios, origen, destino) y vincula el avión mediante `NUMERO_MATRICULA` como clave foránea (FK).
* **TABLA PASAJERO:** Mantiene de forma independiente los datos personales de los pasajeros.
* **TABLA EMPLEADO:** Almacena la información laboral y personal de la tripulación.
* **TABLA AVION:** Aísla las características técnicas de la flota (matrícula, año, modelo, autonomía, capacidad).
* **TABLA ASIGNACION_VUELO:** Resuelve la relación de muchos a muchos entre vuelos y empleados, registrando el rol que desempeña cada trabajador en cada vuelo específico.
* <img width="4382" height="2682" alt="EJERCICIO 2-FASES DE NORMALIZACION drawio" src="https://github.com/user-attachments/assets/4507dc68-229b-4f30-b443-02ee517153a4" />



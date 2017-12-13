
# Proyecto de análisis de datos sobre el control escolar de una universidad pública


*Autor: Carlos Miles Durón del Villar*
*Materia: Bases de Datos NoSQL*
*Institucion: Centro de Investigación en Matemáticas*


## Introducción


## 1.Fuente de Datos

Los datos utilizados para este proyecto se obtuvieron de las bases de datos institucionales modeladas en SQL Server. La estructura original, por cuestiones de seguridad, no puede ser mostrada en su totalidad pero el siguiente diagrama muestra de manera general su diseño:

![Diagrama base de datos](https://github.com/MilesDuronCIMAT/Analisis-Control-Escolar/blob/master/Diagrama.jpg)

Para este ejercicio en particular se busca contestar las siguientes preguntas:

- ¿Cuál es la matricula total por Unidad académica, programa y ciclo escolar desde el año 2000 a la fecha?
- ¿Cuál es el número de alumnos regulares e irregulares Unidad académica, programa y ciclo escolar desde el año 2000 a la fecha?
- ¿Cuáles son las 3 materias más reprobadas por programa académico?
- ¿Cuales son los promedios mas altos y mas bajos de cada programa académico?


## 2.Análisis Exploratorio

La estructura final despues de un procesado inicial en SQL Server es la siguiente:

 - Matricula - Matricula unida con la que esta registrado cada alumno
 - Clave_unidad - Clave de la Unidad Académica
 - Unidad - Nombre de la Unidad Académica
 - Clave_programa - Clave del Programa Académico
 - Programa - Nombre del Programa Académico
 - Clave_ciclo - Clave del ciclo escolar
 - Clave_plan - Clave del plan de estudios
 - Plan - Nombre del plan de estudios
 - Clave_materia - Clave de la materia
 - Materia - Nombre de la materia
 - Valor - Valor en creditos de la materia
 - Grupo
 - Semestre
 - Turno
 - Aprobado - Campo que define si la materia fue aprobada o no
 - Fecha_ord - Fecha en la que se presento el examen Ordinario
 - Calif_ord - Calificación del examen Ordinario
 - Fecha_ext - Fecha en la que se presento el examen Extra Ordinario
 - Calif_ext - Calificación del examen Extra Ordinario
 - Fecha_titulo - Fecha en la que se presento el examen Titulo
 - Calif_titulo - Calificación del examen Titulo
 
**Total de Registros: 6,161,709**

El resultado final fue almacenado en un archivo CSV llamado 'ControlEscolarFinal.csv'

## 3.Transformación de Datos

La transformación y limpieza de los datos se desarrollo utilizando los comandos de la siguiente manera:

Primero se carga el archivo que contiene todos nuestros datos.


```python
import numpy as np
import pandas as pd
ts= pd.read_csv('ControlEscolarFinal.csv')
```

Mostramos los primeros 5 registros para asegurarnos de que se cargó correctamente.


```python
ts.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>grupo</th>
      <th>semestre</th>
      <th>turno</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>﻿28901674</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>1011SNON</td>
      <td>102FM2</td>
      <td>BACHILLERATO DE CIENCIAS FISICO-MATEMATICO</td>
      <td>02FIS3</td>
      <td>FISICA III</td>
      <td>...</td>
      <td>D</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>28901674</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>1011SNON</td>
      <td>102FM2</td>
      <td>BACHILLERATO DE CIENCIAS FISICO-MATEMATICO</td>
      <td>02MAT5</td>
      <td>MATEMATICAS V</td>
      <td>...</td>
      <td>D</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-26 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>28901674</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>1011SNON</td>
      <td>102FM2</td>
      <td>BACHILLERATO DE CIENCIAS FISICO-MATEMATICO</td>
      <td>02QUI3</td>
      <td>QUIMICA III</td>
      <td>...</td>
      <td>D</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>28901674</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>1011SNON</td>
      <td>102FM2</td>
      <td>BACHILLERATO DE CIENCIAS FISICO-MATEMATICO</td>
      <td>02SOM1</td>
      <td>SOCIEDAD MEXICANA I</td>
      <td>...</td>
      <td>D</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-25 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>28901359</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>1011SNON</td>
      <td>102FM2</td>
      <td>BACHILLERATO DE CIENCIAS FISICO-MATEMATICO</td>
      <td>02ECO1</td>
      <td>ECOLOGIA I</td>
      <td>...</td>
      <td>D</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-27 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



Al venir de una base de datos SQL, los campos con información de texto cuentan con espacios que rellenan la longitud total posible del campo ```'PREPARATORIA NO. 2       '``` por lo que es necesario quitar los espacios vacios al final de dichos campos.


```python
ts['matricula']=ts['matricula'].str.strip()
ts['unidad']=ts['unidad'].str.strip()
ts['programa']=ts['programa'].str.strip()
ts['plan']=ts['plan'].str.strip()
ts['materia']=ts['materia'].str.strip()
```

Como solo se quieren los datos del año 2000 a la fecha, se procede a eleminar todos aquellos registros que no entren dentro del rango.


```python
ts=ts[(
    ts['clave_ciclo'].str.match('0001SNON')|
    ts['clave_ciclo'].str.match('0001SPAR')|
    ts['clave_ciclo'].str.match('0102SNON')|
    ts['clave_ciclo'].str.match('0102SPAR')|
    ts['clave_ciclo'].str.match('0203SNON')|
    ts['clave_ciclo'].str.match('0203SPAR')|
    ts['clave_ciclo'].str.match('0304SNON')|
    ts['clave_ciclo'].str.match('0304SPAR')|
    ts['clave_ciclo'].str.match('0405SNON')|
    ts['clave_ciclo'].str.match('0405SPAR')|
    ts['clave_ciclo'].str.match('0506SNON')|
    ts['clave_ciclo'].str.match('0506SPAR')|
    ts['clave_ciclo'].str.match('0607SNON')|
    ts['clave_ciclo'].str.match('0607SPAR')|
    ts['clave_ciclo'].str.match('0708SNON')|
    ts['clave_ciclo'].str.match('0708SPAR')|
    ts['clave_ciclo'].str.match('0809SNON')|
    ts['clave_ciclo'].str.match('0809SPAR')|
    ts['clave_ciclo'].str.match('0910SNON')|
    ts['clave_ciclo'].str.match('0910SPAR')|
    ts['clave_ciclo'].str.match('1011SNON')|
    ts['clave_ciclo'].str.match('1011SPAR')|
    ts['clave_ciclo'].str.match('1112SNON')|
    ts['clave_ciclo'].str.match('1112SPAR')|
    ts['clave_ciclo'].str.match('1213SNON')|
    ts['clave_ciclo'].str.match('1213SPAR')|
    ts['clave_ciclo'].str.match('1314SNON')|
    ts['clave_ciclo'].str.match('1314SPAR')|
    ts['clave_ciclo'].str.match('1415SNON')|
    ts['clave_ciclo'].str.match('1415SPAR')|
    ts['clave_ciclo'].str.match('1516SNON')|
    ts['clave_ciclo'].str.match('1516SPAR')|
    ts['clave_ciclo'].str.match('1617SNON')|
    ts['clave_ciclo'].str.match('1617SPAR')|
    ts['clave_ciclo'].str.match('1718SNON')
)]

```

El identificador único de cada alumno es su matricula la cual siempre debe constar de 8 caracteres, por lo que se consideran como registros basura todos aquellos que contengan matriculas de longitud diferente a 8.


```python
ts[ts['matricula'].str.len()<8]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>grupo</th>
      <th>semestre</th>
      <th>turno</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1154</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCICSA</td>
      <td>INTRODUCCION A LAS CIENCIAS DE LA SALUD</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1155</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCCIMO</td>
      <td>INTRODUCCION A CIENCIAS MORFOLOGICAS</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1156</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCMATE</td>
      <td>MATEMATICAS</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1157</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCQUIM</td>
      <td>QUIMICA GENERAL</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1158</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCBIOL</td>
      <td>BIOLOGIA CELULAR</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1237</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCBIOE</td>
      <td>BIOESTADISTICA</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1238</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCEMBR</td>
      <td>EMBRIOLOGIA</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1239</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCANAT</td>
      <td>ANATOMIA HUMANA</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1240</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCBIOQ</td>
      <td>BIOQUIMICA</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1241</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCHIST</td>
      <td>HISTOLOGIA</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1242</th>
      <td>32</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1112SPAR</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCBIMO</td>
      <td>BIOLOGIA MOLECULAR</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-08-09 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>165704</th>
      <td>513100</td>
      <td>22701</td>
      <td>DERECHO PLANTEL ZACATECAS</td>
      <td>156020</td>
      <td>LICENCIADO EN DERECHO</td>
      <td>0910SNON</td>
      <td>111DE2</td>
      <td>LA CARRERA DE LICENCIADO EN DERECHO.</td>
      <td>11LOMA</td>
      <td>LOGICA MATEMATICA</td>
      <td>...</td>
      <td>M</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>165705</th>
      <td>513100</td>
      <td>22701</td>
      <td>DERECHO PLANTEL ZACATECAS</td>
      <td>156020</td>
      <td>LICENCIADO EN DERECHO</td>
      <td>0910SNON</td>
      <td>111DE2</td>
      <td>LA CARRERA DE LICENCIADO EN DERECHO.</td>
      <td>11MTIJ</td>
      <td>METODOS Y TECNICAS DE INVESTIGACION JURIDICA</td>
      <td>...</td>
      <td>M</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>165706</th>
      <td>513100</td>
      <td>22701</td>
      <td>DERECHO PLANTEL ZACATECAS</td>
      <td>156020</td>
      <td>LICENCIADO EN DERECHO</td>
      <td>0910SNON</td>
      <td>111DE2</td>
      <td>LA CARRERA DE LICENCIADO EN DERECHO.</td>
      <td>11SOJU</td>
      <td>SOCIOLOGIA JURIDICA</td>
      <td>...</td>
      <td>M</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>165707</th>
      <td>513100</td>
      <td>22701</td>
      <td>DERECHO PLANTEL ZACATECAS</td>
      <td>156020</td>
      <td>LICENCIADO EN DERECHO</td>
      <td>0910SNON</td>
      <td>111DE2</td>
      <td>LA CARRERA DE LICENCIADO EN DERECHO.</td>
      <td>11TEGD</td>
      <td>TEORIA GENERAL DEL DERECHO</td>
      <td>...</td>
      <td>M</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>975808</th>
      <td>514554</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102P02</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02CIS1</td>
      <td>CIENCIAS SOCIALES I</td>
      <td>...</td>
      <td>I2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>975809</th>
      <td>514554</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102P02</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02COP1</td>
      <td>COMPUTACION I</td>
      <td>...</td>
      <td>I2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>975810</th>
      <td>514554</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102P02</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02FIS1</td>
      <td>FISICA I</td>
      <td>...</td>
      <td>I2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>975811</th>
      <td>514554</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102P02</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02HUN1</td>
      <td>HUMANIDADES I</td>
      <td>...</td>
      <td>I2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>975812</th>
      <td>514554</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102P02</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02ING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>I2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>975813</th>
      <td>514554</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102P02</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02MAT1</td>
      <td>MATEMATICAS I</td>
      <td>...</td>
      <td>I2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>975814</th>
      <td>514554</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102P02</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02TAL1</td>
      <td>TALLER DE LECTURA Y REDACCION I</td>
      <td>...</td>
      <td>I2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1344115</th>
      <td>2000058</td>
      <td>20200</td>
      <td>PREPARATORIA NO. 1</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>0203SNON</td>
      <td>102P01</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02HUM3</td>
      <td>HUMANIDADES III</td>
      <td>...</td>
      <td>C</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2002-12-03 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1386039</th>
      <td>2000058</td>
      <td>20200</td>
      <td>PREPARATORIA NO. 1</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>0102SPAR</td>
      <td>102P01</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02FIS2</td>
      <td>FISICA II</td>
      <td>...</td>
      <td>E</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>2002-06-05 00:00:00.000</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1796744</th>
      <td>513302</td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>156010</td>
      <td>LICENCIADO EN CONTADURIA</td>
      <td>0910SNON</td>
      <td>110CA8</td>
      <td>LA CARRERA DE LICENCIATURA EN CONTADURIA.</td>
      <td>10CAAP</td>
      <td>CALCULO APLICADO</td>
      <td>...</td>
      <td>A</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1796745</th>
      <td>513302</td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>156010</td>
      <td>LICENCIADO EN CONTADURIA</td>
      <td>0910SNON</td>
      <td>110CA8</td>
      <td>LA CARRERA DE LICENCIATURA EN CONTADURIA.</td>
      <td>10CONI</td>
      <td>CONTABILIDAD INTERMEDIA</td>
      <td>...</td>
      <td>A</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1796746</th>
      <td>513302</td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>156010</td>
      <td>LICENCIADO EN CONTADURIA</td>
      <td>0910SNON</td>
      <td>110CA8</td>
      <td>LA CARRERA DE LICENCIATURA EN CONTADURIA.</td>
      <td>10DEM1</td>
      <td>DERECHO MERCANTIL I</td>
      <td>...</td>
      <td>A</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1796747</th>
      <td>513302</td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>156010</td>
      <td>LICENCIADO EN CONTADURIA</td>
      <td>0910SNON</td>
      <td>110CA8</td>
      <td>LA CARRERA DE LICENCIATURA EN CONTADURIA.</td>
      <td>10INES</td>
      <td>INTRODUCCION A LA ESTADISTICA</td>
      <td>...</td>
      <td>A</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2408935</th>
      <td>129063</td>
      <td>22701</td>
      <td>DERECHO PLANTEL ZACATECAS</td>
      <td>194602</td>
      <td>SUA LICENCIADO EN DERECHO</td>
      <td>0910SNON</td>
      <td>111DE2</td>
      <td>LA CARRERA DE LICENCIADO EN DERECHO.</td>
      <td>11LOMA</td>
      <td>LOGICA MATEMATICA</td>
      <td>...</td>
      <td>JR</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2408936</th>
      <td>129063</td>
      <td>22701</td>
      <td>DERECHO PLANTEL ZACATECAS</td>
      <td>194602</td>
      <td>SUA LICENCIADO EN DERECHO</td>
      <td>0910SNON</td>
      <td>111DE2</td>
      <td>LA CARRERA DE LICENCIADO EN DERECHO.</td>
      <td>11MTIJ</td>
      <td>METODOS Y TECNICAS DE INVESTIGACION JURIDICA</td>
      <td>...</td>
      <td>JR</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2408937</th>
      <td>129063</td>
      <td>22701</td>
      <td>DERECHO PLANTEL ZACATECAS</td>
      <td>194602</td>
      <td>SUA LICENCIADO EN DERECHO</td>
      <td>0910SNON</td>
      <td>111DE2</td>
      <td>LA CARRERA DE LICENCIADO EN DERECHO.</td>
      <td>11SOJU</td>
      <td>SOCIOLOGIA JURIDICA</td>
      <td>...</td>
      <td>JR</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2408938</th>
      <td>129063</td>
      <td>22701</td>
      <td>DERECHO PLANTEL ZACATECAS</td>
      <td>194602</td>
      <td>SUA LICENCIADO EN DERECHO</td>
      <td>0910SNON</td>
      <td>111DE2</td>
      <td>LA CARRERA DE LICENCIADO EN DERECHO.</td>
      <td>11TEGD</td>
      <td>TEORIA GENERAL DEL DERECHO</td>
      <td>...</td>
      <td>JR</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2499105</th>
      <td>127949</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>192001</td>
      <td>SUA BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102QB2</td>
      <td>BACHILLERATO DE CIENCIAS QUIMICAS-BIOLOGICAS.</td>
      <td>02CIE1</td>
      <td>CIENCIAS DE LA SALUD I</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2499106</th>
      <td>127949</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>192001</td>
      <td>SUA BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102QB2</td>
      <td>BACHILLERATO DE CIENCIAS QUIMICAS-BIOLOGICAS.</td>
      <td>02ECO1</td>
      <td>ECOLOGIA I</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2499107</th>
      <td>127949</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>192001</td>
      <td>SUA BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102QB2</td>
      <td>BACHILLERATO DE CIENCIAS QUIMICAS-BIOLOGICAS.</td>
      <td>02SOM1</td>
      <td>SOCIEDAD MEXICANA I</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2499108</th>
      <td>127949</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>192001</td>
      <td>SUA BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102QB2</td>
      <td>BACHILLERATO DE CIENCIAS QUIMICAS-BIOLOGICAS.</td>
      <td>02CIE2</td>
      <td>CIENCIAS DE LA SALUD II</td>
      <td>...</td>
      <td>A</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2499109</th>
      <td>127949</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>192001</td>
      <td>SUA BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102QB2</td>
      <td>BACHILLERATO DE CIENCIAS QUIMICAS-BIOLOGICAS.</td>
      <td>02ECO2</td>
      <td>ECOLOGIA II</td>
      <td>...</td>
      <td>A</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2499110</th>
      <td>127949</td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>192001</td>
      <td>SUA BACHILLERATO</td>
      <td>0910SNON</td>
      <td>102QB2</td>
      <td>BACHILLERATO DE CIENCIAS QUIMICAS-BIOLOGICAS.</td>
      <td>02SOM2</td>
      <td>SOCIEDAD MEXICANA II</td>
      <td>...</td>
      <td>A</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2630431</th>
      <td>513103</td>
      <td>22100</td>
      <td>ENFERMERIA PLANTEL ZACATECAS</td>
      <td>130200</td>
      <td>TECNICO EN ENFERMERIA</td>
      <td>0910SNON</td>
      <td>107EN9</td>
      <td>LA CARRERA DE ENFERMERIA GENERAL.</td>
      <td>07ANFI</td>
      <td>ANATOMIA Y FISIOLOGIA</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2630432</th>
      <td>513103</td>
      <td>22100</td>
      <td>ENFERMERIA PLANTEL ZACATECAS</td>
      <td>130200</td>
      <td>TECNICO EN ENFERMERIA</td>
      <td>0910SNON</td>
      <td>107EN9</td>
      <td>LA CARRERA DE ENFERMERIA GENERAL.</td>
      <td>07DOC1</td>
      <td>DOCENCIA CLINICA I</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2630433</th>
      <td>513103</td>
      <td>22100</td>
      <td>ENFERMERIA PLANTEL ZACATECAS</td>
      <td>130200</td>
      <td>TECNICO EN ENFERMERIA</td>
      <td>0910SNON</td>
      <td>107EN9</td>
      <td>LA CARRERA DE ENFERMERIA GENERAL.</td>
      <td>07ECSA</td>
      <td>ECOLOGIA Y SALUD</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2630434</th>
      <td>513103</td>
      <td>22100</td>
      <td>ENFERMERIA PLANTEL ZACATECAS</td>
      <td>130200</td>
      <td>TECNICO EN ENFERMERIA</td>
      <td>0910SNON</td>
      <td>107EN9</td>
      <td>LA CARRERA DE ENFERMERIA GENERAL.</td>
      <td>07ENCO</td>
      <td>ENFERMERIA COMUNITARIA</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2630435</th>
      <td>513103</td>
      <td>22100</td>
      <td>ENFERMERIA PLANTEL ZACATECAS</td>
      <td>130200</td>
      <td>TECNICO EN ENFERMERIA</td>
      <td>0910SNON</td>
      <td>107EN9</td>
      <td>LA CARRERA DE ENFERMERIA GENERAL.</td>
      <td>07FUEN</td>
      <td>FUNDAMENTOS DE ENFERMERIA</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2630436</th>
      <td>513103</td>
      <td>22100</td>
      <td>ENFERMERIA PLANTEL ZACATECAS</td>
      <td>130200</td>
      <td>TECNICO EN ENFERMERIA</td>
      <td>0910SNON</td>
      <td>107EN9</td>
      <td>LA CARRERA DE ENFERMERIA GENERAL.</td>
      <td>07SAPU</td>
      <td>SALUD PUBLICA</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2734340</th>
      <td>513302</td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>166010</td>
      <td>MAESTRIA EN ADMINISTRACION</td>
      <td>0910SNON</td>
      <td>120MA3</td>
      <td>LA MAESTRIA EN ADMINISTRACION CON ORIENTACION ...</td>
      <td>20ADFI</td>
      <td>ADMINISTRACION FINANCIERA</td>
      <td>...</td>
      <td>B</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2734341</th>
      <td>513302</td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>166010</td>
      <td>MAESTRIA EN ADMINISTRACION</td>
      <td>0910SNON</td>
      <td>120MA3</td>
      <td>LA MAESTRIA EN ADMINISTRACION CON ORIENTACION ...</td>
      <td>20ADMI</td>
      <td>ADMINISTRACION</td>
      <td>...</td>
      <td>B</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2734342</th>
      <td>513302</td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>166010</td>
      <td>MAESTRIA EN ADMINISTRACION</td>
      <td>0910SNON</td>
      <td>120MA3</td>
      <td>LA MAESTRIA EN ADMINISTRACION CON ORIENTACION ...</td>
      <td>20INFO</td>
      <td>INFORMATICA</td>
      <td>...</td>
      <td>B</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2734343</th>
      <td>513302</td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>166010</td>
      <td>MAESTRIA EN ADMINISTRACION</td>
      <td>0910SNON</td>
      <td>120MA3</td>
      <td>LA MAESTRIA EN ADMINISTRACION CON ORIENTACION ...</td>
      <td>20MAAA</td>
      <td>MATEMATICAS APLICADAS A LA ADMINISTRACION</td>
      <td>...</td>
      <td>B</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2734344</th>
      <td>513302</td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>166010</td>
      <td>MAESTRIA EN ADMINISTRACION</td>
      <td>0910SNON</td>
      <td>120MA3</td>
      <td>LA MAESTRIA EN ADMINISTRACION CON ORIENTACION ...</td>
      <td>20MEIN</td>
      <td>METODOLOGIA DE LA INVESTIGACION</td>
      <td>...</td>
      <td>B</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2745069</th>
      <td>516579</td>
      <td>22900</td>
      <td>MUSICA</td>
      <td>110200</td>
      <td>LICENCIATURA EN MUSICA NIVEL JUVENIL</td>
      <td>0910SNON</td>
      <td>0GUNJ1</td>
      <td>LA CARRERA DE GUITARRA NIVEL JUVENIL</td>
      <td>0APMU1</td>
      <td>APRECIACION MUSICAL I</td>
      <td>...</td>
      <td>E</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2745070</th>
      <td>516579</td>
      <td>22900</td>
      <td>MUSICA</td>
      <td>110200</td>
      <td>LICENCIATURA EN MUSICA NIVEL JUVENIL</td>
      <td>0910SNON</td>
      <td>0GUNJ1</td>
      <td>LA CARRERA DE GUITARRA NIVEL JUVENIL</td>
      <td>0GUI01</td>
      <td>GUITARRA I</td>
      <td>...</td>
      <td>E</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2745071</th>
      <td>516579</td>
      <td>22900</td>
      <td>MUSICA</td>
      <td>110200</td>
      <td>LICENCIATURA EN MUSICA NIVEL JUVENIL</td>
      <td>0910SNON</td>
      <td>0GUNJ1</td>
      <td>LA CARRERA DE GUITARRA NIVEL JUVENIL</td>
      <td>0SOLF1</td>
      <td>SOLFEO I</td>
      <td>...</td>
      <td>E</td>
      <td>1</td>
      <td>4</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2881278</th>
      <td>11</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1011SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09SRSO</td>
      <td>SERVICIO SOCIAL</td>
      <td>...</td>
      <td>B</td>
      <td>11</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3055570</th>
      <td></td>
      <td>24001</td>
      <td>MEDICINA HUMANA PLANTEL FRESNILLO</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1314SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCGAST</td>
      <td>GASTROENTEROLOGIA</td>
      <td>...</td>
      <td>B</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>2013-11-27 07:00:00.000</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3409506</th>
      <td></td>
      <td>20300</td>
      <td>PREPARATORIA NO. 2</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>1112SPAR</td>
      <td>1024QB</td>
      <td>BACHILLERATO DE CIENCIAS QUIMICAS BIOLOGICAS</td>
      <td>02MAT4</td>
      <td>MATEMATICAS IV</td>
      <td>...</td>
      <td>CK</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2012-06-19 00:00:00.000</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4355645</th>
      <td></td>
      <td>23803</td>
      <td>PREPARATORIA NO. 11</td>
      <td>120100</td>
      <td>BACHILLERATO</td>
      <td>1213SNON</td>
      <td>102PO3</td>
      <td>BACHILLERATO DE SEIS SEMESTRES.</td>
      <td>02TAL3</td>
      <td>TALLER DE LECTURA Y REDACCION III</td>
      <td>...</td>
      <td>A</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2012-12-03 08:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4694077</th>
      <td></td>
      <td>22600</td>
      <td>CONTADURIA Y ADMINISTRACION</td>
      <td>194601</td>
      <td>SUA LICENCIADO EN CONTADURIA Y ADMINISTRACION</td>
      <td>1415SNON</td>
      <td>110SU8</td>
      <td>LA CARRERA DE LICENCIATURA EN CONTADURIA.</td>
      <td>10INAU</td>
      <td>INTRODUCCION A LA AUDITORIA</td>
      <td>...</td>
      <td>B</td>
      <td>7</td>
      <td>4</td>
      <td>0</td>
      <td>2014-11-26 03:00:00.000</td>
      <td>5</td>
      <td>2014-12-09 03:00:00.000</td>
      <td>5</td>
      <td>1900-01-01 00:00:00.000</td>
      <td>NP</td>
    </tr>
  </tbody>
</table>
<p>57 rows × 21 columns</p>
</div>




```python
ts=ts[~(ts['matricula'].str.len()<8)]
ts[ts['matricula'].str.len()<8]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>grupo</th>
      <th>semestre</th>
      <th>turno</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
<p>0 rows × 21 columns</p>
</div>



De igual manera se consideran registros basura aquellos que los campos **unidad, programa, clave_ciclo y materia** esten vacios.


```python
ts['unidad'].isnull().values.any()
```




    False




```python
ts['programa'].isnull().values.any()
```




    False




```python
ts['clave_ciclo'].isnull().values.any()
```




    False




```python
ts['materia'].isnull().values.any()
```




    False



Dentro de la institución se ofrecen estudios de lenguas extranjeras a la población en general, sin embargo, todo aquel estudiante de lenguas extranjeras que no esta inscrito formalmente en alguna unidad académica no es considerado alumno perteneciente a la universidad. Por lo tanto, se eliminaran todos los registros de dichos estudiantes. El rango de claves son entre 182000 y 182999.


```python
ts[(ts['clave_programa']>=182000)&(ts['clave_programa']<=182999)]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>grupo</th>
      <th>semestre</th>
      <th>turno</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>313</th>
      <td>32EI1158</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1112SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>I</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>NaN</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>314</th>
      <td>32EI1159</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1112SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>I</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>NaN</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>785</th>
      <td>32CI7341</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1213SPAR</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING2</td>
      <td>INGLES II</td>
      <td>...</td>
      <td>I</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>2013-05-31 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3354</th>
      <td>29CI2449</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182002</td>
      <td>FRANCES</td>
      <td>1213SPAR</td>
      <td>CIPFR1</td>
      <td>FRANCES</td>
      <td>CIFRA1</td>
      <td>FRANCES I</td>
      <td>...</td>
      <td>FD</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2013-05-31 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4462</th>
      <td>29CI6781</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182012</td>
      <td>INGLES PARA NIÑOS</td>
      <td>1213SNON</td>
      <td>CINIU1</td>
      <td>INGLES NIÑOS UAZ</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>ND</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>2013-08-31 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6744</th>
      <td>30CI3583</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182012</td>
      <td>INGLES PARA NIÑOS</td>
      <td>1314SNON</td>
      <td>CINIU1</td>
      <td>INGLES NIÑOS UAZ</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>ND</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2014-08-30 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6747</th>
      <td>30CI3584</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182012</td>
      <td>INGLES PARA NIÑOS</td>
      <td>1314SNON</td>
      <td>CINIU1</td>
      <td>INGLES NIÑOS UAZ</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>ND</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2014-08-30 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6796</th>
      <td>32EI1307</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1112SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>AF</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>NaN</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9020</th>
      <td>32EI1308</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1112SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>V</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9243</th>
      <td>32CI6558</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9244</th>
      <td>32CI6560</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9252</th>
      <td>32CI6567</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9299</th>
      <td>32EI1768</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9300</th>
      <td>32EI1769</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9301</th>
      <td>32CI8602</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9302</th>
      <td>32EI1778</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9303</th>
      <td>32CI6590</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9304</th>
      <td>32EI1785</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9305</th>
      <td>32CI8604</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9306</th>
      <td>32EI4209</td>
      <td>29825</td>
      <td>CULTURA PLANTEL OJOCALIENTE</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1314SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>A</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-18 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9614</th>
      <td>32EI1272</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182005</td>
      <td>ALEMAN</td>
      <td>1112SNON</td>
      <td>CIPAL1</td>
      <td>ALEMAN</td>
      <td>CIALE1</td>
      <td>ALEMAN I</td>
      <td>...</td>
      <td>AM</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28135</th>
      <td>32CI5177</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1415SPAR</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING7</td>
      <td>INGLES VII</td>
      <td>...</td>
      <td>B</td>
      <td>7</td>
      <td>2</td>
      <td>1</td>
      <td>2015-06-30 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28136</th>
      <td>32CI7400</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1415SPAR</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING6</td>
      <td>INGLES VI</td>
      <td>...</td>
      <td>H</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>2015-06-30 00:00:00.000</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>28137</th>
      <td>29CI6938</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182006</td>
      <td>ITALIANO</td>
      <td>1415SPAR</td>
      <td>CIPIT1</td>
      <td>ITALIANO</td>
      <td>CIITA1</td>
      <td>ITALIANO I</td>
      <td>...</td>
      <td>D</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2015-06-30 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>30981</th>
      <td>32EI0916</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182002</td>
      <td>FRANCES</td>
      <td>1112SNON</td>
      <td>CIPFR1</td>
      <td>FRANCES</td>
      <td>CIFRA1</td>
      <td>FRANCES I</td>
      <td>...</td>
      <td>FC</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>2012-01-18 00:00:00.000</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35640</th>
      <td>30CI5283</td>
      <td>30203</td>
      <td>IDIOMAS PLANTEL TLALTENANGO</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1011SPAR</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING2</td>
      <td>INGLES II</td>
      <td>...</td>
      <td>A</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35642</th>
      <td>30CI5286</td>
      <td>30203</td>
      <td>IDIOMAS PLANTEL TLALTENANGO</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1011SPAR</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35643</th>
      <td>30CI5287</td>
      <td>30203</td>
      <td>IDIOMAS PLANTEL TLALTENANGO</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1011SPAR</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35644</th>
      <td>30CI5290</td>
      <td>30203</td>
      <td>IDIOMAS PLANTEL TLALTENANGO</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1011SPAR</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING2</td>
      <td>INGLES II</td>
      <td>...</td>
      <td>A</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>35645</th>
      <td>30CI5291</td>
      <td>30203</td>
      <td>IDIOMAS PLANTEL TLALTENANGO</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1011SPAR</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING2</td>
      <td>INGLES II</td>
      <td>...</td>
      <td>A</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5717017</th>
      <td>36CI2045</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING2</td>
      <td>INGLES II</td>
      <td>...</td>
      <td>S</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5717018</th>
      <td>36CI2835</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING3</td>
      <td>INGLES III</td>
      <td>...</td>
      <td>Q</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5717027</th>
      <td>36CI2836</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING3</td>
      <td>INGLES III</td>
      <td>...</td>
      <td>Q</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5717031</th>
      <td>37CI1947</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING2</td>
      <td>INGLES II</td>
      <td>...</td>
      <td>S</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5717032</th>
      <td>37CI1948</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING4</td>
      <td>INGLES IV</td>
      <td>...</td>
      <td>L</td>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5718026</th>
      <td>36CI1108</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182014</td>
      <td>LENGUAJE DE SEÑAS</td>
      <td>1718SNON</td>
      <td>CILSE1</td>
      <td>LENGUAJE DE SEÑAS MANUAL</td>
      <td>CISEM3</td>
      <td>LENGUAJE SEÑAS MANUAL III</td>
      <td>...</td>
      <td>A</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2017-12-15 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5718027</th>
      <td>32CI8561</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING7</td>
      <td>INGLES VII</td>
      <td>...</td>
      <td>TE</td>
      <td>7</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5718028</th>
      <td>29CI6695</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING7</td>
      <td>INGLES VII</td>
      <td>...</td>
      <td>TE</td>
      <td>7</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5718041</th>
      <td>36CI0638</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING3</td>
      <td>INGLES III</td>
      <td>...</td>
      <td>TA</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5718128</th>
      <td>36CI2619</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING2</td>
      <td>INGLES II</td>
      <td>...</td>
      <td>TB</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719437</th>
      <td>34CI0100</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING3</td>
      <td>INGLES III</td>
      <td>...</td>
      <td>V</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719465</th>
      <td>36CI2416</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING3</td>
      <td>INGLES III</td>
      <td>...</td>
      <td>L</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719604</th>
      <td>36CI0337</td>
      <td>29826</td>
      <td>CULTURA PLANTEL JEREZ</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING3</td>
      <td>INGLES III</td>
      <td>...</td>
      <td>A</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2017-12-15 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719621</th>
      <td>37CI1443</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>B2</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719681</th>
      <td>36CI3632</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>B2</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719682</th>
      <td>36CI3511</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>B2</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719690</th>
      <td>37CI0066</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>B2</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719704</th>
      <td>37CI1543</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>B2</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719712</th>
      <td>29CI8926</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182002</td>
      <td>FRANCES</td>
      <td>1718SNON</td>
      <td>CIPFR1</td>
      <td>FRANCES</td>
      <td>CIFRA3</td>
      <td>FRANCES III</td>
      <td>...</td>
      <td>A</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719928</th>
      <td>36CI1879</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING2</td>
      <td>INGLES II</td>
      <td>...</td>
      <td>A2</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719966</th>
      <td>37CI1692</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182015</td>
      <td>PORTUGUES</td>
      <td>1718SNON</td>
      <td>CIPOR1</td>
      <td>PORTUGUÉS</td>
      <td>CIPOR1</td>
      <td>PORTUGUÉS I</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719993</th>
      <td>37CI1693</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>A2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5719995</th>
      <td>29CI8321</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182015</td>
      <td>PORTUGUES</td>
      <td>1718SNON</td>
      <td>CIPOR1</td>
      <td>PORTUGUÉS</td>
      <td>CIPOR1</td>
      <td>PORTUGUÉS I</td>
      <td>...</td>
      <td>A</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5720061</th>
      <td>37CI1695</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>G</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5720118</th>
      <td>35CI0180</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING5</td>
      <td>INGLES V</td>
      <td>...</td>
      <td>N</td>
      <td>5</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5720209</th>
      <td>35CI0382</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182006</td>
      <td>ITALIANO</td>
      <td>1718SNON</td>
      <td>CIPIT1</td>
      <td>ITALIANO</td>
      <td>CIITA3</td>
      <td>ITALIANO III</td>
      <td>...</td>
      <td>A</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>2017-12-15 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5720229</th>
      <td>35CI2001</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING3</td>
      <td>INGLES III</td>
      <td>...</td>
      <td>L</td>
      <td>3</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5720238</th>
      <td>37CI1700</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>I</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5720239</th>
      <td>37CI1702</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>X</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5720286</th>
      <td>37CI1703</td>
      <td>29821</td>
      <td>CULTURA PLANTEL ZACATECAS</td>
      <td>182001</td>
      <td>INGLES</td>
      <td>1718SNON</td>
      <td>CIPIN1</td>
      <td>INGLES</td>
      <td>CIING1</td>
      <td>INGLES I</td>
      <td>...</td>
      <td>U</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>65843 rows × 21 columns</p>
</div>




```python
ts=ts[~((ts['clave_programa']>=182000)&(ts['clave_programa']<=182999))]
ts[(ts['clave_programa']>=182000)&(ts['clave_programa']<=182999)]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>grupo</th>
      <th>semestre</th>
      <th>turno</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
<p>0 rows × 21 columns</p>
</div>



Para que una materia este aprobada debe de tener por lo menos una calificacion aprobatoria en las tres oportunidades disponibles: **Ordinario, Extra Ordinario y Titulo**.


```python
ts[(ts['aprobado']==1)&(ts['calif_ord'].isnull())&(ts['calif_ext'].isnull())&(ts['calif_titulo'].isnull())]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>grupo</th>
      <th>semestre</th>
      <th>turno</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1655243</th>
      <td>27800564</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154060</td>
      <td>LICENCIATURA EN NUTRICIÓN</td>
      <td>0708SNON</td>
      <td>LICNUT</td>
      <td>LICENCIATURA EN NUTRICION.</td>
      <td>TCMATE</td>
      <td>MATEMATICAS</td>
      <td>...</td>
      <td>V</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 21 columns</p>
</div>




```python
ts=ts[~((ts['aprobado']==1)&(ts['calif_ord'].isnull())&(ts['calif_ext'].isnull())&(ts['calif_titulo'].isnull()))]
ts[(ts['aprobado']==1)&(ts['calif_ord'].isnull())&(ts['calif_ext'].isnull())&(ts['calif_titulo'].isnull())]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>grupo</th>
      <th>semestre</th>
      <th>turno</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
<p>0 rows × 21 columns</p>
</div>



La universidad tambien oferta programas de educacion **Secundaria y Preparatoria**, pero para este ejercicios se descartaran. 


```python
media_superior=ts[(ts['unidad'].str.contains('SECUNDARIA'))|(ts['unidad'].str.contains('PREPARATORIA'))]
sorted(media_superior['unidad'].unique())
```




    ['PREPARATORIA NO. 1',
     'PREPARATORIA NO. 10',
     'PREPARATORIA NO. 11',
     'PREPARATORIA NO. 12',
     'PREPARATORIA NO. 13',
     'PREPARATORIA NO. 2',
     'PREPARATORIA NO. 3',
     'PREPARATORIA NO. 4',
     'PREPARATORIA NO. 5',
     'PREPARATORIA NO. 6',
     'PREPARATORIA NO. 7',
     'PREPARATORIA NO. 8',
     'PREPARATORIA NO. 9',
     'SECUNDARIA']




```python
lic_pos=ts[~((ts['unidad'].str.contains('SECUNDARIA'))|(ts['unidad'].str.contains('PREPARATORIA')))]
sorted(lic_pos['unidad'].unique())
```




    ['AGRONOMIA',
     'AGRONOMIA PLANTEL VALPARAISO',
     'ANTROPOLOGIA',
     'ARTES',
     'CIENCIA POLITICA',
     'CIENCIAS BIOLOGICAS',
     'CIENCIAS DE LA SALUD',
     'CIENCIAS DE LA TIERRA',
     'CIENCIAS QUIMICAS',
     'CIENCIAS SOCIALES',
     'CONTADURIA Y ADMINISTRACION',
     'CULTURA PLANTEL ZACATECAS',
     'DERECHO PLANTEL FRESNILLO',
     'DERECHO PLANTEL ZACATECAS',
     'DOCENCIA SUPERIOR',
     'DOCENCIA SUPERIOR PLANTEL JEREZ',
     'DOCENCIA SUPERIOR PLANTEL RIO GRANDE',
     'DOCTORADO EN ESTUDIOS DEL DESARROLLO',
     'ECONOMIA',
     'ENFERMERIA PLANTEL JALPA',
     'ENFERMERIA PLANTEL JUAN ALDAMA',
     'ENFERMERIA PLANTEL NOCHISTLAN',
     'ENFERMERIA PLANTEL ZACATECAS',
     'ESTUDIOS DE LAS HUMANIDADES',
     'ESTUDIOS NUCLEARES',
     'FILOSOFIA',
     'FISICA',
     'HISTORIA',
     'HISTORIA PLANTEL JEREZ',
     'INGENIERIA ELECTRICA PLANTEL JALPA',
     'INGENIERIA ELECTRICA PLANTEL ZACATECAS',
     'INGENIERIA I',
     'LETRAS PLANTEL JEREZ',
     'LETRAS PLANTEL ZACATECAS',
     'MATEMATICAS',
     'MEDICINA HUMANA PLANTEL FRESNILLO',
     'MEDICINA HUMANA PLANTEL ZACATECAS',
     'MEDICINA VETERINARIA Y ZOOTECNIA',
     'MUSICA',
     'NUTRICION PLANTEL TLALTENANGO',
     'ODONTOLOGIA',
     'PSICOLOGIA PLANTEL FRESNILLO',
     'PSICOLOGIA PLANTEL JALPA',
     'PSICOLOGIA PLANTEL JUAN ALDAMA',
     'PSICOLOGIA PLANTEL OJOCALIENTE',
     'PSICOLOGIA PLANTEL ZACATECAS']



La lista anterior muestra las diferentes unidades académicas disponibles, pero al analizarla más a detalle se encuentran extensiones de algunas unidades. Todas las extensiones comparten la palabra ```'PLANTEL' ```.


```python
sorted(lic_pos[lic_pos['unidad'].str.contains("PLANTEL")].unidad.unique())
```




    ['AGRONOMIA PLANTEL VALPARAISO',
     'CULTURA PLANTEL ZACATECAS',
     'DERECHO PLANTEL FRESNILLO',
     'DERECHO PLANTEL ZACATECAS',
     'DOCENCIA SUPERIOR PLANTEL JEREZ',
     'DOCENCIA SUPERIOR PLANTEL RIO GRANDE',
     'ENFERMERIA PLANTEL JALPA',
     'ENFERMERIA PLANTEL JUAN ALDAMA',
     'ENFERMERIA PLANTEL NOCHISTLAN',
     'ENFERMERIA PLANTEL ZACATECAS',
     'HISTORIA PLANTEL JEREZ',
     'INGENIERIA ELECTRICA PLANTEL JALPA',
     'INGENIERIA ELECTRICA PLANTEL ZACATECAS',
     'LETRAS PLANTEL JEREZ',
     'LETRAS PLANTEL ZACATECAS',
     'MEDICINA HUMANA PLANTEL FRESNILLO',
     'MEDICINA HUMANA PLANTEL ZACATECAS',
     'NUTRICION PLANTEL TLALTENANGO',
     'PSICOLOGIA PLANTEL FRESNILLO',
     'PSICOLOGIA PLANTEL JALPA',
     'PSICOLOGIA PLANTEL JUAN ALDAMA',
     'PSICOLOGIA PLANTEL OJOCALIENTE',
     'PSICOLOGIA PLANTEL ZACATECAS']




```python
Dado que en realidad estos planteles son parte de una sola unidad se deberan homologar e una sola unidad.
```


```python
unidades = lic_pos['unidad'].unique()
dict={}
for unidad in unidades:
    if "PLANTEL" in unidad:
        dict[unidad]=unidad[:unidad.find("PLANTEL")-1]
    else:
        dict[unidad]=unidad
print (dict)
```

    {'CULTURA PLANTEL ZACATECAS': 'CULTURA', 'CIENCIAS BIOLOGICAS': 'CIENCIAS BIOLOGICAS', 'DOCENCIA SUPERIOR PLANTEL RIO GRANDE': 'DOCENCIA SUPERIOR', 'AGRONOMIA': 'AGRONOMIA', 'LETRAS PLANTEL JEREZ': 'LETRAS', 'HISTORIA': 'HISTORIA', 'INGENIERIA ELECTRICA PLANTEL JALPA': 'INGENIERIA ELECTRICA', 'DOCTORADO EN ESTUDIOS DEL DESARROLLO': 'DOCTORADO EN ESTUDIOS DEL DESARROLLO', 'DERECHO PLANTEL FRESNILLO': 'DERECHO', 'FILOSOFIA': 'FILOSOFIA', 'MEDICINA HUMANA PLANTEL ZACATECAS': 'MEDICINA HUMANA', 'ESTUDIOS DE LAS HUMANIDADES': 'ESTUDIOS DE LAS HUMANIDADES', 'PSICOLOGIA PLANTEL JUAN ALDAMA': 'PSICOLOGIA', 'INGENIERIA I': 'INGENIERIA I', 'CIENCIAS SOCIALES': 'CIENCIAS SOCIALES', 'INGENIERIA ELECTRICA PLANTEL ZACATECAS': 'INGENIERIA ELECTRICA', 'ANTROPOLOGIA': 'ANTROPOLOGIA', 'PSICOLOGIA PLANTEL OJOCALIENTE': 'PSICOLOGIA', 'DOCENCIA SUPERIOR PLANTEL JEREZ': 'DOCENCIA SUPERIOR', 'PSICOLOGIA PLANTEL JALPA': 'PSICOLOGIA', 'DOCENCIA SUPERIOR': 'DOCENCIA SUPERIOR', 'CIENCIAS QUIMICAS': 'CIENCIAS QUIMICAS', 'ARTES': 'ARTES', 'MATEMATICAS': 'MATEMATICAS', 'FISICA': 'FISICA', 'DERECHO PLANTEL ZACATECAS': 'DERECHO', 'ECONOMIA': 'ECONOMIA', 'AGRONOMIA PLANTEL VALPARAISO': 'AGRONOMIA', 'PSICOLOGIA PLANTEL FRESNILLO': 'PSICOLOGIA', 'LETRAS PLANTEL ZACATECAS': 'LETRAS', 'ESTUDIOS NUCLEARES': 'ESTUDIOS NUCLEARES', 'PSICOLOGIA PLANTEL ZACATECAS': 'PSICOLOGIA', 'MEDICINA HUMANA PLANTEL FRESNILLO': 'MEDICINA HUMANA', 'ENFERMERIA PLANTEL ZACATECAS': 'ENFERMERIA', 'CIENCIAS DE LA TIERRA': 'CIENCIAS DE LA TIERRA', 'ODONTOLOGIA': 'ODONTOLOGIA', 'HISTORIA PLANTEL JEREZ': 'HISTORIA', 'ENFERMERIA PLANTEL NOCHISTLAN': 'ENFERMERIA', 'CONTADURIA Y ADMINISTRACION': 'CONTADURIA Y ADMINISTRACION', 'ENFERMERIA PLANTEL JALPA': 'ENFERMERIA', 'CIENCIA POLITICA': 'CIENCIA POLITICA', 'ENFERMERIA PLANTEL JUAN ALDAMA': 'ENFERMERIA', 'MEDICINA VETERINARIA Y ZOOTECNIA': 'MEDICINA VETERINARIA Y ZOOTECNIA', 'MUSICA': 'MUSICA', 'NUTRICION PLANTEL TLALTENANGO': 'NUTRICION', 'CIENCIAS DE LA SALUD': 'CIENCIAS DE LA SALUD'}



```python
Se agrega al final de la tabla el nombre de la unidad homologada.
```


```python
pd.options.mode.chained_assignment = None
def get_unidad_dict (row):
    return dict[row['unidad']]
lic_pos['unidad_agrupada']=lic_pos.apply (lambda row: get_unidad_dict (row),axis=1)
lic_pos

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>semestre</th>
      <th>turno</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
      <th>unidad_agrupada</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12DHL1</td>
      <td>DESARROLLO DE HABILIDADES LINGÜISTICAS I</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2010-12-10 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-18 00:00:00.000</td>
      <td>NP</td>
      <td>2011-01-22 00:00:00.000</td>
      <td>NP</td>
      <td>ECONOMIA</td>
    </tr>
    <tr>
      <th>31</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12ECP1</td>
      <td>ECONOMIA POLITICA I</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-15 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>ECONOMIA</td>
    </tr>
    <tr>
      <th>32</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12HEG1</td>
      <td>HISTORIA ECONOMICA GENERAL I</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-16 00:00:00.000</td>
      <td>NP</td>
      <td>2011-01-20 00:00:00.000</td>
      <td>NP</td>
      <td>ECONOMIA</td>
    </tr>
    <tr>
      <th>33</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12INTE</td>
      <td>INTRODUCCION A LA TEORIA ECONOMICA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-13 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>ECONOMIA</td>
    </tr>
    <tr>
      <th>49</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCMIOD</td>
      <td>MICROBIOLOGIA ODONTOLOGICA</td>
      <td>...</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
    </tr>
    <tr>
      <th>50</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCNUIN</td>
      <td>NUTRICION Y DESARROLLO INFANTIL</td>
      <td>...</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
    </tr>
    <tr>
      <th>55</th>
      <td>26703061</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13DIBU</td>
      <td>DIBUJO</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>NP</td>
      <td>2010-11-30 00:00:00.000</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>167</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PLTV</td>
      <td>PRACTICAS DE LOCALIZACION Y TRAZO DE VIAS</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>168</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PRF1</td>
      <td>PRACTICAS DE FOTOGRAMETRIA I</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>169</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PTSU</td>
      <td>PRACTICAS DE TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>170</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13TOSU</td>
      <td>TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>171</th>
      <td>20000542</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155110</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>1011SNON</td>
      <td>175IC2</td>
      <td>INGENIERIA EN COMPUTACION</td>
      <td>75PROO</td>
      <td>PROGRAMACION ORIENTADA A OBJETOS</td>
      <td>...</td>
      <td>0</td>
      <td>4</td>
      <td>0</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
    </tr>
    <tr>
      <th>197</th>
      <td>28900079</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>5</td>
      <td>2</td>
      <td>1</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
    </tr>
    <tr>
      <th>199</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCHEMA</td>
      <td>HEMATOLOGIA</td>
      <td>...</td>
      <td>5</td>
      <td>2</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
    </tr>
    <tr>
      <th>200</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>5</td>
      <td>2</td>
      <td>1</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
    </tr>
    <tr>
      <th>202</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFT1</td>
      <td>OPTATIVA I DE FORMACION TECNICA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-26 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
    </tr>
    <tr>
      <th>203</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13DSAN</td>
      <td>DISEÑO DE ANTENAS</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
    </tr>
    <tr>
      <th>204</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFP1</td>
      <td>OPTATIVA I DE FORMACION PROFESIONAL</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
    </tr>
    <tr>
      <th>205</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ETIC</td>
      <td>ETICA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
    </tr>
    <tr>
      <th>206</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13CILL</td>
      <td>CIRCUITOS INTEGRADOS LINEALES Y LABORATORIO</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
    </tr>
    <tr>
      <th>207</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ELPL</td>
      <td>ELECTRONICA DE POTENCIA Y LABORATORIO</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
    </tr>
    <tr>
      <th>224</th>
      <td>30111558</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09QIA1</td>
      <td>QUIMICA ANALITICA I</td>
      <td>...</td>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>NP</td>
      <td>2011-12-14 00:00:00.000</td>
      <td>4</td>
      <td>2012-01-18 00:00:00.000</td>
      <td>NP</td>
      <td>CIENCIAS DE LA SALUD</td>
    </tr>
    <tr>
      <th>225</th>
      <td>30111727</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09FIQU</td>
      <td>FISIOLOGIA</td>
      <td>...</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>2</td>
      <td>2011-12-16 00:00:00.000</td>
      <td>NP</td>
      <td>2012-01-20 00:00:00.000</td>
      <td>NP</td>
      <td>CIENCIAS DE LA SALUD</td>
    </tr>
    <tr>
      <th>234</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ASEV</td>
      <td>ASESORIA EDUCATIVA VOCACIONAL</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2011-12-07 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
    </tr>
    <tr>
      <th>235</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DEHE</td>
      <td>DESARROLLO HUMANO EN EDUCACION</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
    </tr>
    <tr>
      <th>236</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DIVO</td>
      <td>DIAGNOSTICO VOCACIONAL</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2011-11-29 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
    </tr>
    <tr>
      <th>237</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INE3</td>
      <td>INVESTIGACION EDUCATIVA III</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2011-12-01 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
    </tr>
    <tr>
      <th>238</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INPR</td>
      <td>INFORMACION PROFESIOGRAFICA</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2011-11-30 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
    </tr>
    <tr>
      <th>239</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORED</td>
      <td>ORIENTACION EDUCATIVA</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2011-12-02 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
    </tr>
    <tr>
      <th>240</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORSE</td>
      <td>ORIENTACION SEXUAL</td>
      <td>...</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5721586</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ESTT</td>
      <td>ESTATICA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-06-03 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721587</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAES</td>
      <td>LABORATORIO DE ESTATICA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-05-29 14:41:09.730</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721588</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMA</td>
      <td>CIENCIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-05-27 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721589</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PEPI</td>
      <td>PROBABILIDAD Y ESTADISTICA PARA INGENIEROS</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-06-04 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721590</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ELMA</td>
      <td>ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-06 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721591</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAEM</td>
      <td>LABORATORIO DE ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-05 14:42:24.020</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721592</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DINM</td>
      <td>DINAMICA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-02 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721593</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LADI</td>
      <td>LABORATORIO DE DINAMICA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-09 14:43:09.483</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721594</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ANAV</td>
      <td>ANALISIS VECTORIAL</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-11-29 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721595</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ECUD</td>
      <td>ECUACIONES DIFERENCIALES</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-04 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721596</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PRTR</td>
      <td>PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-16 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721597</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAPT</td>
      <td>LABORATORIO DE PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-05-28 14:44:38.120</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721598</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13METN</td>
      <td>METODOS NUMERICOS</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-03 14:46:06.560</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721599</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13SILI</td>
      <td>SISTEMAS LINEALES</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-18 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721600</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MECR</td>
      <td>MECANICA DEL CUERPO RIGIDO</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-20 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721601</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DIBM</td>
      <td>DIBUJO MECANICO</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2012-12-05 14:48:46.460</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721602</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13COGI</td>
      <td>COMUNICACION GRAFICA EN INGENIERIA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-05-30 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721603</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MEIA</td>
      <td>METROLOGIA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-11-25 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721604</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAME</td>
      <td>LABORATORIO DE METROLOGIA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-11-26 14:50:03.320</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721605</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE1</td>
      <td>SOFTWARE ESPECIALIZADO I</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2013-11-27 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721606</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INGM</td>
      <td>INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-06 14:51:51.113</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721607</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAIM</td>
      <td>LABORATORIO DE INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-13 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721608</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD1</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2014-06-12 14:53:02.960</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721609</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LSD1</td>
      <td>LABORATORIO DE MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-12 11:48:42.913</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721610</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMC</td>
      <td>CINEMATICA DE MECANISMOS</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-12-05 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721611</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE2</td>
      <td>SOFTWARE ESPECIALIZADO II</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-05 14:59:09.470</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721612</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TEE1</td>
      <td>TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-11-28 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721613</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LTME</td>
      <td>LABORATORIO DE TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-11-27 15:01:33.780</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721614</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INEL</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-11-24 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
    <tr>
      <th>5721615</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD2</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES II</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>2014-12-01 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
    </tr>
  </tbody>
</table>
<p>3169561 rows × 22 columns</p>
</div>



De la misma manera, existen programas que se ofertan de manera semi-escolarizada (**SUA**) o en modalidad a **Distancia**. Tambien se deben homologar estos programas.


```python
programas=lic_pos['programa'].unique()
sorted(programas)
```




    ['CURSOS POSBASICOS EN ENFERMERIA',
     'DOCTORADO EN ADMINISTRACI\xc3\x93N',
     'DOCTORADO EN CIENCIA POLITICA',
     'DOCTORADO EN CIENCIAS AGROPECUARIAS',
     'DOCTORADO EN CIENCIAS BASICAS',
     'DOCTORADO EN CIENCIAS DE LA INGENIERIA',
     'DOCTORADO EN CIENCIAS HUMANISTICAS Y EDUCATIVAS',
     'DOCTORADO EN CIENCIAS PECUARIAS',
     'DOCTORADO EN ESTUDIOS DEL DESARROLLO',
     'DOCTORADO EN ESTUDIOS NOVOHISPANOS',
     'DOCTORADO EN FARMACOLOGIA',
     'DOCTORADO EN HISTORIA',
     'DOCTORADO EN HISTORIA COLONIAL',
     'DOCTORADO EN HUMANIDADES Y ARTES',
     'DOCTORADO EN INGENIERIA Y TECNOLOGIA APLICADA',
     'ESPECIALIDAD DE ENFERMERIA EN ADMINISTRACION Y DOCENCIA',
     'ESPECIALIDAD DE ENFERMERIA EN SALUD PUBLICA',
     'ESPECIALIDAD DE ENFERMERIA QUIRURGICA',
     'ESPECIALIDAD EN ANESTESIOLOGIA (CONVENIO',
     'ESPECIALIDAD EN CIRUGIA (CONVENIO)',
     'ESPECIALIDAD EN COMPUTACION',
     'ESPECIALIDAD EN DERECHO LABORAL',
     'ESPECIALIDAD EN DISE\xc3\x91O MECANICO',
     'ESPECIALIDAD EN ENFERMER\xc3\x8dA GERONTOGERI\xc3\x81TRICA',
     'ESPECIALIDAD EN FISCAL',
     'ESPECIALIDAD EN GINECOOBSTETRICIA (C.)',
     'ESPECIALIDAD EN MEDICINA FAMILIAR (C.)',
     'ESPECIALIDAD EN MEDICINA INTERNA (CONV)',
     'ESPECIALIDAD EN ODONTOPEDIATRIA',
     'ESPECIALIDAD EN PEDIATRIA (CONVENIO)',
     'ESPECIALIDAD EN PROCESOS METALURGICOS DE MANUFACTURA',
     'ESPECIALIDAD EN PRODUCCI\xc3\x93N AGROPECUARIA',
     'ESPECIALIDAD EN TECNOLOGIAS INFORMATICAS APLICADAS A LA EDUCACI\xc3\x93N',
     'ESPECIALIDAD EN TECNOLOGIAS INFORMATICAS EDUCATIVAS',
     'ESPECIALIDAD EN VALUACION CON ORIENTACION EN IMPACTO AMBIENTAL',
     'ESPECIALIDAD EN VALUACION CON ORIENTACION EN INMUEBLES',
     'ESPECIALIDAD EN VALUACION CON ORIENTACION EN INMUEBLES AGROPECUARIOS',
     'ESPECIALIDAD EN VALUACION CON ORIENTACION EN MAQUINARIA Y EQUIPO',
     'ESPECIALIDAD EN VALUACION CON ORIENTACION EN NEGOCIOS EN MARCHA',
     'ESPECIALIDAD EN VALUACION DE INMUEBLES',
     'ING. EN COMUNICACIONES Y ELECTRONICA',
     'INGENIERIA EN DISE\xc3\x91O INDUSTRIAL',
     'INGENIERIA EN ELECTRONICA INDUSTRIAL',
     'INGENIERIA EN ROBOTICA Y MECATRONICA',
     'INGENIERO AGRONOMO',
     'INGENIERO CIVIL',
     'INGENIERO ELECTRICISTA',
     'INGENIERO EN COMPUTACION',
     'INGENIERO GEOLOGO',
     'INGENIERO MECANICO',
     'INGENIERO MINERO METALURGISTA',
     'INGENIERO QUIMICO',
     'INGENIERO TOPOGRAFO E HIDROGRAFO',
     'LICENCIADO EN BIOLOGIA',
     'LICENCIADO EN CIENCIAS AMBIENTALES',
     'LICENCIADO EN CONTADURIA',
     'LICENCIADO EN DERECHO',
     'LICENCIADO EN DESARROLLO CULTURAL',
     'LICENCIADO EN ECONOMIA',
     'LICENCIADO EN ENFERMERIA',
     'LICENCIADO EN FILOSOFIA',
     'LICENCIADO EN FILOSOFIA A DISTANCIA',
     'LICENCIADO EN FISICA',
     'LICENCIADO EN HISTORIA',
     'LICENCIADO EN HISTORIA A DISTANCIA',
     'LICENCIADO EN HUMANIDADES ANTROPOLOGIA',
     'LICENCIADO EN LETRAS',
     'LICENCIADO EN MATEMATICAS',
     'LICENCIADO EN PSICOLOGIA',
     'LICENCIADO EN TURISMO A DISTANCIA',
     'LICENCIATURA EN ARTES',
     'LICENCIATURA EN CANTO',
     'LICENCIATURA EN DESARROLLO REGIONAL SUSTENTABLE',
     'LICENCIATURA EN INFORMATICA',
     'LICENCIATURA EN INGENIERIA DE SOFTWARE',
     'LICENCIATURA EN LENGUAS EXTRANJERAS',
     'LICENCIATURA EN MUSICA (NIVEL PROPEDEUTICO)',
     'LICENCIATURA EN MUSICA (NIVEL SUPERIOR)',
     'LICENCIATURA EN MUSICA CON ESPECIALIZACION EN INSTRUMENTO',
     'LICENCIATURA EN MUSICA NIVEL JUVENIL',
     'LICENCIATURA EN NUTRICI\xc3\x93N',
     'LICENCIATURA EN PERIODISMO',
     'LICENCIATURA EN TURISMO',
     'MAE. EN PRODUCCION ANIMAL EN ZONAS ARIDA',
     'MAES. EN DOCENCIA EN INV. JURIDICAS',
     'MAESTRIA EN ADMINISTRACION',
     'MAESTRIA EN BIOLOGIA EXPERIMENTAL',
     'MAESTRIA EN CIENCIA E INGENIERIA DE LOS MATERIALES',
     'MAESTRIA EN CIENCIA JURIDICO PENAL',
     'MAESTRIA EN CIENCIA POLITICA',
     'MAESTRIA EN CIENCIA Y TECNOLOGIA QUIMICA',
     'MAESTRIA EN CIENCIAS AGRICOLAS',
     'MAESTRIA EN CIENCIAS AGROPECUARIAS',
     'MAESTRIA EN CIENCIAS BIOLOGICAS',
     'MAESTRIA EN CIENCIAS BIOM\xc3\x89DICAS',
     'MAESTRIA EN CIENCIAS DE LA EDUCACION',
     'MAESTRIA EN CIENCIAS DE LA INGENIERIA',
     'MAESTRIA EN CIENCIAS DE LA SALUD',
     'MAESTRIA EN CIENCIAS FISICAS',
     'MAESTRIA EN CIENCIAS NUCLEARES',
     'MAESTRIA EN CIENCIAS SOCIALES',
     'MAESTRIA EN CIENCIAS SOCIALES CON ORIENTACION EN POLITICAS PUBLICAS',
     'MAESTRIA EN DOCENCIA Y PROCESOS INSTITUCIONALES',
     'MAESTRIA EN ECONOMIA',
     'MAESTRIA EN ECONOMIA REGIONAL Y SECTORIAL',
     'MAESTRIA EN ENERGETICOS',
     'MAESTRIA EN ENSE\xc3\x91ANZA DE LA LENGUA MATERNA',
     'MAESTRIA EN ESTUDIOS DE FILOSOFiA EN MEX',
     'MAESTRIA EN ESTUDIOS ELECTORALES',
     'MAESTRIA EN FILOSOFIA',
     'MAESTRIA EN FILOSOFIA E HISTORIA DE LAS IDEAS',
     'MAESTRIA EN FISICA',
     'MAESTRIA EN HISTORIA',
     'MAESTRIA EN HUMANIDADES LINEA FORMACION DOCENTE',
     'MAESTRIA EN HUMANIDADES Y PROCESOS EDUCATIVOS',
     'MAESTRIA EN HUMANIDADES-HISTORIA',
     'MAESTRIA EN IMPUESTOS',
     'MAESTRIA EN INGENIERIA',
     'MAESTRIA EN INGENIERIA APLICADA, CON ORIENTACI\xc3\x93N EN PROCESOS Y MANUFACTURA',
     'MAESTRIA EN INGENIERIA APLICADA, CON ORIENTACI\xc3\x93N EN RECURSOS HIDRAULICOS.',
     'MAESTRIA EN INGENIERIA Y TECNOLOGIA APLICADA',
     'MAESTRIA EN INVESTIGACIONES HUMANISTICAS Y EDUCATIVAS',
     'MAESTRIA EN MATEMATICA APLICADA',
     'MAESTRIA EN MATEMATICAS',
     'MAESTRIA EN MATEMATICAS EDUCATIVAS',
     'MAESTRIA EN PLANEACION DE REC. HIDRAULIC',
     'MAESTRIA EN POBLACION Y DESARROLLO',
     'MAESTRIA EN POBLACION, DESARROLLO Y POLITICAS PUBLICAS',
     'MAESTRIA EN PROCESOS Y MATERIALES',
     'MAESTRIA EN PSICOTERAPIA PSICOANALITICA',
     'MAESTRIA EN TECNOLOGIA INFORMATICA EDUCATIVA',
     'MAESTRIA EN VALUACION',
     'MEDICO CIRUJANO DENTISTA',
     'MEDICO GENERAL',
     'MEDICO VETERINARIO ZOOTECNISTA',
     'QUIMICA EN ALIMENTOS',
     'QUIMICO FARMACEUTICO-BIOLOGO',
     'SUA INGENIERO AGRONOMO',
     'SUA LICENCIADO EN CONTADURIA Y ADMINISTRACION',
     'SUA LICENCIADO EN DERECHO',
     'SUA LICENCIADO EN PSICOLOGIA',
     'SUBESPECIALIDAD EN NEONATOLOGIA',
     'TECNICO EN ENFERMERIA',
     'TRONCO COMUN']




```python
lic_pos[(lic_pos['programa'].str.contains("DISTANCIA"))|(lic_pos['programa'].str.contains("SUA"))].programa.unique()
```




    array(['SUA INGENIERO AGRONOMO', 'SUA LICENCIADO EN DERECHO',
           'SUA LICENCIADO EN PSICOLOGIA',
           'SUA LICENCIADO EN CONTADURIA Y ADMINISTRACION',
           'LICENCIADO EN HISTORIA A DISTANCIA',
           'LICENCIADO EN FILOSOFIA A DISTANCIA',
           'LICENCIADO EN TURISMO A DISTANCIA'], dtype=object)




```python
dict={}
for programa in programas:
    if "SUA" in programa:
        if "CONTADURIA" in programa:
            dict[programa]="LICENCIADO EN CONTADURIA"
        else:
            dict[programa]=programa[4:]
    elif "A DISTANCIA" in programa:
        dict[programa]=programa[:programa.find("A DISTANCIA")-1]
    else:
        dict[programa]=programa
print (dict)
```

    {'MAESTRIA EN ENSE\xc3\x91ANZA DE LA LENGUA MATERNA': 'MAESTRIA EN ENSE\xc3\x91ANZA DE LA LENGUA MATERNA', 'DOCTORADO EN FARMACOLOGIA': 'DOCTORADO EN FARMACOLOGIA', 'MAESTRIA EN MATEMATICA APLICADA': 'MAESTRIA EN MATEMATICA APLICADA', 'MAESTRIA EN INGENIERIA APLICADA, CON ORIENTACI\xc3\x93N EN RECURSOS HIDRAULICOS.': 'MAESTRIA EN INGENIERIA APLICADA, CON ORIENTACI\xc3\x93N EN RECURSOS HIDRAULICOS.', 'MAESTRIA EN PROCESOS Y MATERIALES': 'MAESTRIA EN PROCESOS Y MATERIALES', 'SUA INGENIERO AGRONOMO': 'INGENIERO AGRONOMO', 'MAESTRIA EN CIENCIAS BIOM\xc3\x89DICAS': 'MAESTRIA EN CIENCIAS BIOM\xc3\x89DICAS', 'ESPECIALIDAD EN DISE\xc3\x91O MECANICO': 'ESPECIALIDAD EN DISE\xc3\x91O MECANICO', 'CURSOS POSBASICOS EN ENFERMERIA': 'CURSOS POSBASICOS EN ENFERMERIA', 'MAESTRIA EN VALUACION': 'MAESTRIA EN VALUACION', 'ESPECIALIDAD EN CIRUGIA (CONVENIO)': 'ESPECIALIDAD EN CIRUGIA (CONVENIO)', 'MAESTRIA EN DOCENCIA Y PROCESOS INSTITUCIONALES': 'MAESTRIA EN DOCENCIA Y PROCESOS INSTITUCIONALES', 'LICENCIADO EN TURISMO A DISTANCIA': 'LICENCIADO EN TURISMO', 'MAESTRIA EN INGENIERIA APLICADA, CON ORIENTACI\xc3\x93N EN PROCESOS Y MANUFACTURA': 'MAESTRIA EN INGENIERIA APLICADA, CON ORIENTACI\xc3\x93N EN PROCESOS Y MANUFACTURA', 'MAESTRIA EN CIENCIA JURIDICO PENAL': 'MAESTRIA EN CIENCIA JURIDICO PENAL', 'INGENIERIA EN DISE\xc3\x91O INDUSTRIAL': 'INGENIERIA EN DISE\xc3\x91O INDUSTRIAL', 'LICENCIADO EN FISICA': 'LICENCIADO EN FISICA', 'ESPECIALIDAD EN TECNOLOGIAS INFORMATICAS EDUCATIVAS': 'ESPECIALIDAD EN TECNOLOGIAS INFORMATICAS EDUCATIVAS', 'LICENCIATURA EN INGENIERIA DE SOFTWARE': 'LICENCIATURA EN INGENIERIA DE SOFTWARE', 'ESPECIALIDAD EN COMPUTACION': 'ESPECIALIDAD EN COMPUTACION', 'SUA LICENCIADO EN DERECHO': 'LICENCIADO EN DERECHO', 'MAESTRIA EN CIENCIAS FISICAS': 'MAESTRIA EN CIENCIAS FISICAS', 'MAESTRIA EN CIENCIAS NUCLEARES': 'MAESTRIA EN CIENCIAS NUCLEARES', 'LICENCIATURA EN LENGUAS EXTRANJERAS': 'LICENCIATURA EN LENGUAS EXTRANJERAS', 'LICENCIATURA EN INFORMATICA': 'LICENCIATURA EN INFORMATICA', 'DOCTORADO EN CIENCIAS DE LA INGENIERIA': 'DOCTORADO EN CIENCIAS DE LA INGENIERIA', 'ESPECIALIDAD EN DERECHO LABORAL': 'ESPECIALIDAD EN DERECHO LABORAL', 'MAESTRIA EN POBLACION Y DESARROLLO': 'MAESTRIA EN POBLACION Y DESARROLLO', 'SUA LICENCIADO EN CONTADURIA Y ADMINISTRACION': 'LICENCIADO EN CONTADURIA', 'ESPECIALIDAD EN ANESTESIOLOGIA (CONVENIO': 'ESPECIALIDAD EN ANESTESIOLOGIA (CONVENIO', 'ESPECIALIDAD EN MEDICINA INTERNA (CONV)': 'ESPECIALIDAD EN MEDICINA INTERNA (CONV)', 'MAES. EN DOCENCIA EN INV. JURIDICAS': 'MAES. EN DOCENCIA EN INV. JURIDICAS', 'LICENCIATURA EN MUSICA (NIVEL PROPEDEUTICO)': 'LICENCIATURA EN MUSICA (NIVEL PROPEDEUTICO)', 'LICENCIATURA EN CANTO': 'LICENCIATURA EN CANTO', 'MAESTRIA EN ENERGETICOS': 'MAESTRIA EN ENERGETICOS', 'MAESTRIA EN MATEMATICAS': 'MAESTRIA EN MATEMATICAS', 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN NEGOCIOS EN MARCHA': 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN NEGOCIOS EN MARCHA', 'MAESTRIA EN CIENCIA Y TECNOLOGIA QUIMICA': 'MAESTRIA EN CIENCIA Y TECNOLOGIA QUIMICA', 'MAESTRIA EN HISTORIA': 'MAESTRIA EN HISTORIA', 'DOCTORADO EN ESTUDIOS NOVOHISPANOS': 'DOCTORADO EN ESTUDIOS NOVOHISPANOS', 'MAESTRIA EN ESTUDIOS ELECTORALES': 'MAESTRIA EN ESTUDIOS ELECTORALES', 'INGENIERO EN COMPUTACION': 'INGENIERO EN COMPUTACION', 'LICENCIADO EN HISTORIA A DISTANCIA': 'LICENCIADO EN HISTORIA', 'MAESTRIA EN PLANEACION DE REC. HIDRAULIC': 'MAESTRIA EN PLANEACION DE REC. HIDRAULIC', 'TRONCO COMUN': 'TRONCO COMUN', 'MAESTRIA EN MATEMATICAS EDUCATIVAS': 'MAESTRIA EN MATEMATICAS EDUCATIVAS', 'ESPECIALIDAD EN PROCESOS METALURGICOS DE MANUFACTURA': 'ESPECIALIDAD EN PROCESOS METALURGICOS DE MANUFACTURA', 'LICENCIATURA EN ARTES': 'LICENCIATURA EN ARTES', 'LICENCIADO EN DERECHO': 'LICENCIADO EN DERECHO', 'MAESTRIA EN ECONOMIA': 'MAESTRIA EN ECONOMIA', 'ESPECIALIDAD DE ENFERMERIA EN ADMINISTRACION Y DOCENCIA': 'ESPECIALIDAD DE ENFERMERIA EN ADMINISTRACION Y DOCENCIA', 'LICENCIADO EN HUMANIDADES ANTROPOLOGIA': 'LICENCIADO EN HUMANIDADES ANTROPOLOGIA', 'MAESTRIA EN CIENCIAS BIOLOGICAS': 'MAESTRIA EN CIENCIAS BIOLOGICAS', 'LICENCIATURA EN MUSICA NIVEL JUVENIL': 'LICENCIATURA EN MUSICA NIVEL JUVENIL', 'DOCTORADO EN CIENCIA POLITICA': 'DOCTORADO EN CIENCIA POLITICA', 'ESPECIALIDAD EN FISCAL': 'ESPECIALIDAD EN FISCAL', 'MAESTRIA EN HUMANIDADES LINEA FORMACION DOCENTE': 'MAESTRIA EN HUMANIDADES LINEA FORMACION DOCENTE', 'ESPECIALIDAD EN ODONTOPEDIATRIA': 'ESPECIALIDAD EN ODONTOPEDIATRIA', 'INGENIERO CIVIL': 'INGENIERO CIVIL', 'DOCTORADO EN ADMINISTRACI\xc3\x93N': 'DOCTORADO EN ADMINISTRACI\xc3\x93N', 'SUBESPECIALIDAD EN NEONATOLOGIA': 'SUBESPECIALIDAD EN NEONATOLOGIA', 'ESPECIALIDAD DE ENFERMERIA EN SALUD PUBLICA': 'ESPECIALIDAD DE ENFERMERIA EN SALUD PUBLICA', 'LICENCIADO EN ENFERMERIA': 'LICENCIADO EN ENFERMERIA', 'ESPECIALIDAD EN GINECOOBSTETRICIA (C.)': 'ESPECIALIDAD EN GINECOOBSTETRICIA (C.)', 'TECNICO EN ENFERMERIA': 'TECNICO EN ENFERMERIA', 'MAESTRIA EN INGENIERIA': 'MAESTRIA EN INGENIERIA', 'MAESTRIA EN CIENCIAS SOCIALES CON ORIENTACION EN POLITICAS PUBLICAS': 'MAESTRIA EN CIENCIAS SOCIALES CON ORIENTACION EN POLITICAS PUBLICAS', 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN INMUEBLES AGROPECUARIOS': 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN INMUEBLES AGROPECUARIOS', 'ESPECIALIDAD EN ENFERMER\xc3\x8dA GERONTOGERI\xc3\x81TRICA': 'ESPECIALIDAD EN ENFERMER\xc3\x8dA GERONTOGERI\xc3\x81TRICA', 'MEDICO GENERAL': 'MEDICO GENERAL', 'MAESTRIA EN FILOSOFIA': 'MAESTRIA EN FILOSOFIA', 'INGENIERO MINERO METALURGISTA': 'INGENIERO MINERO METALURGISTA', 'LICENCIADO EN LETRAS': 'LICENCIADO EN LETRAS', 'LICENCIADO EN HISTORIA': 'LICENCIADO EN HISTORIA', 'LICENCIADO EN CIENCIAS AMBIENTALES': 'LICENCIADO EN CIENCIAS AMBIENTALES', 'LICENCIADO EN DESARROLLO CULTURAL': 'LICENCIADO EN DESARROLLO CULTURAL', 'MAESTRIA EN BIOLOGIA EXPERIMENTAL': 'MAESTRIA EN BIOLOGIA EXPERIMENTAL', 'MAESTRIA EN INVESTIGACIONES HUMANISTICAS Y EDUCATIVAS': 'MAESTRIA EN INVESTIGACIONES HUMANISTICAS Y EDUCATIVAS', 'MAESTRIA EN TECNOLOGIA INFORMATICA EDUCATIVA': 'MAESTRIA EN TECNOLOGIA INFORMATICA EDUCATIVA', 'ESPECIALIDAD EN VALUACION DE INMUEBLES': 'ESPECIALIDAD EN VALUACION DE INMUEBLES', 'MAESTRIA EN ESTUDIOS DE FILOSOFiA EN MEX': 'MAESTRIA EN ESTUDIOS DE FILOSOFiA EN MEX', 'MAESTRIA EN ADMINISTRACION': 'MAESTRIA EN ADMINISTRACION', 'MAESTRIA EN IMPUESTOS': 'MAESTRIA EN IMPUESTOS', 'MAESTRIA EN CIENCIAS AGRICOLAS': 'MAESTRIA EN CIENCIAS AGRICOLAS', 'LICENCIATURA EN MUSICA CON ESPECIALIZACION EN INSTRUMENTO': 'LICENCIATURA EN MUSICA CON ESPECIALIZACION EN INSTRUMENTO', 'LICENCIATURA EN MUSICA (NIVEL SUPERIOR)': 'LICENCIATURA EN MUSICA (NIVEL SUPERIOR)', 'DOCTORADO EN INGENIERIA Y TECNOLOGIA APLICADA': 'DOCTORADO EN INGENIERIA Y TECNOLOGIA APLICADA', 'ESPECIALIDAD DE ENFERMERIA QUIRURGICA': 'ESPECIALIDAD DE ENFERMERIA QUIRURGICA', 'DOCTORADO EN HISTORIA COLONIAL': 'DOCTORADO EN HISTORIA COLONIAL', 'LICENCIATURA EN TURISMO': 'LICENCIATURA EN TURISMO', 'MAESTRIA EN POBLACION, DESARROLLO Y POLITICAS PUBLICAS': 'MAESTRIA EN POBLACION, DESARROLLO Y POLITICAS PUBLICAS', 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN IMPACTO AMBIENTAL': 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN IMPACTO AMBIENTAL', 'DOCTORADO EN CIENCIAS AGROPECUARIAS': 'DOCTORADO EN CIENCIAS AGROPECUARIAS', 'INGENIERO GEOLOGO': 'INGENIERO GEOLOGO', 'MEDICO CIRUJANO DENTISTA': 'MEDICO CIRUJANO DENTISTA', 'MAESTRIA EN PSICOTERAPIA PSICOANALITICA': 'MAESTRIA EN PSICOTERAPIA PSICOANALITICA', 'ING. EN COMUNICACIONES Y ELECTRONICA': 'ING. EN COMUNICACIONES Y ELECTRONICA', 'LICENCIADO EN FILOSOFIA A DISTANCIA': 'LICENCIADO EN FILOSOFIA', 'LICENCIATURA EN NUTRICI\xc3\x93N': 'LICENCIATURA EN NUTRICI\xc3\x93N', 'MAESTRIA EN CIENCIA POLITICA': 'MAESTRIA EN CIENCIA POLITICA', 'DOCTORADO EN HISTORIA': 'DOCTORADO EN HISTORIA', 'INGENIERO TOPOGRAFO E HIDROGRAFO': 'INGENIERO TOPOGRAFO E HIDROGRAFO', 'MAESTRIA EN FISICA': 'MAESTRIA EN FISICA', 'MAESTRIA EN HUMANIDADES Y PROCESOS EDUCATIVOS': 'MAESTRIA EN HUMANIDADES Y PROCESOS EDUCATIVOS', 'MAESTRIA EN CIENCIAS AGROPECUARIAS': 'MAESTRIA EN CIENCIAS AGROPECUARIAS', 'ESPECIALIDAD EN TECNOLOGIAS INFORMATICAS APLICADAS A LA EDUCACI\xc3\x93N': 'ESPECIALIDAD EN TECNOLOGIAS INFORMATICAS APLICADAS A LA EDUCACI\xc3\x93N', 'DOCTORADO EN HUMANIDADES Y ARTES': 'DOCTORADO EN HUMANIDADES Y ARTES', 'MEDICO VETERINARIO ZOOTECNISTA': 'MEDICO VETERINARIO ZOOTECNISTA', 'MAESTRIA EN CIENCIAS SOCIALES': 'MAESTRIA EN CIENCIAS SOCIALES', 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN INMUEBLES': 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN INMUEBLES', 'QUIMICA EN ALIMENTOS': 'QUIMICA EN ALIMENTOS', 'DOCTORADO EN ESTUDIOS DEL DESARROLLO': 'DOCTORADO EN ESTUDIOS DEL DESARROLLO', 'MAESTRIA EN INGENIERIA Y TECNOLOGIA APLICADA': 'MAESTRIA EN INGENIERIA Y TECNOLOGIA APLICADA', 'MAESTRIA EN HUMANIDADES-HISTORIA': 'MAESTRIA EN HUMANIDADES-HISTORIA', 'LICENCIADO EN MATEMATICAS': 'LICENCIADO EN MATEMATICAS', 'LICENCIADO EN PSICOLOGIA': 'LICENCIADO EN PSICOLOGIA', 'DOCTORADO EN CIENCIAS HUMANISTICAS Y EDUCATIVAS': 'DOCTORADO EN CIENCIAS HUMANISTICAS Y EDUCATIVAS', 'ESPECIALIDAD EN PEDIATRIA (CONVENIO)': 'ESPECIALIDAD EN PEDIATRIA (CONVENIO)', 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN MAQUINARIA Y EQUIPO': 'ESPECIALIDAD EN VALUACION CON ORIENTACION EN MAQUINARIA Y EQUIPO', 'INGENIERIA EN ROBOTICA Y MECATRONICA': 'INGENIERIA EN ROBOTICA Y MECATRONICA', 'MAESTRIA EN FILOSOFIA E HISTORIA DE LAS IDEAS': 'MAESTRIA EN FILOSOFIA E HISTORIA DE LAS IDEAS', 'MAESTRIA EN CIENCIAS DE LA INGENIERIA': 'MAESTRIA EN CIENCIAS DE LA INGENIERIA', 'INGENIERO ELECTRICISTA': 'INGENIERO ELECTRICISTA', 'MAESTRIA EN CIENCIA E INGENIERIA DE LOS MATERIALES': 'MAESTRIA EN CIENCIA E INGENIERIA DE LOS MATERIALES', 'MAESTRIA EN CIENCIAS DE LA EDUCACION': 'MAESTRIA EN CIENCIAS DE LA EDUCACION', 'INGENIERO AGRONOMO': 'INGENIERO AGRONOMO', 'INGENIERIA EN ELECTRONICA INDUSTRIAL': 'INGENIERIA EN ELECTRONICA INDUSTRIAL', 'LICENCIATURA EN DESARROLLO REGIONAL SUSTENTABLE': 'LICENCIATURA EN DESARROLLO REGIONAL SUSTENTABLE', 'LICENCIADO EN FILOSOFIA': 'LICENCIADO EN FILOSOFIA', 'ESPECIALIDAD EN MEDICINA FAMILIAR (C.)': 'ESPECIALIDAD EN MEDICINA FAMILIAR (C.)', 'DOCTORADO EN CIENCIAS BASICAS': 'DOCTORADO EN CIENCIAS BASICAS', 'INGENIERO QUIMICO': 'INGENIERO QUIMICO', 'DOCTORADO EN CIENCIAS PECUARIAS': 'DOCTORADO EN CIENCIAS PECUARIAS', 'ESPECIALIDAD EN PRODUCCI\xc3\x93N AGROPECUARIA': 'ESPECIALIDAD EN PRODUCCI\xc3\x93N AGROPECUARIA', 'LICENCIADO EN CONTADURIA': 'LICENCIADO EN CONTADURIA', 'SUA LICENCIADO EN PSICOLOGIA': 'LICENCIADO EN PSICOLOGIA', 'MAESTRIA EN CIENCIAS DE LA SALUD': 'MAESTRIA EN CIENCIAS DE LA SALUD', 'LICENCIADO EN ECONOMIA': 'LICENCIADO EN ECONOMIA', 'INGENIERO MECANICO': 'INGENIERO MECANICO', 'LICENCIATURA EN PERIODISMO': 'LICENCIATURA EN PERIODISMO', 'MAESTRIA EN ECONOMIA REGIONAL Y SECTORIAL': 'MAESTRIA EN ECONOMIA REGIONAL Y SECTORIAL', 'QUIMICO FARMACEUTICO-BIOLOGO': 'QUIMICO FARMACEUTICO-BIOLOGO', 'LICENCIADO EN BIOLOGIA': 'LICENCIADO EN BIOLOGIA', 'MAE. EN PRODUCCION ANIMAL EN ZONAS ARIDA': 'MAE. EN PRODUCCION ANIMAL EN ZONAS ARIDA'}


Se agrega al final de la tabla el nombre del programa homologado.


```python
def get_programa_dict (row):
    return dict[row['programa']]
lic_pos['programa_agrupado']=lic_pos.apply (lambda row: get_programa_dict (row),axis=1)
lic_pos
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>turno</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
      <th>unidad_agrupada</th>
      <th>programa_agrupado</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12DHL1</td>
      <td>DESARROLLO DE HABILIDADES LINGÜISTICAS I</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>2010-12-10 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-18 00:00:00.000</td>
      <td>NP</td>
      <td>2011-01-22 00:00:00.000</td>
      <td>NP</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
    </tr>
    <tr>
      <th>31</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12ECP1</td>
      <td>ECONOMIA POLITICA I</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-15 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
    </tr>
    <tr>
      <th>32</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12HEG1</td>
      <td>HISTORIA ECONOMICA GENERAL I</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-16 00:00:00.000</td>
      <td>NP</td>
      <td>2011-01-20 00:00:00.000</td>
      <td>NP</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
    </tr>
    <tr>
      <th>33</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12INTE</td>
      <td>INTRODUCCION A LA TEORIA ECONOMICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-13 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
    </tr>
    <tr>
      <th>49</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCMIOD</td>
      <td>MICROBIOLOGIA ODONTOLOGICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
    </tr>
    <tr>
      <th>50</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCNUIN</td>
      <td>NUTRICION Y DESARROLLO INFANTIL</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
    </tr>
    <tr>
      <th>55</th>
      <td>26703061</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13DIBU</td>
      <td>DIBUJO</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>NP</td>
      <td>2010-11-30 00:00:00.000</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
    </tr>
    <tr>
      <th>167</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PLTV</td>
      <td>PRACTICAS DE LOCALIZACION Y TRAZO DE VIAS</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
    </tr>
    <tr>
      <th>168</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PRF1</td>
      <td>PRACTICAS DE FOTOGRAMETRIA I</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
    </tr>
    <tr>
      <th>169</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PTSU</td>
      <td>PRACTICAS DE TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
    </tr>
    <tr>
      <th>170</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13TOSU</td>
      <td>TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
    </tr>
    <tr>
      <th>171</th>
      <td>20000542</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155110</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>1011SNON</td>
      <td>175IC2</td>
      <td>INGENIERIA EN COMPUTACION</td>
      <td>75PROO</td>
      <td>PROGRAMACION ORIENTADA A OBJETOS</td>
      <td>...</td>
      <td>4</td>
      <td>0</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>INGENIERO EN COMPUTACION</td>
    </tr>
    <tr>
      <th>197</th>
      <td>28900079</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>2</td>
      <td>1</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
    </tr>
    <tr>
      <th>199</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCHEMA</td>
      <td>HEMATOLOGIA</td>
      <td>...</td>
      <td>2</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
    </tr>
    <tr>
      <th>200</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>2</td>
      <td>1</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
    </tr>
    <tr>
      <th>202</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFT1</td>
      <td>OPTATIVA I DE FORMACION TECNICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-26 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
    </tr>
    <tr>
      <th>203</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13DSAN</td>
      <td>DISEÑO DE ANTENAS</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
    </tr>
    <tr>
      <th>204</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFP1</td>
      <td>OPTATIVA I DE FORMACION PROFESIONAL</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
    </tr>
    <tr>
      <th>205</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ETIC</td>
      <td>ETICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
    </tr>
    <tr>
      <th>206</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13CILL</td>
      <td>CIRCUITOS INTEGRADOS LINEALES Y LABORATORIO</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
    </tr>
    <tr>
      <th>207</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ELPL</td>
      <td>ELECTRONICA DE POTENCIA Y LABORATORIO</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
    </tr>
    <tr>
      <th>224</th>
      <td>30111558</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09QIA1</td>
      <td>QUIMICA ANALITICA I</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>NP</td>
      <td>2011-12-14 00:00:00.000</td>
      <td>4</td>
      <td>2012-01-18 00:00:00.000</td>
      <td>NP</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
    </tr>
    <tr>
      <th>225</th>
      <td>30111727</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09FIQU</td>
      <td>FISIOLOGIA</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>2</td>
      <td>2011-12-16 00:00:00.000</td>
      <td>NP</td>
      <td>2012-01-20 00:00:00.000</td>
      <td>NP</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
    </tr>
    <tr>
      <th>234</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ASEV</td>
      <td>ASESORIA EDUCATIVA VOCACIONAL</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2011-12-07 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
    </tr>
    <tr>
      <th>235</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DEHE</td>
      <td>DESARROLLO HUMANO EN EDUCACION</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
    </tr>
    <tr>
      <th>236</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DIVO</td>
      <td>DIAGNOSTICO VOCACIONAL</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2011-11-29 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
    </tr>
    <tr>
      <th>237</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INE3</td>
      <td>INVESTIGACION EDUCATIVA III</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2011-12-01 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
    </tr>
    <tr>
      <th>238</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INPR</td>
      <td>INFORMACION PROFESIOGRAFICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2011-11-30 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
    </tr>
    <tr>
      <th>239</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORED</td>
      <td>ORIENTACION EDUCATIVA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2011-12-02 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
    </tr>
    <tr>
      <th>240</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORSE</td>
      <td>ORIENTACION SEXUAL</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5721586</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ESTT</td>
      <td>ESTATICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-06-03 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721587</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAES</td>
      <td>LABORATORIO DE ESTATICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-05-29 14:41:09.730</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721588</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMA</td>
      <td>CIENCIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-05-27 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721589</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PEPI</td>
      <td>PROBABILIDAD Y ESTADISTICA PARA INGENIEROS</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-06-04 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721590</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ELMA</td>
      <td>ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-06 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721591</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAEM</td>
      <td>LABORATORIO DE ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-05 14:42:24.020</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721592</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DINM</td>
      <td>DINAMICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-02 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721593</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LADI</td>
      <td>LABORATORIO DE DINAMICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-09 14:43:09.483</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721594</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ANAV</td>
      <td>ANALISIS VECTORIAL</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-11-29 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721595</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ECUD</td>
      <td>ECUACIONES DIFERENCIALES</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-04 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721596</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PRTR</td>
      <td>PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-16 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721597</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAPT</td>
      <td>LABORATORIO DE PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-05-28 14:44:38.120</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721598</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13METN</td>
      <td>METODOS NUMERICOS</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-12-03 14:46:06.560</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721599</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13SILI</td>
      <td>SISTEMAS LINEALES</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-18 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721600</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MECR</td>
      <td>MECANICA DEL CUERPO RIGIDO</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-20 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721601</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DIBM</td>
      <td>DIBUJO MECANICO</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2012-12-05 14:48:46.460</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721602</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13COGI</td>
      <td>COMUNICACION GRAFICA EN INGENIERIA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-05-30 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721603</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MEIA</td>
      <td>METROLOGIA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-11-25 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721604</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAME</td>
      <td>LABORATORIO DE METROLOGIA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-11-26 14:50:03.320</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721605</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE1</td>
      <td>SOFTWARE ESPECIALIZADO I</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2013-11-27 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721606</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INGM</td>
      <td>INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-06 14:51:51.113</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721607</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAIM</td>
      <td>LABORATORIO DE INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-13 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721608</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD1</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2014-06-12 14:53:02.960</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721609</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LSD1</td>
      <td>LABORATORIO DE MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-12 11:48:42.913</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721610</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMC</td>
      <td>CINEMATICA DE MECANISMOS</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-12-05 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721611</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE2</td>
      <td>SOFTWARE ESPECIALIZADO II</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-06-05 14:59:09.470</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721612</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TEE1</td>
      <td>TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-11-28 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721613</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LTME</td>
      <td>LABORATORIO DE TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-11-27 15:01:33.780</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721614</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INEL</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-11-24 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
    <tr>
      <th>5721615</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD2</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES II</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>2014-12-01 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
    </tr>
  </tbody>
</table>
<p>3169561 rows × 23 columns</p>
</div>



Por cuestiones practicas, se agregaran los nombres de los ciclos y el año al que pertenecen.


```python
def get_ciclo_nombre(row):
    clave=row['clave_ciclo']
    texto=""
    if "SNON" in clave:
        texto="AGOSTO-DICIEMBRE 20"+clave[:2]
    else:
        texto="ENERO-JULIO 20"+clave[2:4]
    return texto
lic_pos['ciclo']=lic_pos.apply (lambda row: get_ciclo_nombre (row),axis=1)
lic_pos
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>aprobado</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
      <th>unidad_agrupada</th>
      <th>programa_agrupado</th>
      <th>ciclo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12DHL1</td>
      <td>DESARROLLO DE HABILIDADES LINGÜISTICAS I</td>
      <td>...</td>
      <td>0</td>
      <td>2010-12-10 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-18 00:00:00.000</td>
      <td>NP</td>
      <td>2011-01-22 00:00:00.000</td>
      <td>NP</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>31</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12ECP1</td>
      <td>ECONOMIA POLITICA I</td>
      <td>...</td>
      <td>1</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-15 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>32</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12HEG1</td>
      <td>HISTORIA ECONOMICA GENERAL I</td>
      <td>...</td>
      <td>0</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-16 00:00:00.000</td>
      <td>NP</td>
      <td>2011-01-20 00:00:00.000</td>
      <td>NP</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>33</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12INTE</td>
      <td>INTRODUCCION A LA TEORIA ECONOMICA</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-13 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>49</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCMIOD</td>
      <td>MICROBIOLOGIA ODONTOLOGICA</td>
      <td>...</td>
      <td>1</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>50</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCNUIN</td>
      <td>NUTRICION Y DESARROLLO INFANTIL</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>55</th>
      <td>26703061</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13DIBU</td>
      <td>DIBUJO</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>NP</td>
      <td>2010-11-30 00:00:00.000</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>167</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PLTV</td>
      <td>PRACTICAS DE LOCALIZACION Y TRAZO DE VIAS</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>168</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PRF1</td>
      <td>PRACTICAS DE FOTOGRAMETRIA I</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>169</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PTSU</td>
      <td>PRACTICAS DE TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>170</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13TOSU</td>
      <td>TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>171</th>
      <td>20000542</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155110</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>1011SNON</td>
      <td>175IC2</td>
      <td>INGENIERIA EN COMPUTACION</td>
      <td>75PROO</td>
      <td>PROGRAMACION ORIENTADA A OBJETOS</td>
      <td>...</td>
      <td>0</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>197</th>
      <td>28900079</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>199</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCHEMA</td>
      <td>HEMATOLOGIA</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>200</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>202</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFT1</td>
      <td>OPTATIVA I DE FORMACION TECNICA</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-26 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>203</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13DSAN</td>
      <td>DISEÑO DE ANTENAS</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>204</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFP1</td>
      <td>OPTATIVA I DE FORMACION PROFESIONAL</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>205</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ETIC</td>
      <td>ETICA</td>
      <td>...</td>
      <td>1</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>206</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13CILL</td>
      <td>CIRCUITOS INTEGRADOS LINEALES Y LABORATORIO</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>207</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ELPL</td>
      <td>ELECTRONICA DE POTENCIA Y LABORATORIO</td>
      <td>...</td>
      <td>1</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
    </tr>
    <tr>
      <th>224</th>
      <td>30111558</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09QIA1</td>
      <td>QUIMICA ANALITICA I</td>
      <td>...</td>
      <td>0</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>NP</td>
      <td>2011-12-14 00:00:00.000</td>
      <td>4</td>
      <td>2012-01-18 00:00:00.000</td>
      <td>NP</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
    </tr>
    <tr>
      <th>225</th>
      <td>30111727</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09FIQU</td>
      <td>FISIOLOGIA</td>
      <td>...</td>
      <td>0</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>2</td>
      <td>2011-12-16 00:00:00.000</td>
      <td>NP</td>
      <td>2012-01-20 00:00:00.000</td>
      <td>NP</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
    </tr>
    <tr>
      <th>234</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ASEV</td>
      <td>ASESORIA EDUCATIVA VOCACIONAL</td>
      <td>...</td>
      <td>1</td>
      <td>2011-12-07 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
    </tr>
    <tr>
      <th>235</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DEHE</td>
      <td>DESARROLLO HUMANO EN EDUCACION</td>
      <td>...</td>
      <td>1</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
    </tr>
    <tr>
      <th>236</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DIVO</td>
      <td>DIAGNOSTICO VOCACIONAL</td>
      <td>...</td>
      <td>1</td>
      <td>2011-11-29 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
    </tr>
    <tr>
      <th>237</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INE3</td>
      <td>INVESTIGACION EDUCATIVA III</td>
      <td>...</td>
      <td>1</td>
      <td>2011-12-01 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
    </tr>
    <tr>
      <th>238</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INPR</td>
      <td>INFORMACION PROFESIOGRAFICA</td>
      <td>...</td>
      <td>1</td>
      <td>2011-11-30 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
    </tr>
    <tr>
      <th>239</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORED</td>
      <td>ORIENTACION EDUCATIVA</td>
      <td>...</td>
      <td>1</td>
      <td>2011-12-02 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
    </tr>
    <tr>
      <th>240</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORSE</td>
      <td>ORIENTACION SEXUAL</td>
      <td>...</td>
      <td>1</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5721586</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ESTT</td>
      <td>ESTATICA</td>
      <td>...</td>
      <td>1</td>
      <td>2013-06-03 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
    </tr>
    <tr>
      <th>5721587</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAES</td>
      <td>LABORATORIO DE ESTATICA</td>
      <td>...</td>
      <td>1</td>
      <td>2013-05-29 14:41:09.730</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
    </tr>
    <tr>
      <th>5721588</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMA</td>
      <td>CIENCIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>1</td>
      <td>2013-05-27 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
    </tr>
    <tr>
      <th>5721589</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PEPI</td>
      <td>PROBABILIDAD Y ESTADISTICA PARA INGENIEROS</td>
      <td>...</td>
      <td>1</td>
      <td>2013-06-04 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
    </tr>
    <tr>
      <th>5721590</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ELMA</td>
      <td>ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>1</td>
      <td>2013-12-06 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721591</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAEM</td>
      <td>LABORATORIO DE ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>1</td>
      <td>2013-12-05 14:42:24.020</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721592</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DINM</td>
      <td>DINAMICA</td>
      <td>...</td>
      <td>1</td>
      <td>2013-12-02 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721593</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LADI</td>
      <td>LABORATORIO DE DINAMICA</td>
      <td>...</td>
      <td>1</td>
      <td>2013-12-09 14:43:09.483</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721594</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ANAV</td>
      <td>ANALISIS VECTORIAL</td>
      <td>...</td>
      <td>1</td>
      <td>2013-11-29 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721595</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ECUD</td>
      <td>ECUACIONES DIFERENCIALES</td>
      <td>...</td>
      <td>1</td>
      <td>2013-12-04 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721596</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PRTR</td>
      <td>PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>1</td>
      <td>2014-06-16 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
    </tr>
    <tr>
      <th>5721597</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAPT</td>
      <td>LABORATORIO DE PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>1</td>
      <td>2014-05-28 14:44:38.120</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
    </tr>
    <tr>
      <th>5721598</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13METN</td>
      <td>METODOS NUMERICOS</td>
      <td>...</td>
      <td>1</td>
      <td>2013-12-03 14:46:06.560</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721599</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13SILI</td>
      <td>SISTEMAS LINEALES</td>
      <td>...</td>
      <td>1</td>
      <td>2014-06-18 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
    </tr>
    <tr>
      <th>5721600</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MECR</td>
      <td>MECANICA DEL CUERPO RIGIDO</td>
      <td>...</td>
      <td>1</td>
      <td>2014-06-20 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
    </tr>
    <tr>
      <th>5721601</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DIBM</td>
      <td>DIBUJO MECANICO</td>
      <td>...</td>
      <td>1</td>
      <td>2012-12-05 14:48:46.460</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2012</td>
    </tr>
    <tr>
      <th>5721602</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13COGI</td>
      <td>COMUNICACION GRAFICA EN INGENIERIA</td>
      <td>...</td>
      <td>1</td>
      <td>2013-05-30 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
    </tr>
    <tr>
      <th>5721603</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MEIA</td>
      <td>METROLOGIA</td>
      <td>...</td>
      <td>1</td>
      <td>2013-11-25 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721604</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAME</td>
      <td>LABORATORIO DE METROLOGIA</td>
      <td>...</td>
      <td>1</td>
      <td>2013-11-26 14:50:03.320</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721605</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE1</td>
      <td>SOFTWARE ESPECIALIZADO I</td>
      <td>...</td>
      <td>1</td>
      <td>2013-11-27 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
    </tr>
    <tr>
      <th>5721606</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INGM</td>
      <td>INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>1</td>
      <td>2014-06-06 14:51:51.113</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
    </tr>
    <tr>
      <th>5721607</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAIM</td>
      <td>LABORATORIO DE INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>1</td>
      <td>2014-06-13 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
    </tr>
    <tr>
      <th>5721608</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD1</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2014-06-12 14:53:02.960</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
    </tr>
    <tr>
      <th>5721609</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LSD1</td>
      <td>LABORATORIO DE MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>1</td>
      <td>2014-06-12 11:48:42.913</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
    </tr>
    <tr>
      <th>5721610</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMC</td>
      <td>CINEMATICA DE MECANISMOS</td>
      <td>...</td>
      <td>1</td>
      <td>2014-12-05 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
    </tr>
    <tr>
      <th>5721611</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE2</td>
      <td>SOFTWARE ESPECIALIZADO II</td>
      <td>...</td>
      <td>1</td>
      <td>2014-06-05 14:59:09.470</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
    </tr>
    <tr>
      <th>5721612</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TEE1</td>
      <td>TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>1</td>
      <td>2014-11-28 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
    </tr>
    <tr>
      <th>5721613</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LTME</td>
      <td>LABORATORIO DE TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>1</td>
      <td>2014-11-27 15:01:33.780</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
    </tr>
    <tr>
      <th>5721614</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INEL</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>...</td>
      <td>1</td>
      <td>2014-11-24 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
    </tr>
    <tr>
      <th>5721615</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD2</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES II</td>
      <td>...</td>
      <td>1</td>
      <td>2014-12-01 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
    </tr>
  </tbody>
</table>
<p>3169561 rows × 24 columns</p>
</div>




```python
def get_ciclo_anio(row):
    clave=row['clave_ciclo']
    texto="20"+clave[:2]
    return texto
lic_pos['anio']=lic_pos.apply (lambda row: get_ciclo_anio (row),axis=1)
lic_pos
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
      <th>unidad_agrupada</th>
      <th>programa_agrupado</th>
      <th>ciclo</th>
      <th>anio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12DHL1</td>
      <td>DESARROLLO DE HABILIDADES LINGÜISTICAS I</td>
      <td>...</td>
      <td>2010-12-10 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-18 00:00:00.000</td>
      <td>NP</td>
      <td>2011-01-22 00:00:00.000</td>
      <td>NP</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>31</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12ECP1</td>
      <td>ECONOMIA POLITICA I</td>
      <td>...</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-15 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>32</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12HEG1</td>
      <td>HISTORIA ECONOMICA GENERAL I</td>
      <td>...</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-16 00:00:00.000</td>
      <td>NP</td>
      <td>2011-01-20 00:00:00.000</td>
      <td>NP</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>33</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12INTE</td>
      <td>INTRODUCCION A LA TEORIA ECONOMICA</td>
      <td>...</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-13 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>49</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCMIOD</td>
      <td>MICROBIOLOGIA ODONTOLOGICA</td>
      <td>...</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>50</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCNUIN</td>
      <td>NUTRICION Y DESARROLLO INFANTIL</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>55</th>
      <td>26703061</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13DIBU</td>
      <td>DIBUJO</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>NP</td>
      <td>2010-11-30 00:00:00.000</td>
      <td>6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>167</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PLTV</td>
      <td>PRACTICAS DE LOCALIZACION Y TRAZO DE VIAS</td>
      <td>...</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>168</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PRF1</td>
      <td>PRACTICAS DE FOTOGRAMETRIA I</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>169</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PTSU</td>
      <td>PRACTICAS DE TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>170</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13TOSU</td>
      <td>TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>171</th>
      <td>20000542</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155110</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>1011SNON</td>
      <td>175IC2</td>
      <td>INGENIERIA EN COMPUTACION</td>
      <td>75PROO</td>
      <td>PROGRAMACION ORIENTADA A OBJETOS</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>NP</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>NP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>197</th>
      <td>28900079</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>199</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCHEMA</td>
      <td>HEMATOLOGIA</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>5</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>200</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>202</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFT1</td>
      <td>OPTATIVA I DE FORMACION TECNICA</td>
      <td>...</td>
      <td>2010-11-26 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>203</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13DSAN</td>
      <td>DISEÑO DE ANTENAS</td>
      <td>...</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>204</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFP1</td>
      <td>OPTATIVA I DE FORMACION PROFESIONAL</td>
      <td>...</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>205</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ETIC</td>
      <td>ETICA</td>
      <td>...</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>206</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13CILL</td>
      <td>CIRCUITOS INTEGRADOS LINEALES Y LABORATORIO</td>
      <td>...</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>207</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ELPL</td>
      <td>ELECTRONICA DE POTENCIA Y LABORATORIO</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>224</th>
      <td>30111558</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09QIA1</td>
      <td>QUIMICA ANALITICA I</td>
      <td>...</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>NP</td>
      <td>2011-12-14 00:00:00.000</td>
      <td>4</td>
      <td>2012-01-18 00:00:00.000</td>
      <td>NP</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>225</th>
      <td>30111727</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09FIQU</td>
      <td>FISIOLOGIA</td>
      <td>...</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>2</td>
      <td>2011-12-16 00:00:00.000</td>
      <td>NP</td>
      <td>2012-01-20 00:00:00.000</td>
      <td>NP</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>234</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ASEV</td>
      <td>ASESORIA EDUCATIVA VOCACIONAL</td>
      <td>...</td>
      <td>2011-12-07 00:00:00.000</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>235</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DEHE</td>
      <td>DESARROLLO HUMANO EN EDUCACION</td>
      <td>...</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>236</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DIVO</td>
      <td>DIAGNOSTICO VOCACIONAL</td>
      <td>...</td>
      <td>2011-11-29 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>237</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INE3</td>
      <td>INVESTIGACION EDUCATIVA III</td>
      <td>...</td>
      <td>2011-12-01 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>238</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INPR</td>
      <td>INFORMACION PROFESIOGRAFICA</td>
      <td>...</td>
      <td>2011-11-30 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>239</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORED</td>
      <td>ORIENTACION EDUCATIVA</td>
      <td>...</td>
      <td>2011-12-02 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>240</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORSE</td>
      <td>ORIENTACION SEXUAL</td>
      <td>...</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5721586</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ESTT</td>
      <td>ESTATICA</td>
      <td>...</td>
      <td>2013-06-03 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721587</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAES</td>
      <td>LABORATORIO DE ESTATICA</td>
      <td>...</td>
      <td>2013-05-29 14:41:09.730</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721588</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMA</td>
      <td>CIENCIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>2013-05-27 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721589</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PEPI</td>
      <td>PROBABILIDAD Y ESTADISTICA PARA INGENIEROS</td>
      <td>...</td>
      <td>2013-06-04 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721590</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ELMA</td>
      <td>ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>2013-12-06 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721591</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAEM</td>
      <td>LABORATORIO DE ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>2013-12-05 14:42:24.020</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721592</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DINM</td>
      <td>DINAMICA</td>
      <td>...</td>
      <td>2013-12-02 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721593</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LADI</td>
      <td>LABORATORIO DE DINAMICA</td>
      <td>...</td>
      <td>2013-12-09 14:43:09.483</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721594</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ANAV</td>
      <td>ANALISIS VECTORIAL</td>
      <td>...</td>
      <td>2013-11-29 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721595</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ECUD</td>
      <td>ECUACIONES DIFERENCIALES</td>
      <td>...</td>
      <td>2013-12-04 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721596</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PRTR</td>
      <td>PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>2014-06-16 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721597</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAPT</td>
      <td>LABORATORIO DE PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>2014-05-28 14:44:38.120</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721598</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13METN</td>
      <td>METODOS NUMERICOS</td>
      <td>...</td>
      <td>2013-12-03 14:46:06.560</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721599</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13SILI</td>
      <td>SISTEMAS LINEALES</td>
      <td>...</td>
      <td>2014-06-18 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721600</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MECR</td>
      <td>MECANICA DEL CUERPO RIGIDO</td>
      <td>...</td>
      <td>2014-06-20 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721601</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DIBM</td>
      <td>DIBUJO MECANICO</td>
      <td>...</td>
      <td>2012-12-05 14:48:46.460</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2012</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721602</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13COGI</td>
      <td>COMUNICACION GRAFICA EN INGENIERIA</td>
      <td>...</td>
      <td>2013-05-30 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721603</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MEIA</td>
      <td>METROLOGIA</td>
      <td>...</td>
      <td>2013-11-25 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721604</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAME</td>
      <td>LABORATORIO DE METROLOGIA</td>
      <td>...</td>
      <td>2013-11-26 14:50:03.320</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721605</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE1</td>
      <td>SOFTWARE ESPECIALIZADO I</td>
      <td>...</td>
      <td>2013-11-27 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721606</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INGM</td>
      <td>INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>2014-06-06 14:51:51.113</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721607</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAIM</td>
      <td>LABORATORIO DE INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>2014-06-13 00:00:00.000</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721608</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD1</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2014-06-12 14:53:02.960</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721609</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LSD1</td>
      <td>LABORATORIO DE MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>2014-06-12 11:48:42.913</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721610</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMC</td>
      <td>CINEMATICA DE MECANISMOS</td>
      <td>...</td>
      <td>2014-12-05 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5721611</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE2</td>
      <td>SOFTWARE ESPECIALIZADO II</td>
      <td>...</td>
      <td>2014-06-05 14:59:09.470</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721612</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TEE1</td>
      <td>TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>2014-11-28 00:00:00.000</td>
      <td>8</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5721613</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LTME</td>
      <td>LABORATORIO DE TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>2014-11-27 15:01:33.780</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5721614</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INEL</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>...</td>
      <td>2014-11-24 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5721615</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD2</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES II</td>
      <td>...</td>
      <td>2014-12-01 00:00:00.000</td>
      <td>9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
  </tbody>
</table>
<p>3169561 rows × 25 columns</p>
</div>



En los campos **calif_ord, calif_ext y calif_titulo** se almacenan las calificaciones del alumno, mas estos campos no son numericos y contienen caracteres que no son necesario. A continuación se muestra un ejemplo con el **campo calif_ord**.


```python
lic_pos['calif_ord']=lic_pos['calif_ord'].str.strip().fillna('0')
lic_pos['calif_ext']=lic_pos['calif_ext'].str.strip().fillna('0')
lic_pos['calif_titulo']=lic_pos['calif_titulo'].str.strip().fillna('0')
ordi = lic_pos['calif_ord'].unique()
ordi
```




    array(['5', 'NP', '8', '7', '9', '10', '2', '4', '6', '3', '0', '1', '10.',
           'NA', '-1', '3.8', '4.6', '3.5', '3.4', '4.2', '1.3', '.5', '1.2',
           '.4', '4.1', 'np', '7.5', '', '5.', '5,', 'N', '8.', '9,', '9.',
           '+7', '+5', '7,', '7.', '6,', '09', '.8', '7.00', '1.7', 'na',
           '8.00', '00', '0.8', '0.7', '0.2', '0.95', '4.', '8.5', '+9', '9.0',
           '9.3', '9.1', '8.6', '8.9', '9.5', '9.8', '8.7', '8.8', '9.6',
           '9.4', '8.0', '9.7'], dtype=object)



Es necesario entonces limpiar estos 3 campos. 


```python
import re
def limpia_calif(row,name):
    val=row[name]
    val=val.replace('+','')
    val=val.replace(',','')
    result=0
    if (unicode(val,'utf-8').isnumeric()):
        result=float(val)
    elif val.startswith('.'):
        result=float('0'+val)
    elif val.endswith('.'):
        result=float(val+'0')
    elif re.match("^\d+?\.\d+?$", val) is None:
        result=0
    else:
        result=float(val)
    if result>10:
        result=10
    return result
lic_pos['calif_ord']=lic_pos.apply (lambda row: limpia_calif (row,'calif_ord'),axis=1)
lic_pos['calif_ext']=lic_pos.apply (lambda row: limpia_calif (row,'calif_ext'),axis=1)
lic_pos['calif_titulo']=lic_pos.apply (lambda row: limpia_calif (row,'calif_titulo'),axis=1)
lic_pos
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>fecha_ord</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
      <th>unidad_agrupada</th>
      <th>programa_agrupado</th>
      <th>ciclo</th>
      <th>anio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12DHL1</td>
      <td>DESARROLLO DE HABILIDADES LINGÜISTICAS I</td>
      <td>...</td>
      <td>2010-12-10 00:00:00.000</td>
      <td>5.0</td>
      <td>2010-12-18 00:00:00.000</td>
      <td>0.0</td>
      <td>2011-01-22 00:00:00.000</td>
      <td>0.0</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>31</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12ECP1</td>
      <td>ECONOMIA POLITICA I</td>
      <td>...</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>0.0</td>
      <td>2010-12-15 00:00:00.000</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>32</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12HEG1</td>
      <td>HISTORIA ECONOMICA GENERAL I</td>
      <td>...</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>5.0</td>
      <td>2010-12-16 00:00:00.000</td>
      <td>0.0</td>
      <td>2011-01-20 00:00:00.000</td>
      <td>0.0</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>33</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12INTE</td>
      <td>INTRODUCCION A LA TEORIA ECONOMICA</td>
      <td>...</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>0.0</td>
      <td>2010-12-13 00:00:00.000</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>49</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCMIOD</td>
      <td>MICROBIOLOGIA ODONTOLOGICA</td>
      <td>...</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>50</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCNUIN</td>
      <td>NUTRICION Y DESARROLLO INFANTIL</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>55</th>
      <td>26703061</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13DIBU</td>
      <td>DIBUJO</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>0.0</td>
      <td>2010-11-30 00:00:00.000</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>167</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PLTV</td>
      <td>PRACTICAS DE LOCALIZACION Y TRAZO DE VIAS</td>
      <td>...</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>168</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PRF1</td>
      <td>PRACTICAS DE FOTOGRAMETRIA I</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>169</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PTSU</td>
      <td>PRACTICAS DE TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>170</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13TOSU</td>
      <td>TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>171</th>
      <td>20000542</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155110</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>1011SNON</td>
      <td>175IC2</td>
      <td>INGENIERIA EN COMPUTACION</td>
      <td>75PROO</td>
      <td>PROGRAMACION ORIENTADA A OBJETOS</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>0.0</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>197</th>
      <td>28900079</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>199</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCHEMA</td>
      <td>HEMATOLOGIA</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>5.0</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>200</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>2010-11-20 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>202</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFT1</td>
      <td>OPTATIVA I DE FORMACION TECNICA</td>
      <td>...</td>
      <td>2010-11-26 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>203</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13DSAN</td>
      <td>DISEÑO DE ANTENAS</td>
      <td>...</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>204</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFP1</td>
      <td>OPTATIVA I DE FORMACION PROFESIONAL</td>
      <td>...</td>
      <td>2010-11-29 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>205</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ETIC</td>
      <td>ETICA</td>
      <td>...</td>
      <td>2010-12-01 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>206</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13CILL</td>
      <td>CIRCUITOS INTEGRADOS LINEALES Y LABORATORIO</td>
      <td>...</td>
      <td>2010-11-24 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>207</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ELPL</td>
      <td>ELECTRONICA DE POTENCIA Y LABORATORIO</td>
      <td>...</td>
      <td>2010-11-22 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
    </tr>
    <tr>
      <th>224</th>
      <td>30111558</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09QIA1</td>
      <td>QUIMICA ANALITICA I</td>
      <td>...</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>0.0</td>
      <td>2011-12-14 00:00:00.000</td>
      <td>4.0</td>
      <td>2012-01-18 00:00:00.000</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>225</th>
      <td>30111727</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09FIQU</td>
      <td>FISIOLOGIA</td>
      <td>...</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>2.0</td>
      <td>2011-12-16 00:00:00.000</td>
      <td>0.0</td>
      <td>2012-01-20 00:00:00.000</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>234</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ASEV</td>
      <td>ASESORIA EDUCATIVA VOCACIONAL</td>
      <td>...</td>
      <td>2011-12-07 00:00:00.000</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>235</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DEHE</td>
      <td>DESARROLLO HUMANO EN EDUCACION</td>
      <td>...</td>
      <td>2011-11-28 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>236</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DIVO</td>
      <td>DIAGNOSTICO VOCACIONAL</td>
      <td>...</td>
      <td>2011-11-29 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>237</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INE3</td>
      <td>INVESTIGACION EDUCATIVA III</td>
      <td>...</td>
      <td>2011-12-01 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>238</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INPR</td>
      <td>INFORMACION PROFESIOGRAFICA</td>
      <td>...</td>
      <td>2011-11-30 00:00:00.000</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>239</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORED</td>
      <td>ORIENTACION EDUCATIVA</td>
      <td>...</td>
      <td>2011-12-02 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>240</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORSE</td>
      <td>ORIENTACION SEXUAL</td>
      <td>...</td>
      <td>2011-12-05 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5721586</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ESTT</td>
      <td>ESTATICA</td>
      <td>...</td>
      <td>2013-06-03 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721587</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAES</td>
      <td>LABORATORIO DE ESTATICA</td>
      <td>...</td>
      <td>2013-05-29 14:41:09.730</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721588</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMA</td>
      <td>CIENCIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>2013-05-27 00:00:00.000</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721589</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PEPI</td>
      <td>PROBABILIDAD Y ESTADISTICA PARA INGENIEROS</td>
      <td>...</td>
      <td>2013-06-04 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721590</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ELMA</td>
      <td>ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>2013-12-06 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721591</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAEM</td>
      <td>LABORATORIO DE ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>2013-12-05 14:42:24.020</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721592</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DINM</td>
      <td>DINAMICA</td>
      <td>...</td>
      <td>2013-12-02 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721593</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LADI</td>
      <td>LABORATORIO DE DINAMICA</td>
      <td>...</td>
      <td>2013-12-09 14:43:09.483</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721594</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ANAV</td>
      <td>ANALISIS VECTORIAL</td>
      <td>...</td>
      <td>2013-11-29 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721595</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ECUD</td>
      <td>ECUACIONES DIFERENCIALES</td>
      <td>...</td>
      <td>2013-12-04 00:00:00.000</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721596</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PRTR</td>
      <td>PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>2014-06-16 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721597</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAPT</td>
      <td>LABORATORIO DE PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>2014-05-28 14:44:38.120</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721598</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13METN</td>
      <td>METODOS NUMERICOS</td>
      <td>...</td>
      <td>2013-12-03 14:46:06.560</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721599</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13SILI</td>
      <td>SISTEMAS LINEALES</td>
      <td>...</td>
      <td>2014-06-18 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721600</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MECR</td>
      <td>MECANICA DEL CUERPO RIGIDO</td>
      <td>...</td>
      <td>2014-06-20 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721601</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DIBM</td>
      <td>DIBUJO MECANICO</td>
      <td>...</td>
      <td>2012-12-05 14:48:46.460</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2012</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721602</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13COGI</td>
      <td>COMUNICACION GRAFICA EN INGENIERIA</td>
      <td>...</td>
      <td>2013-05-30 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
    </tr>
    <tr>
      <th>5721603</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MEIA</td>
      <td>METROLOGIA</td>
      <td>...</td>
      <td>2013-11-25 00:00:00.000</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721604</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAME</td>
      <td>LABORATORIO DE METROLOGIA</td>
      <td>...</td>
      <td>2013-11-26 14:50:03.320</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721605</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE1</td>
      <td>SOFTWARE ESPECIALIZADO I</td>
      <td>...</td>
      <td>2013-11-27 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721606</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INGM</td>
      <td>INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>2014-06-06 14:51:51.113</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721607</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAIM</td>
      <td>LABORATORIO DE INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>2014-06-13 00:00:00.000</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721608</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD1</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>2014-06-12 14:53:02.960</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721609</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LSD1</td>
      <td>LABORATORIO DE MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>2014-06-12 11:48:42.913</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721610</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMC</td>
      <td>CINEMATICA DE MECANISMOS</td>
      <td>...</td>
      <td>2014-12-05 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5721611</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE2</td>
      <td>SOFTWARE ESPECIALIZADO II</td>
      <td>...</td>
      <td>2014-06-05 14:59:09.470</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
    </tr>
    <tr>
      <th>5721612</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TEE1</td>
      <td>TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>2014-11-28 00:00:00.000</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5721613</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LTME</td>
      <td>LABORATORIO DE TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>2014-11-27 15:01:33.780</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5721614</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INEL</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>...</td>
      <td>2014-11-24 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
    <tr>
      <th>5721615</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD2</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES II</td>
      <td>...</td>
      <td>2014-12-01 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
    </tr>
  </tbody>
</table>
<p>3169561 rows × 25 columns</p>
</div>



Una vez limpios los campos es necesario agregar una columna más con la calificacion final aprobatoria del alumno para agilizar su procesamiento.


```python
def join_calif(row):
    aprobado=row['aprobado']
    if aprobado==1:
        ordinario=row['calif_ord']
        extra=row['calif_ext']
        titulo=row['calif_titulo']
        if titulo>=6:
            return titulo
        elif extra>=6:
            return extra
        else:
            return ordinario
    else:
        return 0
lic_pos['calif']=lic_pos.apply (lambda row: join_calif (row),axis=1)
```


```python
lic_pos
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>matricula</th>
      <th>clave_unidad</th>
      <th>unidad</th>
      <th>clave_programa</th>
      <th>programa</th>
      <th>clave_ciclo</th>
      <th>clave_plan</th>
      <th>plan</th>
      <th>clave_materia</th>
      <th>materia</th>
      <th>...</th>
      <th>calif_ord</th>
      <th>fecha_ext</th>
      <th>calif_ext</th>
      <th>fecha_titulo</th>
      <th>calif_titulo</th>
      <th>unidad_agrupada</th>
      <th>programa_agrupado</th>
      <th>ciclo</th>
      <th>anio</th>
      <th>calif</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12DHL1</td>
      <td>DESARROLLO DE HABILIDADES LINGÜISTICAS I</td>
      <td>...</td>
      <td>5.0</td>
      <td>2010-12-18 00:00:00.000</td>
      <td>0.0</td>
      <td>2011-01-22 00:00:00.000</td>
      <td>0.0</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>31</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12ECP1</td>
      <td>ECONOMIA POLITICA I</td>
      <td>...</td>
      <td>0.0</td>
      <td>2010-12-15 00:00:00.000</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>32</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12HEG1</td>
      <td>HISTORIA ECONOMICA GENERAL I</td>
      <td>...</td>
      <td>5.0</td>
      <td>2010-12-16 00:00:00.000</td>
      <td>0.0</td>
      <td>2011-01-20 00:00:00.000</td>
      <td>0.0</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>24504854</td>
      <td>21300</td>
      <td>ECONOMIA</td>
      <td>156030</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>1011SNON</td>
      <td>112TCE</td>
      <td>TRONCO COMUN DE LA LICENCIATURA EN ECONOMIA.</td>
      <td>12INTE</td>
      <td>INTRODUCCION A LA TEORIA ECONOMICA</td>
      <td>...</td>
      <td>0.0</td>
      <td>2010-12-13 00:00:00.000</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>ECONOMIA</td>
      <td>LICENCIADO EN ECONOMIA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>49</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCMIOD</td>
      <td>MICROBIOLOGIA ODONTOLOGICA</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>50</th>
      <td>28900370</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154040</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>1011SNON</td>
      <td>116MC3</td>
      <td>LICENCIATURA DE MEDICO CIRUJANO DENTISTA.</td>
      <td>TCNUIN</td>
      <td>NUTRICION Y DESARROLLO INFANTIL</td>
      <td>...</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO CIRUJANO DENTISTA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>55</th>
      <td>26703061</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13DIBU</td>
      <td>DIBUJO</td>
      <td>...</td>
      <td>0.0</td>
      <td>2010-11-30 00:00:00.000</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>167</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PLTV</td>
      <td>PRACTICAS DE LOCALIZACION Y TRAZO DE VIAS</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>168</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PRF1</td>
      <td>PRACTICAS DE FOTOGRAMETRIA I</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>169</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13PTSU</td>
      <td>PRACTICAS DE TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>170</th>
      <td>23400732</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155070</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>1011SNON</td>
      <td>113IT2</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO.</td>
      <td>13TOSU</td>
      <td>TOPOGRAFIA SUBTERRANEA</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO TOPOGRAFO E HIDROGRAFO</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>171</th>
      <td>20000542</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155110</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>1011SNON</td>
      <td>175IC2</td>
      <td>INGENIERIA EN COMPUTACION</td>
      <td>75PROO</td>
      <td>PROGRAMACION ORIENTADA A OBJETOS</td>
      <td>...</td>
      <td>0.0</td>
      <td>2010-12-03 00:00:00.000</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>INGENIERO EN COMPUTACION</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>197</th>
      <td>28900079</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>199</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCHEMA</td>
      <td>HEMATOLOGIA</td>
      <td>...</td>
      <td>5.0</td>
      <td>2010-12-06 00:00:00.000</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>200</th>
      <td>25604124</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154030</td>
      <td>MEDICO GENERAL</td>
      <td>1011SNON</td>
      <td>114MC5</td>
      <td>LICENCIATURA DE MEDICO GENERAL.</td>
      <td>TCIMAG</td>
      <td>IMAGENOLOGIA I</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>MEDICO GENERAL</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>202</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFT1</td>
      <td>OPTATIVA I DE FORMACION TECNICA</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>203</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13DSAN</td>
      <td>DISEÑO DE ANTENAS</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>204</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113OC2</td>
      <td>LICENCIATURA EN INGENIERIA EN COMUNICACIONES Y...</td>
      <td>13OFP1</td>
      <td>OPTATIVA I DE FORMACION PROFESIONAL</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>205</th>
      <td>27800419</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ETIC</td>
      <td>ETICA</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>206</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13CILL</td>
      <td>CIRCUITOS INTEGRADOS LINEALES Y LABORATORIO</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>207</th>
      <td>27800455</td>
      <td>23201</td>
      <td>INGENIERIA ELECTRICA PLANTEL ZACATECAS</td>
      <td>155060</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>1011SNON</td>
      <td>113TRO</td>
      <td>INGENIERIA EN COMUNICACIONES Y ELECTRONICA.</td>
      <td>13ELPL</td>
      <td>ELECTRONICA DE POTENCIA Y LABORATORIO</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>ING. EN COMUNICACIONES Y ELECTRONICA</td>
      <td>AGOSTO-DICIEMBRE 2010</td>
      <td>2010</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>224</th>
      <td>30111558</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09QIA1</td>
      <td>QUIMICA ANALITICA I</td>
      <td>...</td>
      <td>0.0</td>
      <td>2011-12-14 00:00:00.000</td>
      <td>4.0</td>
      <td>2012-01-18 00:00:00.000</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>225</th>
      <td>30111727</td>
      <td>29401</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>154020</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>1112SNON</td>
      <td>109F10</td>
      <td>LICENCIATURA DE QUIMICO FARMACEUTICO BIOLOGO.</td>
      <td>09FIQU</td>
      <td>FISIOLOGIA</td>
      <td>...</td>
      <td>2.0</td>
      <td>2011-12-16 00:00:00.000</td>
      <td>0.0</td>
      <td>2012-01-20 00:00:00.000</td>
      <td>0.0</td>
      <td>CIENCIAS DE LA SALUD</td>
      <td>QUIMICO FARMACEUTICO-BIOLOGO</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>234</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ASEV</td>
      <td>ASESORIA EDUCATIVA VOCACIONAL</td>
      <td>...</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>235</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DEHE</td>
      <td>DESARROLLO HUMANO EN EDUCACION</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>236</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29DIVO</td>
      <td>DIAGNOSTICO VOCACIONAL</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>237</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INE3</td>
      <td>INVESTIGACION EDUCATIVA III</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>238</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29INPR</td>
      <td>INFORMACION PROFESIOGRAFICA</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>239</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORED</td>
      <td>ORIENTACION EDUCATIVA</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>240</th>
      <td>28902881</td>
      <td>22801</td>
      <td>PSICOLOGIA PLANTEL ZACATECAS</td>
      <td>156040</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>1112SNON</td>
      <td>129P1E</td>
      <td>LICENCIATURA EN PSICOLOGIA EN EL AREA EDUCATIVA.</td>
      <td>29ORSE</td>
      <td>ORIENTACION SEXUAL</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>PSICOLOGIA</td>
      <td>LICENCIADO EN PSICOLOGIA</td>
      <td>AGOSTO-DICIEMBRE 2011</td>
      <td>2011</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5721586</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ESTT</td>
      <td>ESTATICA</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721587</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAES</td>
      <td>LABORATORIO DE ESTATICA</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721588</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMA</td>
      <td>CIENCIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>5721589</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PEPI</td>
      <td>PROBABILIDAD Y ESTADISTICA PARA INGENIEROS</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>5721590</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ELMA</td>
      <td>ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>5721591</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAEM</td>
      <td>LABORATORIO DE ELECTRICIDAD Y MAGNETISMO</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>5721592</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DINM</td>
      <td>DINAMICA</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721593</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LADI</td>
      <td>LABORATORIO DE DINAMICA</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721594</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ANAV</td>
      <td>ANALISIS VECTORIAL</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721595</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13ECUD</td>
      <td>ECUACIONES DIFERENCIALES</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>5721596</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13PRTR</td>
      <td>PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>5721597</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAPT</td>
      <td>LABORATORIO DE PRINCIPIOS DE TERMOFLUIDOS</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>5721598</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13METN</td>
      <td>METODOS NUMERICOS</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721599</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13SILI</td>
      <td>SISTEMAS LINEALES</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721600</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MECR</td>
      <td>MECANICA DEL CUERPO RIGIDO</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>5721601</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13DIBM</td>
      <td>DIBUJO MECANICO</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2012</td>
      <td>2012</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>5721602</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1213SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13COGI</td>
      <td>COMUNICACION GRAFICA EN INGENIERIA</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2013</td>
      <td>2012</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721603</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MEIA</td>
      <td>METROLOGIA</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>5721604</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAME</td>
      <td>LABORATORIO DE METROLOGIA</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>5721605</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE1</td>
      <td>SOFTWARE ESPECIALIZADO I</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2013</td>
      <td>2013</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>5721606</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INGM</td>
      <td>INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>5721607</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LAIM</td>
      <td>LABORATORIO DE INGENIERIA DE LOS MATERIALES</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721608</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD1</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>0.0</td>
      <td>2014-06-12 14:53:02.960</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>5721609</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LSD1</td>
      <td>LABORATORIO DE MECANICA DE SOLIDOS DEFORMABLES I</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>5721610</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13CIMC</td>
      <td>CINEMATICA DE MECANISMOS</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>5721611</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1314SPAR</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TSE2</td>
      <td>SOFTWARE ESPECIALIZADO II</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>ENERO-JULIO 2014</td>
      <td>2013</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721612</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13TEE1</td>
      <td>TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>8.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>5721613</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13LTME</td>
      <td>LABORATORIO DE TECNOLOGIA MECANICA I</td>
      <td>...</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>5721614</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13INEL</td>
      <td>INGENIERIA ELECTRICA</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>5721615</th>
      <td>29100540</td>
      <td>22400</td>
      <td>INGENIERIA I</td>
      <td>155040</td>
      <td>INGENIERO MECANICO</td>
      <td>1415SNON</td>
      <td>113IN5</td>
      <td>LICENCIATURA EN INGENIERIA MECANICA</td>
      <td>13MSD2</td>
      <td>MECANICA DE SOLIDOS DEFORMABLES II</td>
      <td>...</td>
      <td>9.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>INGENIERIA I</td>
      <td>INGENIERO MECANICO</td>
      <td>AGOSTO-DICIEMBRE 2014</td>
      <td>2014</td>
      <td>9.0</td>
    </tr>
  </tbody>
</table>
<p>3169561 rows × 26 columns</p>
</div>



## Almacenamiento

Una vez limpios los datos, se procede a almacenarlos en una base de datos documental en MongoDB.


```python
from pymongo import MongoClient
import json

con = MongoClient()
db = con.control_escolar
ce = db.datos


```


```python
for idx, row in lic_pos.iterrows():
    ce.insert_one(json.loads(row.to_json()))
```

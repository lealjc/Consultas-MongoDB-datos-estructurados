# Consultas-MongoDB-datos-estructurados
Básico/Introducción a Base de Datos/Consultas con MongoDB a datos estructurados/Postwork

## ¡NOTA IMPORTANTE!
En el Postwork 5, hice la base de datos jcleal_postwork que incluia 2 colecciones y fueron suficientes para ese objetivo.

Para este Postwork 6, agregué una colección más para poder cumplir con los objetivos de esta sesión.

<hr>

### 1.	Como ahora ya puedes hacer uso de expresiones regulares, ordenamientos, agrupaciones, limitar el número de documentos o usar funciones como suma, mínimo, máximo, promedio ya puedes responder a preguntas como:

## BASE DE DATOS UTILIZADA: jclealPostwork
## COLECCIONES: estadis_ene_2020

### ¿Cuál es la lista de todos los documentos que incluyan el "texto" en algún campo?

*** En iFILTER... ***

##### a)	Lista de los documentos de un usuario

{USERIP: "usuario.01"} -> 42 documentos

{USERIP: "usuario.02"} -> 75 documentos


#### b)	Lista de los documentos de un municipio

{MUNICIPIO: "GUADALUPE"} -> 3 documentos

{MUNICIPIO: "TOLUCA"} -> 2 documentos


#### c)	Lista de los documentos de una delegación

{UR: "001"} -> 14 documentos

{DELEGACION: "AGUASCALIENTES"} -> 14 documentos


### *** Utilizando Aggregations ***

#### a)	Lista de los documentos de un usuario

En la PRIMERA capa:

$project

{
  "DELEGACION": 1, "MUNICIPIO": 1, "USERIP": 1
}

En la SEGUNDA capa:

{
  USERIP: "usuario.01"
}


#### b)	Lista de los documentos de un municipio

En la PRIMERA capa:

$project

{
  "DELEGACION": 1, "MUNICIPIO": 1, "USERIP": 1
}

En la SEGUNDA capa:

{
  MUNICIPIO: "GUADALUPE"
}


#### c)	Lista de los documentos de una delegación

En la PRIMERA capa:

$project

{
  "DELEGACION": 1, "MUNICIPIO": 1, "USERIP": 1
}

En la SEGUNDA capa:

{
  DELEGACION: "AGUASCALIENTES"
}


### ¿Cuáles son los documentos que tienen mayor o menor cantidad en algún campo?

*** Utilizando Aggregations ***

#### 10 documentos con mayor número de altas

En la PRIMERA capa:

$addFields

{
  ALTAS: {$sum: ["$AMLA", "$AMLB", "$AMLC", "$AMMA", "$AMMB", "$AMP1", "$AMP2", "$AMP3", "$AFLA", "$AFLB", "$AFLC", "$AFMA", "$AFMB", "$AFP1", "$AFP2", "$AFP3"] },
   
BAJAS: {$sum: ["$BMLA", "$BMLB", "$BMLC", "$BMMA", "$BMMB", "$BMP1", "$BMP2", "$BMP3", "$BFLA", "$BFLB", "$BFLC", "$BFMA", "$BFMB", "$BFP1", "$BFP2", "$BFP3"] }
}

En la SEGUNDA capa:

$project

{
  "ALTAS": 1, "BAJAS": 1, "DELEGACION": 1, "MUNICIPIO": 1, EBDI: 1
}

En la TERCERA capa:

$sort

{
  ALTAS: -1
}

En la CUARTA capa:

$limit

10


#### 10 documentos con mayor número de bajas

En la PRIMERA capa:

$addFields

{
  ALTAS: {$sum: ["$AMLA", "$AMLB", "AMLC", "AMMA", "AMMB", "AMP1", "AMP2", "AMP3", "AFLA", "AFLB", "AFLC", "AFMA", "AFMB", "AFP1", "AFP2", "AFP3"] },
  BAJAS: {$sum:  
["$BMLA", "$BMLB", "BMLC", "BMMA", "BMMB", "BMP1", "BMP2", "BMP3", "BFLA", "BFLB", "BFLC", "BFMA", "BFMB", "BFP1", "BFP2", "BFP3"] }
}

En la SEGUNDA capa:

$project

{
  "ALTAS": 1, "BAJAS": 1, "DELEGACION": 1, "MUNICIPIO": 1, EBDI: 1
}

En la TERCERA capa:

$sort

{
  BAJAS: -1
}

En la CUARTA capa:

$limit

10


#### 10 documentos con MENOR POBLACIÓN

En la PRIMERA capa:

$addFields

{
  POBLACION_ANT: {$sum: ["$IAMLA", "$IAMLB", "$IAMLC", "$IAMMA", "$IAMMB", "$IAMP1", "$IAMP2", "$IAMP3", "$IAFLA", "$IAFLB", "$IAFLC", "$IAFMA", "$IAFMB", "$IAFP1",  
"$IAFP2", "$IAFP3"] },
  ALTAS: {$sum: ["$AMLA", "$AMLB", "$AMLC", "$AMMA", "$AMMB", "$AMP1", "$AMP2", "$AMP3", "$AFLA", "$AFLB", "$AFLC", "$AFMA", "$AFMB", "$AFP1",  
"$AFP2", "$AFP3"] },
  BAJAS: {$sum: ["$BMLA", "$BMLB", "$BMLC", "$BMMA", "$BMMB", "$BMP1", "$BMP2", "$BMP3", "$BFLA", "$BFLB", "$BFLC", "$BFMA", "$BFMB", "$BFP1",  
"$BFP2", "$BFP3"] },
  ASISTENCIA: {$sum: ["$AALA", "$AALB", "$AALC", "$AAMA", "$AAMB", "$AAP1", "$AAP2", "$AAP3"] },
  MOVIMIENTOS: {$sum: ["$MIMLA", "$MIMLB",  
"$MIMLC", "$MIMMA", "$MIMMB", "$MIMP1", "$MIMP2", "$MIMP3", "$MIFLA", "$MIFLB", "$MIFLC", "$MIFMA", "$MIFMB", "$MIFP1", "$MIFP2", "$MIFP3"] }
}

En la SEGUNDA capa:

$addFields

{
  POBLACION: {$sum: ["$POBLACION_ANT", "$ALTAS", "$BAJAS", "$MOVIMIENTOS"] }
}

En la TERCERA capa:

$project

{
  REGIMEN_B: 1, EBDI: 1, DELEGACION: 1, ASISTENCIA: 1, POBLACION: 1
}

En la CUARTA capa:

$sort

{
  POBLACION: 1
}

En la QUINTA capa

$limit

10


#### 10 documentos con menor asistencia

En la PRIMERA capa:

$addFields

{
  POBLACION_ANT: {$sum: ["$IAMLA", "$IAMLB", "$IAMLC", "$IAMMA", "$IAMMB", "$IAMP1", "$IAMP2", "$IAMP3", "$IAFLA", "$IAFLB", "$IAFLC", "$IAFMA", "$IAFMB", "$IAFP1",  
"$IAFP2", "$IAFP3"] },
  ALTAS: {$sum: ["$AMLA", "$AMLB", "$AMLC", "$AMMA", "$AMMB", "$AMP1", "$AMP2", "$AMP3", "$AFLA", "$AFLB", "$AFLC", "$AFMA", "$AFMB", "$AFP1",  
"$AFP2", "$AFP3"] },
  BAJAS: {$sum: ["$BMLA", "$BMLB", "$BMLC", "$BMMA", "$BMMB", "$BMP1", "$BMP2", "$BMP3", "$BFLA", "$BFLB", "$BFLC", "$BFMA", "$BFMB", "$BFP1",  
"$BFP2", "$BFP3"] },
  ASISTENCIA: {$sum: ["$AALA", "$AALB", "$AALC", "$AAMA", "$AAMB", "$AAP1", "$AAP2", "$AAP3"] },
  MOVIMIENTOS: {$sum: ["$MIMLA", "$MIMLB",  
"$MIMLC", "$MIMMA", "$MIMMB", "$MIMP1", "$MIMP2", "$MIMP3", "$MIFLA", "$MIFLB", "$MIFLC", "$MIFMA", "$MIFMB", "$MIFP1", "$MIFP2", "$MIFP3"] }
}

En la SEGUNDA capa:

$addFields

{
  POBLACION: {$sum: ["$POBLACION_ANT", "$ALTAS", "$BAJAS", "$MOVIMIENTOS"] }
}

En la TERCERA capa:

$project

{
  REGIMEN_B: 1, EBDI: 1, DELEGACION: 1, ASISTENCIA: 1, POBLACION: 1
}

En la CUARTA capa:

$sort

{
  ASISTENCIA: 1
}

En la QUINTA capa

$limit

10


#### ¿Cuál es el promedio de todos los valores de algún campo como edad, precio, cantidad?

Promedio de asistencia de todos los documentos y promedio de días laborados.


En la PRIMERA capa:

$addFields

{
  ASISTENCIA: {$sum: ["$AALA", "$AALB", "$AALC", "$AAMA", "$AAMB", "$AAP1", "$AAP2", "$AAP3"] }
}

En la SEGUNDA capa:

$group

{
  _id: null,
  ASISTENCIA_TOTAL: {$sum: "$ASISTENCIA"},
  DIAS_LABORADOS: {$sum: "$DL"},
  REGISTROS: {$sum: "$MES"}
}

En la TERCERA capa:

$addFields

{
  PROMEDIO_ASISTENCIA: {$divide: ["$ASISTENCIA_TOTAL", "$REGISTROS"]},
  PROMEDIO_DIAS_LABORADOS: {$divide: ["$DIAS_LABORADOS", "$REGISTROS"]}
}


#### ¿Cuál es la cantidad de documentos por tipo de producto o por cierta fecha?

Cantidad por cada "REGIMEN_B"

En la PRIMERA capa

$group

{
  _id: "$REGIMEN_B",
  MODALIDAD: {
    $sum: 1
  }
}

Cantidad de "EBDI" por cada delegación

En el entendido que cada documento es una EBDI

En la PRIMERA capa

$group

{
  _id: "$DELEGACION",
  TOTAL_EBDIS: {
    $sum: 1
  },
}

En la SEGUNDA capa

$sort

{
  _id: 1
}


#### ¿Cuál es el producto con más o menos ventas?

Documento con menor y mayor población

En la PRIMERA capa

$addFields

{
  POBLACION_ANT: {$sum: ["$IAMLA", "$IAMLB", "$IAMLC", "$IAMMA", "$IAMMB", "$IAMP1", "$IAMP2", "$IAMP3", "$IAFLA", "$IAFLB", "$IAFLC", "$IAFMA", "$IAFMB", "$IAFP1", "$IAFP2", "$IAFP3"] },
  ALTAS: {$sum: ["$AMLA", "$AMLB", "$AMLC", "$AMMA", "$AMMB", "$AMP1", "$AMP2", "$AMP3", "$AFLA", "$AFLB", "$AFLC", "$AFMA", "$AFMB", "$AFP1", "$AFP2", "$AFP3"] },
  BAJAS: {$sum: ["$BMLA", "$BMLB", "$BMLC", "$BMMA", "$BMMB", "$BMP1", "$BMP2", "$BMP3", "$BFLA", "$BFLB", "$BFLC", "$BFMA", "$BFMB", "$BFP1", "$BFP2", "$BFP3"] },
  MOVIMIENTOS: {$sum: ["$MIMLA", "$MIMLB", "$MIMLC", "$MIMMA", "$MIMMB", "$MIMP1", "$MIMP2", "$MIMP3", "$MIFLA", "$MIFLB", "$MIFLC", "$MIFMA", "$MIFMB", "$MIFP1", "$MIFP2", "$MIFP3"] }
}

En la SEGUNDA capa:

$addFields

{
  POBLACION: {$sum: ["$POBLACION_ANT", "$ALTAS", "$BAJAS", "$MOVIMIENTOS"] }
}

En la TERCERA capa:

$project

{
  EBDI: 1, DELEGACION: 1, POBLACION: 1
}

En la CUARTA capa:

$group

{
  _id: "null",
  POBLACION_MENOR: {
    $min: "$POBLACION"
  },
    POBLACION_MAYOR: {
      $max: "$POBLACION"
    }
}




#### ¿Cuál es el promedio de ventas en el periodo de cierto año?

Promedio de altas y bajas en el mes

En la PRIMERA capa:

$addField

{
  ALTAS: {$sum: ["$AMLA", "$AMLB", "$AMLC", "$AMMA", "$AMMB", "$AMP1", "$AMP2", "$AMP3", "$AFLA", "$AFLB", "$AFLC", "$AFMA", "$AFMB", "$AFP1", "$AFP2", "$AFP3"] },
  BAJAS: {$sum: ["$BMLA", "$BMLB", "$BMLC", "$BMMA", "$BMMB", "$BMP1", "$BMP2", "$BMP3", "$BFLA", "$BFLB", "$BFLC", "$BFMA", "$BFMB", "$BFP1", "$BFP2", "$BFP3"] }
}

En la SEGUNDA capa:

$group

{
  _id: "null",
  PROMEDIO_ALTAS: {
    $avg: "$ALTAS"
  },
    PROMEDIO_BAJAS: {
      $avg: "$BAJAS"
    }
}


### Crea algunas preguntas para tu conjunto de datos y construye las consultas en MongoDB que den la respuesta, además toma en cuenta sólo usar una colección a la vez

#### ¿Cuantas personas con discapacidad hay y cuantos registraron accidente?

En la PRIMERA capa:

$addField

{
  DISCAPACIDAD: {$sum: ["$DMLA", "$DMLB", "$DMLC", "$DMMA", "$DMMB", "$DMP1", "$DMP2", "$DMP3", "$DFLA", "$DFLB", "$DFLC", "$DFMA", "$DFMB", "$DFP1", "$DFP2", "$DFP3"]},
  ACCIDENTES: {$sum: ["$AAHMLA", "$AAHFLA", "$AAHMLB", "$AAHFLB", "$AAHMLC", "$AAHFLC", "$AAHMMA", "$AAHFMA", "$AAHMMB", "$AAHFMB", "$AAHMP1", "$AAHFP1", "$AAHMP2", "$AAHFP2", "$AAHMP3", "$AAHFP3", "$AASMLA", "$AASFLA", "$AASMLB", "$AASFLB", "$AASMLC", "$AASFLC", "$AASMMA", "$AASFMA", "$AASMMB", "$AASFMB", "$AASMP1", "$AASFP1", "$AASMP2", "$AASFP2", "$AASMP3", "$AASFP3", "$ACAMLA", "$ACAFLA", "$ACAMLB", "$ACAFLB", "$ACAMLC", "$ACAFLC", "$ACAMMA", "$ACAFMA", "$ACAMMB", "$ACAFMB", "$ACAMP1", "$ACAFP1", "$ACAMP2", "$ACAFP2", "$ACAMP3", "$ACAFP3", "$AENMLA", "$AENFLA", "$AENMLB", "$AENFLB", "$AENMLC", "$AENFLC", "$AENMMA", "$AENFMA", "$AENMMB", "$AENFMB", "$AENMP1","$AENFP1", "$AENMP2", "$AENFP2", "$AENMP3", "$AENFP3", "$AQUMLA", "$AQUFLA", "$AQUMLB", "$AQUFLB", "$AQUMLC", "$AQUFLC", "$AQUMMA", "$AQUFMA", "$AQUMMB", "$AQUFMB", "$AQUMP1", "$AQUFP1", "$AQUMP2", "$AQUFP2", "$AQUMP3", "$AQUFP3"]}
}

En la SEGUNDA capa:

$group

{
  _id: "null",
  DISCAPACIDAD_TOTAL: {
    $sum: "$DISCAPACIDAD"
  },
    ACCIDENTES_TOTAL: {
      $sum: "$ACCIDENTES"
    }
}


### 2. Si alguna de las preguntas implica el uso de más de una colección, entonces recuerda que posiblemente tendrás que realizar algunos preparativos:

#### Solo utilice una colección.

# NOTA:
## Para los casos que el resultado son un determinado número de documentos, configure en Settings en la opción Limit y también utilice Max Time y Number of Preview Documents

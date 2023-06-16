# Índice
1. [Estructura](#estructura)
2. [Variables](#variables)
3. [Formas Condicionales](#formas-condicionales)
4. [Bucles](#sección-2)
5. [Tipos de datos](#tipos-de-datos)
6. [Keywords](#keywords)
    1. [De espera](#de-espera)
    2. [Cambio de ventanas](#cambio-de-ventanas)
7. [Atributos](#atributos)
8. [Elementos HTML](#elementos-html)
9. [Extensiones](#elementos-html)
10. [Elementos HTML](#elementos-html)
11. [XPATH | DOM](#elementos-html)


# Estructura

``` python
*** Settings ***
Library   SeleniumLibrary
Library    BuiltIn
Resource    ../tmp/robot.resource
Resource    tmp/app.robot
Library    Recursos/RobotCBRs.py
Variables  credentials.yaml

*** Variables ***

*** Keywords ***

*** Test Cases ***

```

# Variables
- Definicion de variables en la sección `*** variables ***`
```python
# Datos comunes
${URL}  https://www.google.com

# Diccionario
&{TIPO_DOCUMENTO_DICT}    Certificado GP=Inscripciones de CBR    Copia Inscripcion Hipoteca=Inscripciones de CBR

# Lista
@{LISTA}    Inscripciones de CBR    Copia Inscripcion Inscripciones de CBR
```
- Defición de variable en otras secciones (kewywords/test cases)
```python
*** Keywords ***
Prueba
    # Establecer una variable global
    Set Global Variable    ${USUARIO}  ale 
```

```python
*** Keywords ***
Prueba
    # Si la operacion de posicion 4 es Nula o NOOK, entonces asignara a la variable como 1, caso contrario como 0
    ${isEscritura}=    Set Variable If    '${OPERATION}[4]' == '${None}' or '${OPERATION}[4]' == 'NOOK'    1    0
```

```python
*** Keywords ***
Prueba
    # Definir una variable con la fecha actual
    ${DATE_NOW}=    Get Current Date    result_format=%Y-%m-%d
```

# Formas Condicionales
- Variable asignada mediante condicional (si existe o no el valor de esa variable) y **EVALUATE**
```python
${isValue}=    Evaluate    "OK" if ${isPresentExhibit} else "NOOK"
```

- Evalua un booleano si una subcadena se encuentra en la cadena (lineal o multilineal)
```python
${contiene_rechazo}=    Evaluate    "Rechazo" in """${estado_resguardo}"""
```

- Ejecuta una keyword (en este caso, Set Variable o Evaluate) según una condicional (en este caso si es es igual a None)
```python
${FILE_COUNT}=    Run Keyword If    '${FILE_COUNT}' == 'None'    Set Variable    1    ELSE    Evaluate    ${FILE_COUNT} + 1
```

- Consulta si una variable de ambiente es igual a TEST
```python
IF    '%{ENVIROMENT}' == 'TEST'
    ...
END
```

- Comparación entre variables de cadena (se les agrega comillas siempre), en este caso ${element} viene a ser una lista
```python
IF    '${STAGE}' == ""

IF    '${element}[5]'=='Uno a Uno'
```

- Comparacion entre variable de numero
```python
IF    ${SEARCH} == 0
```

- Comparacion entre variable numerica (${COUNT}) con una cadena
```python
IF    '${COUNT}' == "2"
```

- En el caso de comprobar si una cadena esta presente en una variable de cadena
```python
IF    'alert-error' in '${TYPE_ALERT}' 
```

- Forma de evaluar y devolver un booleano mediante varias condicionales
```python
${isSantiagoFinalized}=    Evaluate  ${ACCOUNT} == 1 and '${ACCOUNTS}' == "Finalizada Despachada"
```

- Continuara a la siguiente iteraicon del bucle si se cumple esa condicion
```python
FOR ..
    IF    '${consultar}'=='OK' or '${consultar}'=='EN REPARO'    CONTINUE
    END
END
```

- Negacion de un booleano
```python
IF    not ${isDocument}
```

- Buscar si un elemento se encuentra en una lista
```python
${VAR}=    Create List    abc    cde    fgh
IF    'abc' in ${VAR}
```

- Buscamos si un elemento cadena no esta en la lista de cadenas
```python
IF    'Copia Maestra' not in ${LIST_DOCUMENTS}
```

# Bucles


# Tipos de datos


# Keywords

## De espera

- Comprobamos que en un timeout de 30 segundos se ejecute con exito la keyword/funcion python `Verify File  arg1`, con un espacio de 2s en el siguiente reintento (por defecto tiene 5 reintentos)
```python
Wait Until Keyword Succeeds    30s    2s    Verify File    ${PATH_SAVE}${TITLE}

# Otras maneras de definir
#       | Wait Until Keyword Succeeds | 2 min | 5 sec | My keyword | argument |
#       | ${result} = | Wait Until Keyword Succeeds | 3x | 200ms | My keyword |
#       | ${result} = | Wait Until Keyword Succeeds | 3x | strict: 200ms | My keyword |
```

- Comprobamos que en un timeout de 5s(por defecto) espera que un elemento sea visible, caso contrario error
```python
Wait Until Element Is Visible    //td[@id="cke_contents_ta"]/iframe
```

- Retorna un booleano si el elemento con text()="${OBSERVATION}" aparece en un tiempo de 30s
```python
${observationPresent}=    Run Keyword And Return Status    Wait Until Element Is Visible    //table[@id="TablaComentarios"]/tbody/tr[1]/td[3][text()="${OBSERVATION}"]    timeout=30
```

- Espera(por defecto 5s) a que ese elemento se encuentre incrustado en el DOM
```python
Wait Until Page Contains Element    //span[@id="err"]
```

- Espera un determinado timeout si tal archivo se llegó a descargar, devolviendo su estado como un booleano
```python
${isPresentCopy} =    Run Keyword And Return Status    Wait Until Created    ${PATH_SAVE}${TITLE_ORIGINAL_COPY}    timeout=15
```

- Espera que aparezca una opcion determinada del select, ya que a veces segun la circunstancias, un select puede tener distintas opciones y puede que se demore en cargas todas las opciones
```python
Wait Until Element Is Visible    //select[@id="ddlTipoDocumentoSolicitudBus"]/option[text()="${TIPO_DOCUMENTO_WF}"]    timeout=10
```

## Cambios de Ventana


# Atributos
- Añadir un id personalizado a un elemento
```python
# Asignamos un id, y luego en su interior del elemento localizado asignamos el valor de ${observacion} en el contenido
Assign Id To Element    //body[@class="cke_show_borders"]   texto
Execute Javascript  document.getElementById("texto").innerHTML= "${observacion}";
```

- Esperar que el elemento tenga como texto el valor de ${OBSERVATION}
```python
Wait Until Element Is Visible    //table[@id="TablaComentarios"]/tbody/tr[1]/td[3][text()="${OBSERVATION}"]    timeout=30
```

- Obtener la segunda colulmna del ultimo elemento de todas filas tr 
```python
${STAGE_CBR}=     Get Text    //tbody[@id="bodyMovimiento"]/tr[last()]/td[2]
```

- Obtener texto expecificando por variable el numero de posicion de tr + 1
```python
${DOCUMENT_TEXT}=    Get Text    //div[@id="documentosblock"]/table/tbody/tr[${ROW+1}]/td[1]
```

- Cantidad de elementos dentro del elemento tr
```python
${ROWS}=    Get Element Count     //div[@id="documentosblock"]/table/tbody/tr
```

- Obtenemos un atributo especifico (ejm: de 'a' su atributo 'href')
```python
${LINK}=    Get Element Attribute    //div[@id="documentosblock"]/table/tbody/tr[${ROW}]/td[2]/a    href
```

- Clickeamos  un elemento(puede tener varias clases) que contiene dicha clase "textAdd"
```python
Click Element   //a[contains(@class, "textAdd")]
```

- Seleccionar Select de tal atributo name, con el contenido de una option como "Bancos y Financieras - Carta de Resguardo"
```python
Select From List By Label    //select[@name="familiaFech"]      Bancos y Financieras - Carta de Resguardo
```

# Elementos HTML


# Extensiones


# XPATH | DOM


[Regresar al Índice](#índice)

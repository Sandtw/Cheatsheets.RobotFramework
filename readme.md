# Índice
1. [Estructura](#estructura)
2. [Variables](#variables)
3. [Formas Condicionales](#formas-condicionales)
4. [Bucles](#bucles)
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
```

# Variables
- 
```python
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

## Cambios de Ventana


# Atributos


# Elementos HTML


# Extensiones


# XPATH | DOM


[Regresar al Índice](#índice)

Aplicações de LexSyntax
=======================

## Números decimais

### Definição

```antlr
EXPRESSAO
    : NUMERO
    ;

NUMERO
    : DECIMAL+ SEPARADOR DECIMAL+
    | DECIMAL
    ;

DECIMAL
    : '0'
    | '1'
    | '2'
    | '3'
    | '4'
    | '5'
    | '6'
    | '7'
    | '8'
    | '9'
    ;

SEPARADOR
    : '.'
    ;
```

### Valores aceitso

* `17.89`
* `1`
* `000`
* `8878787.077676`

## Datas

### Definição

```antlr
EXPRESSAO
    : DATA_FULL
    | DATA
    ;

DATA
    : DECIMAL{1,2} SLASH DECIMAL{1,2} SLASH DECIMAL{2,4}
    ;

DATA_FULL
    : DECIMAL{2} SLASH DECIMAL{2} SLASH DECIMAL{4}
    ;

DATA_ISO
    : DECIMAL{4} DASH DECIMAL{2} DASH DECIMAL{2}

DECIMAL
    : '0'
    | '1'
    | '2'
    | '3'
    | '4'
    | '5'
    | '6'
    | '7'
    | '8'
    | '9'
    ;

SLASH
    : '/'
    ;

DASH
    : '-'
    ;
```

### Valores aceitso

* `00/00/0000`
* `00/00/00`
* `0/0/0000`
* `0/0/00`
* `0000-00-00`

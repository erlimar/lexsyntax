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
    : DIAMES SLASH DIAMES SLASH ANO
    ;

DIAMES
    : DECIMAL{2}
    | DECIMAL
    ;

ANO
    : DECIMAL{4}
    | DECIMAL{2}
    ;

DATA_FULL
    : DIAMES_FULL SLASH DIAMES_FULL SLASH ANO_FULL
    ;

DIAMES_FULL
    : DECIMAL{2}
    ;

ANO_FULL
    : DECIMAL{4}
    ;

DATA_ISO
    : ANO_FULL DASH DIAMES_FULL DASH DIAMES_FULL

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


## ANTLR

```antlr
EXPR
    : NAME_EXPR ':' TYPE_EXPR '|' TYPE_EXPR ';'
    | NAME_EXPR ':' TYPE_EXPR ';'
    ;

OR_EXPR
    : '|' TYPE_EXPR
    ;

TYPE_EXPR
    : NAME_WITH_STATIC_EXPR
    | NAME_WITH_INFINIT_EXPR
    | NAME_EXPR
    | CHAR_EXPR
    ;

NAME_WITH_STATIC_EXPR
    : NAME_EXPR '{' DECIMAL+ '}' // NAME{00}

NAME_WITH_INFINIT_EXPR
    : NAME_EXPR '+' // NAME+
    ;

NAME_EXPR
    : CHAR+ // NAME_NAME_NAME
    ;

CHAR_EXPR
    : '\'' CHAR '\'' // 'C'
    ;

CHAR
    : ['a'..'z']
    | ['A'..'Z']
    | DECIMAL
    | '_'
    ;

DECIMAL
    : [0..9]
    ;
```

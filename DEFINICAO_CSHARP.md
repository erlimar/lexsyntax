Definição C# .NET de LexSyntax
==============================

## Representação ANTLR

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

## Representação C#

```csharp
class Token {}
class CharToken : Token {}
class GroupToken : Token {}
class ListToken : Token { int Count = 1; Token[] List; } //0: Infinite, 1...N

var separadorToken = new CharToken("SEPARADOR", new char[] { '.' });
var decimalToken = new CharToken("DECIMAL", new char[] { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' });
var numeroToken = new ListToken("NUMERO") {
    List = new Token[] {
        // : DECIMAL+ SEPARADOR DECIMAL+
        new ListToken {
            List = new Token[] {
                new ListToken[] { List = new Token[] { decimalToken }, Count = 0 },
                separadorToken,
                new ListToken[] { List = new Token[] { decimalToken }, Count = 0 }
            }
        },
        // | DECIMAL
        decimalToken
    }
});
var expressao = new ListToken("EXPRESSAO", new Token[] { numeroToken });

var resultFail = expressao.Eval("6s7,90"); // Throw Exception!
var resultOk = expressao.Eval("76.56"); // Success!

delegate EvalFunc = (string code) => return new TokenResult();
```

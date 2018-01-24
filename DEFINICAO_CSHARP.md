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

## Representação C# com API Fluente

```csharp
var grammar = new LexSyntaxGrammar()

    .Def("EXPRESSAO", _ => _.Rule("NUMERO"))
    
    .Def("NUMERO", _ => _
        .List(l => l
            .Unlimited("DECIMAL")
            .Rule("SEPARADOR")
            .Unlimited("DECIMAL")
        )
        .Rule("DECIMAL")
    )

    .Def("DECIMAL", _ => _.CharRange('0','9'))

    .Def("SEPARADOR", _ => _.Char('.'))

    ;

var scanner = new LexSyntaxScanner(grammar);

LexSyntaxTree rFail = scanner.Parse(new LexSyntaxStringStream("6s7,90"));
LexSyntaxTree rOk = scanner.Parse(new LexSyntaxStringStream("76.56"));

Assert.False(rFail.IsValid);
Assert.True(rOk.IsValid);

Assert.Null(rFail.Root);
Assert.NotNull(rOk.Root);

Assert.Equal("NUMERO", rOk.Root.Name);
Assert.Equal(LexSyntaxTreeType.Rule, rOk.Root.Type);
```

Um Exemplo de LexSyntax
=======================

Imagine que você deseja que um usuário entre com um determinado valor em um campo texto qualquer
em uma interface gráfica conhecida. Esse valor deve ser uma distância.

Você já sabe que a distância será medida em metros, e no final deve ser uma valor numérico
decimal, algo como:

```js
17.89
```

O que seria o mesmo que `17.89 mt`. Então você deixaria o usuário informar o valor `17.89` e
omitiria o `mt`, porque você já sabe que a distância sempre será em metros.

Logo você poderia descrever **formalmente** essa **expressão** que o usuário deveria informar
como abaixo:

```antlr
EXPRESSAO
    : NUMERO
    ;

NUMERO
    : DECIMAL+ SEPARADOR DECIMAL+
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

Nessa nossa **descrição formal** acima nós definimos 4 (quatro) símbolos (também chamados **tokens**),
são eles: `EXPRESSAO`, `NUMERO`, `DECIMAL` e `SEPARADOR`.

* `EXPRESSAO`

  É basicamente nosso ponto de partida, o objetivo da nossa descrição, ou a própria expressão definida.
  Também opderíamos chamá-la de `ROOT`, que é a raiz de nossa descrição.
  
  Nossa `EXPRESSAO` pode ser somente uma coisa: Um `NUMERO`.
  
  Não sabemos ainda o que é um `NUMERO`, mas já sabemos que: quando soubermos disso, já conheceremos se
  temos uma `EXPRESSAO` válida ou não e qual seu conteúdo.
  
  A partir desse ponto, o que temos em seguida são simples decomposições de símbolos, ou **tokens**.

* `NUMERO`

  Esse por sua vez, pode ser somente uma coisa: Uma combinação de um ou mais `DECIMAL` _(O símbolo `+`
  no fim do nome indica isso)_, seguido de um  `SEPARADOR`, seguido de uma combinação de um ou mais
  `DECIMAL` novamente.
  
  Da mesma forma, ainda não sabemos o que é um `DECIMAL` nem o que é um `SEPARADOR`, mas quando
  soubermos...

* `DECIMAL`

  Esse é não é tão complexo, porque é na verdade um caractere `0` ou o caractere `1` ou o caractere
  `2` ou o caractere `3` ou o caractere `4` ou o caractere `5` ou o caractere `6` ou o caractere `7`
  ou o caractere `8` ou o caractere `9`.
  
  Bem que eu poderia só dizer que é um dos caracteres: `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8` ou `9`.

* `SEPARADOR`

  E esse então, só pode ser um `.` (ponto). Nada além disso.

__Resumindo:__ A `EXPRESSAO` que esperamos é um `NUMERO`, que por sua vez deve ser um ou mais `DECIMAL` (que é um número de `0` a `9`) + um `SEPARADOR` (que é um ponto) + outro `DECIMAL`. Que poderia ser qualquer uma das sequências abaixo:

- `0.0`
- `1.23`
- `999.1`
  
## Explicando

O que nós fizemos acima na visão geral que apresentamos, foi simplesmente definir um termo, ou melhor
uma `EXPRESSAO`, que é explicado por um conjunto possível e conhecido; significado por outros
simbolos que por sua vez são explicados individualmente, seguindo uma linha de decomposição de
palavras até chegar ao mínimo de um `caractere` ou uma lista de `caracteres` conhecida.

No fim das contas o que temos é uma restrição ao uso de um conjunto conhecido de caracteres que podem
ser usados em uma expressão, esses caracteres só podem ser agrupados em uma ordem estabelecida
rigidamente, e que qualquer coisa fora disso é uma **expressão inválida**.
Chamamos isso de **gramática**.

A **Gramática** determina um grupo de palavras _(tokens)_ que devem ser dispostas de uma forma _(syntax)_
para fazer sentido em seu idioma.

No exemplo acima, o que fizemos foi descrever formalmente um idioma básico, pois contém os _tokens_ e
a _syntax_ desse idioma. Com essas regras podemos avaliar se uma escrita está ou não nesse idioma.

## Voltando o Exemplo

Com essa explicação que clarea nossa mente (pelo menos parcialmente, eu espero), podemos então
voltar ao exemplo inicial.

Então pedimos ao usuário que nos informe _"a tal distância"_.

Ele então nos informa o valor **6s7,90**, e você logo diz? **POW!**: o valor está errado!

Logo em seguida ele nos informa o valor **76.56**, e você diz: **OBRIGADO!**: a distância é de
**76.56 metros!**

## Agora me explique você: Como você conseguiu determinar se o valor estava certo ou errado?

Foi tão rápido que você nem percebeu!

Mas você avaliou cada caractere dessa sequência da esquerda para a direita (se você fosse um
_oriental_ faria da direita para a esquerda, mas isso não vem ao caso agora), um após o outro,
vez por vez até chegar ao final ou até encontrar um erro.

### Vamos seguir o caminho que seu cérebro percorreu até descobrir que o valor estava errado

Você pegou a primeira expressão **"6s7,90"** e começou a avaliar cada caractere:

 1. Viu que a expressão espera um `NUMERO` e só.
 2. Pegou a expressão **"6s7,90"** e perguntou: é um `NUMERO`?
 3. Daí percebeu que você não sabia o que é um `NUMERO`, e então procurou em seu dicionário de
    símbolos o que significava um `NUMERO`. Foi aí que conheceu que só poderia ser uma combinação
    de um ou mais `DECIMAL`, seguido de um  `SEPARADOR`, seguido de uma combinação de um ou mais
   `DECIMAL` novamente.
 4. Daí com **"6s7,90"** montou sua matriz de verificação, que seria: `Lista de DECIMAL`, e
    `Um SEPARADOR` e `Lista de DECIMAL`. E começou a pegar as partes, a primeira é uma
    `Lista de DECIMAL`, mas espera aí: O que é um `DECIMAL`?
 5. Recorreu novamente a seu dicionário, e descobriu que se tratava de um dos seguintes caracteres:
    `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8` ou `9`.
 6. Agora sim, você começou a pegar a primeira parte, e percorreu a lista capturando os caracteres
    enquanto fossem um `DECIMAL`. O que na verdade só foi o caractere `6`, porque `s` não atendia
    ao critério.
 7. Beleza! Até agora já temos o primeiro item, uma `Lista de DECIMAL` contendo um item: `6`.
 8. Você então, a partir do que sobrou, ou seja **"s7,90"**, continuou a buscar suas partes.
    O que você esperava agora deveria ser `Um SEPARADOR`. E vejam só! Você também não sabe o que é
    um `SEPARADOR`.
 9. Volta a recorrer a seu dicionário, e descobre que um separador só pode ser o caractere `.` (ponto).
10. Com isso, você percebe que o caractere da vez, o **"s"** não atende ao requisito. E não
    encontrou um valor para `SEPARADOR`.
11. Nesse ponto não vale mais a pena continuar, porque:
    a) O que você esperava não era um `SPERADOR`, que era necessário para determinar um `NUMERO`,
    b) Que por sua vez, o `NUMERO` era a única opção que se tinha pra determinar o que é a `EXPRESSAO`
12. Então você concluíu: **Expressão Inválida!**. E foi por isso que você disse:
    **POW!**: o valor está errado!

Seguindo o raciocínio do nosso exemplo, que diz que: logo em seguida ele nos informa o valor
**76.56**, e você diz: **OBRIGADO!**: a distância é de **76.56 metros!**

### Vamos seguir novamente o caminho que seu cérebro percorreu até descobrir que o valor estava correto

Você pegou a segunda expressão **"76.56"** e começou a avaliar cada caractere:

 1. Viu que a expressão espera um `NUMERO` e só.
 2. Pegou a expressão **"76.56"** e perguntou: é um `NUMERO`?
 3. Daí percebeu que você não sabia o que é um `NUMERO`, e então procurou em seu dicionário de
    símbolos o que significava um `NUMERO`. Foi aí que conheceu que só poderia ser uma combinação
    de um ou mais `DECIMAL`, seguido de um  `SEPARADOR`, seguido de uma combinação de um ou mais
   `DECIMAL` novamente.
 4. Daí com **"76.56"** montou sua matriz de verificação, que seria: `Lista de DECIMAL`, e
    `Um SEPARADOR` e `Lista de DECIMAL`. E começou a pegar as partes, a primeira é uma
    `Lista de DECIMAL`, mas espera aí: O que é um `DECIMAL`?
 5. Recorreu novamente a seu dicionário, e descobriu que se tratava de um dos seguintes caracteres:
    `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8` ou `9`.
 6. Agora sim, você começou a pegar a primeira parte, e percorreu a lista capturando os caracteres
    enquanto fossem um `DECIMAL`. O que agora foram os caracteres `7` e `6`, porque `.` não atendia
    ao critério.
 7. Beleza! Até agora já temos o primeiro item, uma `Lista de DECIMAL` contendo dois itens: `7` e `6`.
 8. Você então, a partir do que sobrou, ou seja **".56"**, continuou a buscar suas partes.
    O que você esperava agora deveria ser `Um SEPARADOR`. E vejam só! Você também não sabe o que é
    um `SEPARADOR`.
 9. Volta a recorrer a seu dicionário, e descobre que um separador só pode ser o caractere `.` (ponto).
10. Com isso, você percebe que o caractere da vez, o **"."** é exatamente o que espera. E determina
    o valor de `SEPARADOR` como `.`.
11. Beleza de novo! Até agora já temos dois itens, uma `Lista de DECIMAL` contendo dois itens:
    `7` e `6`; e `Um SEPARADOR` com valor `.`.
12. Você então, a partir do que sobrou, ou seja **"56"**, continuou a buscar suas partes.
    O que você esperava agora deveria ser outra `Lista de DECIMAL`. Mas agora você já sabe o que é.
13. Você novamente, percorreu a lista capturando tudo que era um `DECIMAL`. O que agora foram os
    caracteres `5` e `6`, porque você chegou ao fim. E chegamos ao último item, uma `Lista de DECIMAL`
    contendo dois itens: `5` e `6`.
14. Nesse ponto, como você já chegou ao fim dos dados, fez a conferência e notou:
    a) O que você esperava para um `NUMERO` eram três itens. E você **TEM** três itens;
    b) O _primeiro item_ deve ser uma sequência de números decimais: E você **TEM** como primeiro
       item a lista (`7`,`6`);
    c) O _segundo item_ deve ser um caractere `.`: E você **TEM** como segundo item o caractere `.`;
    d) O _terceiro item_ deve ser outra sequência de números decimais: E você **TEM** como terceiro
       item a lista (`5`, `6`);
15. Com todos os itens verificados, você obteve o seguinte resultado:
```js
let resultado = {
  "tipo": "token",
  "id": "EXPRESSAO",
  "valor": {
    "tipo": "token",
    "id": "NUMERO",
    "valor": [
      {
        "tipo": "token",
        "id": "DECIMAL",
        "valor": "76"
      },
      {
        "tipo": "token",
        "id": "SEPARADOR",
        "valor": "."
      },
      {
        "tipo": "token",
        "id": "DECIMAL",
        "valor": "56"
      }
    ]
  }
}
```
16. Então você concluíu: **Expressão válida!**.
17. Em seguida, com o resultado em mãos, e como a estrutura já era conhecida, você
    pode, com toda segurança fazer uma conversão simples:
```js
let valor_parte1 = resultado.valor.valor[0];
let valor_parte2 = resultado.valor.valor[2];
let valor_final = valor_parte1 + '.' + valor_parte2 + ' metros';
```
18. Por fim, disse: **OBRIGADO!**: a distância é de **76.56 metros!**

## Conclusão

No exemplo que apresentamos o que ocorreu com seu cérebro, foi que você extraiu os dados de um texto
e analisou com base nas definições de um idioma que você conhecia, e com o resultado da análise você
compilou uma resposta.

Podemos então perceber que um scanner e analisador sintático, analisa e gera informações coerentes
para serem usadas por um próximo componente, um compilador, esse gera uma resposta mais adequada.

E para o propósito a que se destina esse componente **LexSyntax**, o compilador que falamos aqui não
é necessariamente um compilador de programas de computador de forma geral, mas um componente que compila
uma informação menor, usada em um programa, no caso um conversor de dados.

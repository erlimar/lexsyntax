LexSyntax
=========

LexSyntax é um scanner léxico pseudo analisador sintático.

O objetivo é criar um scanner de código capaz de identificar tokens de uma definição sintática.
Com ele temos uma forma de validar qualquer entrada de texto com base em uma definição sintática
básica.

O resultado imediato é que podemos **validar** uma entrada de texto com base em um formato
esperado.

Um resultado possível é que, com o objeto resultante de uma análise bem sucedida podemos emitir
alguma ação sobre os dados, obtendo assim um **emissor** de valores, ou seja, um conversor de
tipos de dados complexos.

## Visão Geral

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

```
<EXPRESSAO>      ::= <NUMERO_DECIMAL>
                   | <NUMERO_DECIMAL> <SEPARADOR> <NUMERO_DECIMAL>
                   ;
<NUMERO_DECIMAL> ::= <DECIMAL>
                   | <DECIMAL> <DECIMAL>
                   ;
<SEPARADOR>      ::= "."
                   ;
<DECIMAL>        ::= "0"
                   | "1"
                   | "2"
                   | "3"
                   | "4"
                   | "5"
                   | "6"
                   | "7"
                   | "8"
                   | "9"
                   ;
```

Nessa nossa **descrição formal** acima nós definimos 4 (quatro) símbolos (também chamados **tokens**),
são eles: `EXPRESSAO`, `NUMERO_DECIMAL`, `SEPARADOR` e `DECIMAL`.

* `<EXPRESSAO>`
  É basicamente nosso ponto de partida, o objetivo da nossa descrição, ou a própria expressão definida.
  Também opderíamos chamá-la de `<ROOT>`, que é a raiz de nossa descrição.
  
  Nossa `EXPRESSAO` pode somente 2 (duas) coisas. Um simples `NUMERO_DECIMAL` ou uma combinação
  de `NUMERO_DECIMAL`, um `SEPARADOR` e outro `NUMERO_DECIMAL`.
  
  Não sabemos ainda o que é um `NUMERO_DECIMAL` nem ao menos um `SEPARADOR`, mas já sabemos que,
  quando soubermos disso, já conheceremos se temos uma `EXPRESSAO` válida ou não.
  
  A partir desse ponto, o que temos em seguida são simples decomposições de símbolos, ou **tokens**.

* `<NUMERO_DECIMAL>`
  Um `NUMERO_DECIMAL` por sua vez, também só pode ser 2 (duas) coisas. Um `DECIMAL` ou uma
  combinação de `DECIMAL` e outro `DECIMAL`.

* `<SEPARADOR>`
  Um `SEPARADOR` é algo bem mais simples. Tão simples como um **caractere**. E nesse caso só pode
  ser o caractere `.` (ponto). Nada além disso.
  
* `<DECIMAL>`
  E um `DECIMAL` (obserque que não estamos falando de um `NUMERO_DECIMAL`, mas somente de um `DECIMAL`)
  é também algo tão simples quanto um **caractere**, porém um pouco menos complexo, porque é na
  verdade um caractere `0` ou o caractere `1` ou o caractere `2` ou o caractere `3` ou o caractere `4`
  ou o caractere `5` ou o caractere `6` ou o caractere `7` ou o caractere `8` ou o caractere `9`.
  
  Ufa!...
  
  Bem que eu poderia só dizer que é um dos caracteres da lista: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9.

## Explicando

O que nós fizemos acima na visão geral que apresentamos, foi simplesmente definir um termo, ou melhor
uma `<EXPRESSAO>`, que é explicado por um conjunto possível e conhecido, significado por outros
simbolos, que por sua vez são explicados individualmente, seguindo uma linha de decomposição de
palavras até chegar ao mínimo de um `caractere` ou uma lista de `caracteres` conhecida.

No fim das contas o que temos é uma restrição ao uso de um conjunto conhecido de caracteres que podem
ser usados em uma expressão, esses caracteres só podem ser agrupados em uma ordem estabelecida
rigidamente, e que qualquer coisa fora disso é uma **expressão inválida**.
Chamamos isso de gramática.

## Voltando a Visão Geral

Com essa explicação que clarea nossa mente, podemos então voltar ao exemplo.

Então queremos que o usuário nos informe "a tal distância".

Então o usuário nos informa **6s7,90**, e o que você diz? **POW!** valor errado.

Mas então ele nos informa **76.56**. E você diz: **OBRIGADO!** a distância é de **76.56 metros!**

## Agora me explique você, como conseguiu determinar se o valor estava certo ou errado?

Foi tão rápido que você nem percebeu. Mas você avaliou cada caractere dessa sequência da esquerda
para a direita (se você fosse um _oriental_ faria da direita para a esquerda, mas isso não vem
ao caso agora), um após o outro, vez por vez.

Você pegou a primeira expressão **"6s7,90"** e começou a avaliar cada caractere:

1. Viu que a expressão aceita `<NUMERO_DECIMAL>` e só, ou um `<NUMERO_DECIMAL>` com um `<SEPARADOR>`
   com um `<NUMERO_DECIMAL>`.
2. Pegou o primeiro caractere, **"6"** e perguntou é um `<NUMERO_DECIMAL>`?
3. Daí percebeu que você não sabia o que é um `<NUMERO_DECIMAL>`, e então procurou em seu dicionário
   de símbolos o que significava um `<NUMERO_DECIMAL>`. Então descobriu que se trata de um `<DECIMAL>`
   e só, ou um `<DECIMAL>` com um `<DECIMAL>`.
4. Daí com o **"6"** que já estava na mão e já sabendo o que era um `<NUMERO_DECIMAL>` perguntou,
   é um `<DECIMAL>`?
5. Daí, novamente percebeu que você também não sabia o que era um `<DECIMAL>`, recorreu mais uma vez
   a seu dicionário de símbolos, e descobriu que se tratava de um dos seguintes caracteres: 0, 1, 2,
   3, 4, 5, 6, 7, 8 ou 9. Agora ficou fácil, você descobriu que se tratava realmente de um `<DECIMAL>`
   válido.
   
   ** Continuar aqui!!!!!!!!!! **
   
6. Com isso você percebeu que ainda existem mais caracteres disponíveis, e que existem mais definiões
   compostas para essa expressão. Poderia ser um `<DECIMAL> <DECIMAL>`.
7. Daí lancou mão do próximo caractere na lista, no caso o **"s"**, para avaliar seu significado, se
   for um `<DECIMAL>` então o token ainda teria mais caracteres. Ao que também percebeu não se tratava
   de um `<DECIMAL>`. Portanto determinou que se tratava de um `<DECIMAL>` e só, com isso definiu
   aquele **"6"** como sendo o primeiro **TOKEN** encontrado, do tipo `<NUMERO_DECIMAL>` composto
   por um `<DECIMAL>` e só.
8. Se não houvesse mais nada, sua expressão seria válida como sendo de um `<NUMERO_DECIMAL>` e só,
   composta por um `<DECIMAL>` **"6"** e só. Mas como não era o caso, você tem mais caracteres
   disponíveis, continuou com sua busca por tokens válidos.
9. Viu que já que não se tratava de um `<NUMERO_DECIMAL>` e só, poderia se tratar de um Continuando agora do **"s"**. Mas você viu que
   não era o caso, pois não estava na lista válida de `<DECIMAL>`, ou seja: 0, 1, 2, 3, 4, 5, 6, 7,
   8 ou 9. Logo era **inválido**.
8. Como não fazia mais parte de um conteúdo aceito para o símbolo `<NUMERO_DECIMAL>`, pois não era
   um `<DECIMAL>` ou um `<DECIMAL>` `<DECIMAL>`. Procurou saber se havia uma outra alternativa.
9. E Notou que ainda poderia se tratar de um `<NUMERO_DECIMAL>` com um `<SEPARADOR>` e um
   `<NUMERO_DECIMAL>`. Como já sabe do que se trata um `<NUMERO_DECIMAL>` e que o tal **"s"** não
   era um

## Notas

Pra não gerar confusão, observe que falamos de um `<NUMERO_DECIMAL>` e um `<DECIMAL>`. E no nosso dia
a dia isso parece a mesma coisa, mas lembre-se que aqui são nomes de símbolos, e escolhemos dizer que
`<DECIMAL>` representa um único caractere de número decimal; ex: `0` ou `1` ou `2`. Já para
`<NUMERO_DECIMAL>` escolhemos para representar uma junção de caracteres de números decimais;
ex: `120`, `02` ou `098765`.
  

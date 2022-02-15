# Expressões regulares (regex)
É uma linguagem que permite encontrar padrões bem definidos (pettern) em textos (target). A interpretação do padrão e sua aplicação no alvo é feita por uma Regex Engine, que possui uma implementação para cada linguagem.

# Expressões regulares (regex)

2.1 Meta-char
Um meta caracter é um caracter cujo seu valor não é interpretado de forma literal pela Regex Engine, eles significam algo mais. Ex:

.: o caracter "ponto" significa qualquer caracter
( e ): os "parênteses" indicam um grupo que deve ser capturado
^: o acento chapéuzinho (também conhecido como circunflexo) "nega" um caracter, ou seja, captura qualquer um exceto o que está avaliando
a|b: caracter a ou b
Para capturar um meta char devemos escapar o caracter: \., \(, \).

2.2 Quantifier
Os quantificadores são meta caracteres que definem quantas vezes um padrão deve ser buscado em uma sequência direta. Ex:

*: o "asterisco" após um padrão, indica que este pode aparecer nenhuma ou muitas vez
?: zero ou uma vez
+: uma ou muitas vezes
{n}: exatamente n vezes
{n,}: no mínimo n vezes
{n, m}: no mínimo n vezes e no máximo m vezes
Por padrão os quantificadores vão selecionar o máximo de caracteres possível, mas podemos definir um comportamento preguiçoso (lazy) passando o operador ?.

2.3 Classe de caracteres
É um conjunto de caracteres que podem ser capturados. Existem classes predefinidas como:

\d: qualquer digito entre 0 e 9 ([0-9])
\w: chamada de word char, além dos digitos, captura as letras de a a Z e o underline ([a-zA-Z0-9_])
\s: chamada de white space, captura um espaço em branco ou tabulação ([\t\r\n\f])
Também podemos definir nossas próprias classes utilizando os colchetes (ou parênteses quadrados):

[0-7]: todos os números inteiros entre 0 e 7, inclusivos
[abc]: letras a, b e c
[1-36-9]: números entre 1 e 3 e também entre 6 e 9
[-.]: caracteres - e .
[a-eF-J]: caracteres minúsculos de a a e e os maiusculos de F até J
[a-zàáâãéêíôõú]: todas as letras minúsculas e suas variações acentuadas mais comuns no Portguês
Dentro das classes QUASE todos os caracteres são avaliados de acordo com seu valor literal, não sendo necessário escapa-los. Os únicos caractéres que se comportam como meta-chars dentro de classes são a \ (barra invertida), o - (hífen) e o ^ (circunflexo).

Outro ponto (xD) que deve ser frisado é que as classes definem apenas quais são os caracteres que podem ser capturados, a quantidade vai depender do quantificador aplicado.

2.4 Âncoras
Permitem selecionar posições no texto alvo. As âncoras não capturam nenhum caracter, apenas definem a partir de onde será iniciada a avaliação.

\b: denominada de word boundary, seleciona o início ou fim de uma palavra. Antes ou depois dessa âncora não pode haver um word char (\w)
\B: também existe a non-word boundary, que seleciona posições dentro de uma palavra. Antes ou depois dessa âncora deve existir um word char (\w)
^: seleciona o início da string alvo
$: seleciona o fim da string alvo
2.5 Grupos
Podemos quebrar o padrão regex avaliado em grupos, dessa forma as strings captuiradas por esses grupos serão retornadas quando a busca for executada. Para definir um padrão como grupo basta colocá-lo entre parênteses. Além disso, usando esse recurso podemos condicionar a captura de um padrão composto por várias classes e até tornar este grupo todo opcional usando o operador ?.

Também podemos definir que um grupo não seja retornado na resposta. Isso é chamado de non-capturing group, para fazê-lo passamos o caracter ? logo após abrir os parênteses e usamos : antes do padrão buscado no grupos.

Exemplo: (\w+), (\w+)? e (?:\w+).

2.6 Back-reference
A regex abaixo foi escrita com o objetivo de capturar uma tag do HTML. faz uso do operador | para que um padrão ou outro seja capturado Note que foi definido um grupo que faz uso do operador | para que a string h1 ou h2 seja capturada e esse grupo foi definido em dois locais diferentes:

<(h1|h2).+?>([\w\sàãáâéêíîõóôúç]+)</(h1|h2)>
Gostaríamos de capturar a mesma string em ambas as localizações do gropo,porém da forma como a regex está escrita as quatro tags abaixo seriam capturadas:

<h1>Tag capturada pela regex</h2>
<h2>Tag capturada pela regex</h2>
<h1>Tag capturada pela regex (inválida)</h2>
<h1>Tag capturada pela regex (inválida)</h1>
Quando estivermos na situação em que temos o mesmo grupo definido mais de uma vez na expressão regular e queremos que esse grupo tenha o mesmo comportamento em todos os locais, devemos utilizar o recurso de back-reference. Usando o a referêcia ao um grupo teremos o mesmo comportamento do grupo referênciado.

Para referênciar um grupo usamos a notação \índice, onde o índice 0 a própria regex, o índice 1 o grupo mais a esquerda e assim por diante.

<(h1|h2).+?>([\w\sàãáâÃéêíîõóôúç]+)</\1>
Assim garantimos que o mesmo padrão será capturado em ambas as posições:

<h1>Tag capturada pela regex</h2>
<h2>Tag capturada pela regex</h2>
<h1>Tag NÃO capturada pela regex (inválida)</h2>
<h1>Tag NÃO capturada pela regex (inválida)</h1>
Outra forma de escrever uma regex que satisfaça nossa exigência pode ser:

<(h[1-2])>.*</\1>
2.7 Selecionando o que não queremos
Existem situações em que definir o que não queremos capturar será mais simples.

Mais acima vimos o operador ^ que quando colocado dentro de uma classe se comporta como negação. Ex: [^x] vai capturar qualquer caracter exceto o x.

2.8 Exemplos
Testes: regex101

CEP
/^[0-9]{2}\.[0-9]{3}\-[0-9]{3}$/
exemplo: 12.345-678
/^: início da linha
[0-9]{2}: duas ocorrências de um número no intervalo passado (12)
\.: o caracter especial "ponto" que separa 12 e 345
[0-9]{3}: ocorrências de um número no intervalo passado (345)
\-: o caracter especial "traço" que separa 345 e 678
[0-9]{3}: ocorrências de um número no intervalo passado (345)
/$: fim de linha
Email
/^[a-z0-9.]+@[a-z0-9]+.[a-z]+.([a-z]+)?$/
HTML
<h1.+?>([\w\sãàáâõóôíúç.]+)</h1>
exemplo: <h1 class="text-left">Expressões regulares</h1>
<h1: valor literal
<h1
.+: qualquer caracter, mais de uma vez
<h1 class="text-left">Expressões regulares</h1>
.+?>: todos os caracteres até >
<h1 class="text-left">
([\w\sãàáâõóôíúç.]+): conteúdo da tag
<h1 class="text-left">Expressões regulares
</h1>: valor literal
<h1 class="text-left">Expressões regulares</h1>
3 Capturando expressões regulares com PHP
A função mais simples do PHP para avaliar expressões regulares é a preg_match(). Essa função possui como parâmetros obrigatórios a regex e o texto alvo, nessa ordem. Seu retorno é um valor inteiro se o padrão foi encontrado (1) no alvo ou não (0), e se um erro ocorrer retorna false.

O terceiro parâmetro é opcional e será a variável para armazenar os resultados. Caso o padrão seja encontrado, essa variável receberá um vetor em que o primeiro elemento será o padrão completo e os próximos serão os grupos definidos.

A preg_match() executa apenas uma vez, ou seja, assim que encontrar um padrão sua execução encerra. Se o objetivo for varrer toda a string (executar de forma global) para obter todos os matches, deve-se usar a função preg_match_all(). Retorna é um inteiro para o número de vezes que o padrão completo for encontrado ou false se ocorrer algum erro.

Se for passada uma váriavel para armazenar os resultados, está vai receber um array de arrays. Com o primeiro array armazenando todas as ocorrências do padrão completo, o segundo array com todas as ocorrências do primeiro grupo e assim por diante.

Também podemos passar flags para obter informações extras, como o índice de cada padrão/grupo capturado.

3.1 Substituindo
O PHP posssui uma função muito interessante para substituição que faz uso das regexes, a preg_replace(). Essa função recebe como parâmetros a regex, o texto substituto e o alvo, nessa ordem. O texto substituto também pode ser uma regex e podemos até referênciar grupos da primeira regex usando a notação $n, onde n é o índice do grupo. No exemplo abaixo, o formato da data 2020-10-05 é alterado para 05/10/2020:

preg_replace("/(\d{4})-(\d{2})-(\d{2})/", "$3/$2/$1", "2020-10-05")
Outro detalhe que não foi mencionado é que devemos "rodear" a string que representa a regex com os caracteres ~ ou /.

4 Dados brasileiros
4.1 Validando CPF
Os últimos dois números de um CPF (x e y, no exemplo que segue) são chamados de digitos verificadores e podem ser obtidos a partir dos outros números através de alguns cálculos. Esse algoritmo é carinhosamente chamado de "módulo 11", a seguir são apresentados os passos para obter os digitos verificadores do número de CPF 123.456.789-xy:

Primeiro digito verificador (x)
preenchemos a tabela abaixo colocando os números do CPF na ordem na primeira linha e na segunda os pesos de cada número:

CPFi	1	2	3	4	5	6	7	8	9
Pesoi	10	9	8	7	6	5	4	3	2
CPFi x Pesoi	10	18	24	28	30	30	28	24	18
Após multiplicar cada um dos números no CPF pelo seu peso, somamos todos estes números:
$$\sum_{i=1}^{9} CPF_i * Peso_i = 10 + 18 + 24 + 28 + 30 + 30 + 28 + 24 + 18 = 210 = resultadoSoma_1$$

$$resultadoSoma_1 = 210$$

o resto da divisão deste total (resultadoSoma1) por 11 deve ser subtraído de 11:

210 % 11 = 1

11 - 1 = 10

"Se o resultado da subtração for maior que 9, o dígito é ZERO. Caso contrário, o dígito verificador é o resultado dessa subtração."

Portanto, o primeiro digito verificador do CPF apresentado acima é 0.

obtendo o segundo digito verificador (y)
repetimos o processo anterior, mas agora adicionando o digito verificador na tabela e modificando os pesos:
CPFi	1	2	3	4	5	6	7	8	9	0
Pesoi	11	10	9	8	7	6	5	4	3	2
CPFi x Pesoi	11	20	27	32	35	36	35	32	27	0
$$resultadoSoma_2 = \sum_{i=1}^{10} CPF_i * Peso_i = 255$$

255 % 11 = 2
11 - 2 = 9
"Se o resultado da subtração for maior que 9, o dígito é ZERO. Caso contrário, o dígito verificador é o resultado dessa subtração."

Portanto, o segundo digito verificador do CPF apresentado acima é 9.
4.2 Validando CNPJ
O CNPJ também possui regras específicas de validação para obter os digitos verificadores, a diferença para o CPF fica pela quantidade de caracteres e os pesos. Tomando o número 12.345.678/9123-xy como exemplo:

obtendo o primeiro digito verificador (x)
i	1	2	3	4	5	6	7	8	9	10	11	12
CNPJi	1	2	3	4	5	6	7	8	9	1	2	3
Pesoi	5	4	3	2	9	8	7	6	5	4	3	2
CNPJi x Pesoi	5	8	9	8	45	48	49	48	45	4	6	6
$$total_1 = \sum_{i=1}^{9} CPF_i * Peso_i = 5 + 8 + 9 + 8 + 45 + 48 + 49 + 48 + 40 + 4 + 6 + 6 = 281 = total_1$$

$$total_1 = 281$$

281 % 11 = 6
11 - 6 = 5
"Se o resto da divisão totali for menor que 2, o dígito é ZERO. Caso contrário, o dígito verificador é o resultado da subtração do resto de 11."

Como o resto de divisão do total por 11 é 1, o primeiro digito verificador do nosso exemplo é 5.

obtendo o segundo digito verificador (y)
i	1	2	3	4	5	6	7	8	9	10	11	12	13
CNPJi	1	2	3	4	5	6	7	8	9	1	2	3	5
Pesoi	6	5	4	3	2	9	8	7	6	5	4	3	2
CNPJi x Pesoi	6	10	12	12	10	54	56	56	54	5	8	9	10
total2 = 302

302 % 11 = 6
11 - 5 = 6
Como o resto da divisão de total2 por 11 é maior que 1, o segundo digito verificador do CNPJ é 6.

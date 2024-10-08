O laboratório tem como objetivo familiarizar os alunos com criptografia de chave secreta, explorando algoritmos, modos de criptografia, preenchimentos e vetores iniciais (IV). A prática envolve o uso de ferramentas e programação para cifrar e decifrar mensagens, destacando erros comuns na implementação que podem resultar em vulnerabilidades. Os tópicos abrangem cifra de substituição, análise de frequência e equívocos típicos na utilização de algoritmos criptográficos.

##  Lab Task 1: Frequency Analysis

Nesta tarefa de análise de frequência, enfrentamos um texto cifrado por uma cifra monoalfabética. O objetivo é descobrir o texto original utilizando análise de frequência. Inicialmente, geramos uma chave de encriptação permutando o alfabeto e a usamos para cifrar um artigo simplificado. Após converter maiúsculas em minúsculas e remover pontuações e números, aplicamos a cifra monoalfabética. O desafio é usar o programa Python freq.py para analisar n-gramas, incluindo frequências de letras, bigramas e trigramas. Com a análise de frequência, busca-se descobrir a chave de encriptação e o texto original, aproveitando recursos online, como a Wikipedia, para referências adicionais sobre frequência de caracteres. O processo envolve substituir caracteres conhecidos no texto cifrado para facilitar a identificação, utilizando o comando tr.

Vamos permutar o alfabeto de a a z usando Python, e usar o alfabeto permutado como chave.

```bash
#!/bin/env python3
import random

s = "abcdefghijklmnopqrstuvwxyz"
list = random.sample(s, len(s))
key = ''.join(list)
print(key)
```

Mantemos os espaços para simplificar a tarefa. Fizemos isso usando o seguinte comando:

```bash
$ tr [:upper:] [:lower:] < article.txt > lowercase.txt
$ tr -cd ’[a-z][\n][:space:]’ < lowercase.txt > plaintext.txt
```

Usamos o comando tr para fazer a encriptação. Apenas encriptamos letras, deixando o espaço e os caracteres de retorno em paz.

```bash
$ tr 'abcdefghijklmnopqrstuvwxyz' 'sxtrwinqbedpvgkfmalhyuojzc' < plaintext.txt > ciphertext.txt
```

A partir daqui, o objetivo será descobrir a cifra, e, do mesmo modo, o texto original a partir de análise de frequência.

Tal como sugerido no guião, usamos [recursos](https://en.wikipedia.org/wiki/Frequency_analysis) já existentes para facilitar a nossa busca.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d5/English_letter_frequency_%28alphabetic%29.svg/340px-English_letter_frequency_%28alphabetic%29.svg.png)

Primeiramente, obtemos os seguintes resultados ao correr o ficheiro `freq.py` fornecido:

```
-------------------------------------
1-gram (top 20):
n: 488
y: 373
v: 348
x: 291
u: 280
q: 276
m: 264
h: 235
t: 183
i: 166
p: 156
a: 116
c: 104
z: 95
l: 90
g: 83
b: 83
r: 82
e: 76
d: 59
-------------------------------------
2-gram (top 20):
yt: 115
tn: 89
mu: 74
nh: 58
vh: 57
hn: 57
vu: 56
nq: 53
xu: 52
up: 46
xh: 45
yn: 44
np: 44
vy: 44
nu: 42
qy: 39
vq: 33
vi: 32
gn: 32
av: 31
-------------------------------------
3-gram (top 20):
ytn: 78
vup: 30
mur: 20
ynh: 18
xzy: 16
mxu: 14
gnq: 14
ytv: 13
nqy: 13
vii: 13
bxh: 13
lvq: 12
nuy: 12
vyn: 12
uvy: 11
lmu: 11
nvh: 11
cmu: 11
tmq: 10
vhp: 10
```
E logo de imediato, podemos considerar os elementos mais comuns, `n`, `yt` e `ytn` como sendo `e`, `th`, e `the`, respetivamente, sendo esses a letra, dígrafo e trígrafo mais comuns.

Seguidamente, continuamos a efetuar modificações através da análise de frequência no texto cifrado e recursos fornecidos:

| Substituição | Texto | Cifra |
| :----------: | :---: | :---: |
| `ytn` ~ `THE` | THE xqavhq Tzhu  xu qzupvd lHmaH qEEcq vgxzT hmrHT vbTEh THmq ixur qThvurE vlvhpq Thme THE gvrrEh bEEiq imsE v uxuvrEuvhmvu Txx | `----n--t-----------y------` |
| `vup` ~ `AND` | THE xqaAhq TzhN  xN qzNDAd lHmaH qEEcq AgxzT hmrHT AbTEh THmq ixNr qThANrE AlAhDq Thme THE gArrEh bEEiq imsE A NxNArENAhmAN Txx | `v--pn--t-----u-----y------` |
| `x` ~ `O` | THE OqaAhq TzhN  ON qzNDAd lHmaH qEEcq AgOzT hmrHT AbTEh THmq iONr qThANrE AlAhDq Thme THE gArrEh bEEiq imsE A NONArENAhmAN TOO | `v--pn--t-----ux----y------` |
| `m` ~ `I` | THE OqaAhq TzhN  ON qzNDAd lHIaH qEEcq AgOzT hIrHT AbTEh THIq iONr qThANrE AlAhDq ThIe THE gArrEh bEEiq iIsE A NONArENAhIAN TOO | `v--pn--tm----ux----y------` |
| `r` ~ `G` | THE OqaAhq TzhN  ON qzNDAd lHIaH qEEcq AgOzT hIGHT AbTEh THIq iONG qThANGE AlAhDq ThIe THE gAGGEh bEEiq iIsE A NONAGENAhIAN TOO | `v--pn-rtm----ux----y------` |
| `h` ~ `R` | THE OqaARq TzRN  ON qzNDAd lHIaH qEEcq AgOzT RIGHT AbTER THIq iONG qTRANGE AlARDq TRIe THE gAGGER bEEiq iIsE A NONAGENARIAN TOO | `v--pn-rtm----ux--h-y------` |
| `q` ~ `S` | THE OSaARS TzRN  ON SzNDAd lHIaH SEEcS AgOzT RIGHT AbTER THIS iONG STRANGE AlARDS TRIe THE gAGGER bEEiS iIsE A NONAGENARIAN TOO | `v--pn-rtm----ux--hqy------` |
| `b` ~ `F` | THE OSaARS TzRN  ON SzNDAd lHIaH SEEcS AgOzT RIGHT AFTER THIS iONG STRANGE AlARDS TRIe THE gAGGER FEEiS iIsE A NONAGENARIAN TOO | `v--pnbrtm----ux--hqy------` |
| `az` ~ `CU` | THE OSCARS TURN  ON SUNDAd lHICH SEEcS AgOUT RIGHT AFTER THIS iONG STRANGE AlARDS TRIe THE gAGGER FEEiS iIsE A NONAGENARIAN TOO | `v-apnbrtm----ux--hqyz-----` |
| `ldgis` ~ `WYBLK` | THE OSCARS TURN  ON SUNDAY WHICH SEEcS ABOUT RIGHT AFTER THIS LONG STRANGE AWARDS TRIe THE BAGGER FEELS LIKE A NONAGENARIAN TOO | `vgapnbrtm-si-ux--hqyz-l-d-` |
| `ce` ~ `MP` | THE OSCARS TURN  ON SUNDAY WHICH SEEMS ABOUT RIGHT AFTER THIS LONG STRANGE AWARDS TRIP THE BAGGER FEELS LIKE A NONAGENARIAN TOO |`vgapnbrtm-sicuxe-hqyz-l-d-` |

Having successfully deciphered the first paragraph, the rest is simple. With the final substitution being `fkjow` ~ `VXQJZ`, we obtain the cipher `vgapnbrtmosicuxejhqyzflkdw` and the completely deciphered text down below:

```THE OSCARS TURN  ON SUNDAY WHICH SEEMS ABOUT RIGHT AFTER THIS LONG STRANGE
AWARDS TRIP THE BAGGER FEELS LIKE A NONAGENARIAN TOO

THE AWARDS RACE WAS BOOKENDED BY THE DEMISE OF HARVEY WEINSTEIN AT ITS OUTSET
AND THE APPARENT IMPLOSION OF HIS FILM COMPANY AT THE END AND IT WAS SHAPED BY
THE EMERGENCE OF METOO TIMES UP BLACKGOWN POLITICS ARMCANDY ACTIVISM AND
A NATIONAL CONVERSATION AS BRIEF AND MAD AS A FEVER DREAM ABOUT WHETHER THERE
OUGHT TO BE A PRESIDENT WINFREY THE SEASON DIDNT JUST SEEM EXTRA LONG IT WAS
EXTRA LONG BECAUSE THE OSCARS WERE MOVED TO THE FIRST WEEKEND IN MARCH TO
AVOID CONFLICTING WITH THE CLOSING CEREMONY OF THE WINTER OLYMPICS THANKS
PYEONGCHANG

ONE BIG QUESTION SURROUNDING THIS YEARS ACADEMY AWARDS IS HOW OR IF THE
CEREMONY WILL ADDRESS METOO ESPECIALLY AFTER THE GOLDEN GLOBES WHICH BECAME
A JUBILANT COMINGOUT PARTY FOR TIMES UP THE MOVEMENT SPEARHEADED BY 
POWERFUL HOLLYWOOD WOMEN WHO HELPED RAISE MILLIONS OF DOLLARS TO FIGHT SEXUAL
HARASSMENT AROUND THE COUNTRY

SIGNALING THEIR SUPPORT GOLDEN GLOBES ATTENDEES SWATHED THEMSELVES IN BLACK
SPORTED LAPEL PINS AND SOUNDED OFF ABOUT SEXIST POWER IMBALANCES FROM THE RED
CARPET AND THE STAGE ON THE AIR E WAS CALLED OUT ABOUT PAY INEQUITY AFTER
ITS FORMER ANCHOR CATT SADLER QUIT ONCE SHE LEARNED THAT SHE WAS MAKING FAR
LESS THAN A MALE COHOST AND DURING THE CEREMONY NATALIE PORTMAN TOOK A BLUNT
AND SATISFYING DIG AT THE ALLMALE ROSTER OF NOMINATED DIRECTORS HOW COULD
THAT BE TOPPED

AS IT TURNS OUT AT LEAST IN TERMS OF THE OSCARS IT PROBABLY WONT BE

WOMEN INVOLVED IN TIMES UP SAID THAT ALTHOUGH THE GLOBES SIGNIFIED THE
INITIATIVES LAUNCH THEY NEVER INTENDED IT TO BE JUST AN AWARDS SEASON
CAMPAIGN OR ONE THAT BECAME ASSOCIATED ONLY WITH REDCARPET ACTIONS INSTEAD
A SPOKESWOMAN SAID THE GROUP IS WORKING BEHIND CLOSED DOORS AND HAS SINCE
AMASSED  MILLION FOR ITS LEGAL DEFENSE FUND WHICH AFTER THE GLOBES WAS
FLOODED WITH THOUSANDS OF DONATIONS OF  OR LESS FROM PEOPLE IN SOME 
COUNTRIES


NO CALL TO WEAR BLACK GOWNS WENT OUT IN ADVANCE OF THE OSCARS THOUGH THE
MOVEMENT WILL ALMOST CERTAINLY BE REFERENCED BEFORE AND DURING THE CEREMONY 
ESPECIALLY SINCE VOCAL METOO SUPPORTERS LIKE ASHLEY JUDD LAURA DERN AND
NICOLE KIDMAN ARE SCHEDULED PRESENTERS

ANOTHER FEATURE OF THIS SEASON NO ONE REALLY KNOWS WHO IS GOING TO WIN BEST
PICTURE ARGUABLY THIS HAPPENS A LOT OF THE TIME INARGUABLY THE NAILBITER
NARRATIVE ONLY SERVES THE AWARDS HYPE MACHINE BUT OFTEN THE PEOPLE FORECASTING
THE RACE SOCALLED OSCAROLOGISTS CAN MAKE ONLY EDUCATED GUESSES

THE WAY THE ACADEMY TABULATES THE BIG WINNER DOESNT HELP IN EVERY OTHER
CATEGORY THE NOMINEE WITH THE MOST VOTES WINS BUT IN THE BEST PICTURE
CATEGORY VOTERS ARE ASKED TO LIST THEIR TOP MOVIES IN PREFERENTIAL ORDER IF A
MOVIE GETS MORE THAN  PERCENT OF THE FIRSTPLACE VOTES IT WINS WHEN NO
MOVIE MANAGES THAT THE ONE WITH THE FEWEST FIRSTPLACE VOTES IS ELIMINATED AND
ITS VOTES ARE REDISTRIBUTED TO THE MOVIES THAT GARNERED THE ELIMINATED BALLOTS
SECONDPLACE VOTES AND THIS CONTINUES UNTIL A WINNER EMERGES

IT IS ALL TERRIBLY CONFUSING BUT APPARENTLY THE CONSENSUS FAVORITE COMES OUT
AHEAD IN THE END THIS MEANS THAT ENDOFSEASON AWARDS CHATTER INVARIABLY
INVOLVES TORTURED SPECULATION ABOUT WHICH FILM WOULD MOST LIKELY BE VOTERS
SECOND OR THIRD FAVORITE AND THEN EQUALLY TORTURED CONCLUSIONS ABOUT WHICH
FILM MIGHT PREVAIL

IN  IT WAS A TOSSUP BETWEEN BOYHOOD AND THE EVENTUAL WINNER BIRDMAN
IN  WITH LOTS OF EXPERTS BETTING ON THE REVENANT OR THE BIG SHORT THE
PRIZE WENT TO SPOTLIGHT LAST YEAR NEARLY ALL THE FORECASTERS DECLARED LA
LA LAND THE PRESUMPTIVE WINNER AND FOR TWO AND A HALF MINUTES THEY WERE
CORRECT BEFORE AN ENVELOPE SNAFU WAS REVEALED AND THE RIGHTFUL WINNER
MOONLIGHT WAS CROWNED

THIS YEAR AWARDS WATCHERS ARE UNEQUALLY DIVIDED BETWEEN THREE BILLBOARDS
OUTSIDE EBBING MISSOURI THE FAVORITE AND THE SHAPE OF WATER WHICH IS
THE BAGGERS PREDICTION WITH A FEW FORECASTING A HAIL MARY WIN FOR GET OUT

BUT ALL OF THOSE FILMS HAVE HISTORICAL OSCARVOTING PATTERNS AGAINST THEM THE
SHAPE OF WATER HAS  NOMINATIONS MORE THAN ANY OTHER FILM AND WAS ALSO
NAMED THE YEARS BEST BY THE PRODUCERS AND DIRECTORS GUILDS YET IT WAS NOT
NOMINATED FOR A SCREEN ACTORS GUILD AWARD FOR BEST ENSEMBLE AND NO FILM HAS
WON BEST PICTURE WITHOUT PREVIOUSLY LANDING AT LEAST THE ACTORS NOMINATION
SINCE BRAVEHEART IN  THIS YEAR THE BEST ENSEMBLE SAG ENDED UP GOING TO
THREE BILLBOARDS WHICH IS SIGNIFICANT BECAUSE ACTORS MAKE UP THE ACADEMYS
LARGEST BRANCH THAT FILM WHILE DIVISIVE ALSO WON THE BEST DRAMA GOLDEN GLOBE
AND THE BAFTA BUT ITS FILMMAKER MARTIN MCDONAGH WAS NOT NOMINATED FOR BEST
DIRECTOR AND APART FROM ARGO MOVIES THAT LAND BEST PICTURE WITHOUT ALSO
EARNING BEST DIRECTOR NOMINATIONS ARE FEW AND FAR BETWEEN
```


## Lab Task 2: Encryption using Different Ciphers and Modes

Posteriormente, a nossa tarefa foi explorar vários cifradores e modos de cifra. Tendo em conta que a tarefa seguinte incide sobre os modos de cifra, decidimos concentrar-nos exclusivamente em diferentes tipos de cifradores para este exercício.

Entre a variedade de cifradores, os cifradores de fluxo e de bloco destacam-se como particularmente notáveis. Movidos pela curiosidade, aprofundámos a nossa compreensão sobre estes dois tipos.

### Cifradores de Fluxo

Um cifrador de fluxo é um cifrador de chave simétrica onde os símbolos do texto original são combinados com uma sequência de cifra pseudoaleatória, conhecida como uma corrente de chaves. Neste tipo de cifradores, cada símbolo é encriptado individualmente, sendo combinado com o símbolo correspondente da corrente de chaves para formar o símbolo do texto cifrado. Geralmente, cada símbolo é um bit ou um byte, e a operação de combinação utilizada é o XOR (ou exclusivo).

### Cifradores de Bloco

Os cifradores de bloco representam uma categoria de cifradores de chave simétrica que trabalham com conjuntos de bits de tamanho definido, conhecidos como blocos. Neste tipo de cifrador, blocos de bits de tamanho constante são processados, com os tamanhos mais usuais sendo os de 64 bits e 128 bits. Caso o tamanho do texto a ser cifrado não seja um múltiplo de 8, é necessário aplicar um mecanismo de preenchimento adicional.

Embora em termos conceituais os cifradores de bloco partilhem semelhanças com os cifradores de fluxo, a principal diferença está no volume de dados processados simultaneamente. Além disso, há outras distinções menos evidentes entre os dois tipos, que são detalhadas a seguir:

| Cifrador de Fluxo | Cifrador de Bloco |
|-------------------|-------------------|
| Processa um <u>símbolo</u> do texto original de cada vez. | Processa um <u>bloco</u> do texto original de cada vez.|
| As chaves são de uso único. | Uma mesma chave pode ser utilizada várias vezes.|
| Mais rápido do que o cifrador de bloco. | Mais lento do que o cifrador de fluxo. |
| Simples de reverter o texto encriptado. | Complexo de reverter o texto encriptado.|
| Requer menos código para implementação. | Necessita de mais código para implementação. |
| Adequado para implementação em hardware. | Mais apropriado para implementação em software. |

### Experiências com Cifradores

Tendo assimilado as distinções entre os dois tipos predominantes de cifradores, partimos para a fase prática: testar alguns deles. Escolhemos cifrar um arquivo bastante simples que criámos, denominado "message.txt", contendo o texto:

```bash
"teste"
```

O guia forneceu o comando a seguir, e apenas tivemos de substituir o campo do cifrador pelo que pretendíamos utilizar.

Este formato permite uma apresentação clara e estruturada do processo de teste dos cifradores.

Podemos utilizar o seguinte comando openssl enc para encriptar/desencriptar um ficheiro.

 ```bash
 $ openssl enc -ciphertype -e  -in plain.txt -out cipher.bin \
              -K  00112233445566778889aabbccddeeff \
              -iv 0102030405060708

-in <file>
-out <file>
-e
-d
-K/-iv
input file
output file
encrypt
decrypt
key/iv in hex is the next argument
-[pP]          print the iv/key (then exit if -P)
 ```


## Lab Task 3: Encryption Mode – ECB vs. CBC

No decorrer da tarefa, é explorada a encriptação de uma imagem BMP, focalizando nos modos ECB e CBC. Após a cifragem, o desafio inclui a manipulação dos cabeçalhos do ficheiro encriptado para assegurar que seja reconhecido como uma imagem BMP legítima. A utilização de ferramentas como hex bless ou comandos como head e tail facilitam a extração e combinação precisa de partes específicas dos ficheiros. Posteriormente, a visualização da imagem encriptada, por meio de um programa de visualização de imagens, desafia os participantes a extrair informações úteis sobre a imagem original, consolidando assim a compreensão prática dos conceitos de encriptação e os desafios associados à preservação da integridade dos formatos originais.

Também podemos utilizar os seguintes comandos para obter o cabeçalho de p1.bmp, os dados de p2.bmp (do offset 55 até ao fim do ficheiro) e, em seguida, combinar o cabeçalho e os dados num novo ficheiro.

```bash
$ head -c 54 p1.bmp  > header
$ tail -c +55 p2.bmp > body
$ cat header body > new.bmp
```

## Lab Task 4: Padding

Além disso, investigamos o esquema de preenchimento PKCS#5, frequentemente usado em cifras de bloco. Experimentamos com modos que exigem preenchimento e outros que não o necessitam, fornecendo uma compreensão abrangente de quando e por que o preenchimento é aplicado. Ao criar ficheiros de tamanhos específicos, exploramos as implicações do preenchimento nos ficheiros cifrados e utilizamos a opção "-nopad" durante a descriptografia para examinar os dados adicionados. Esse experimento contribui para uma compreensão prática dos desafios relacionados ao preenchimento em cifras de bloco, enfatizando a sua importância na segurança da criptografia.


It should be noted that padding data may not be printable, so you need to use a hex tool to display the content. The following example shows how to display a file in the hex format:

```bash
$ hexdump -C p1.txt
00000000  31 32 33 34 35 36 37 38  39 49 4a 4b 4c 0a   |123456789IJKL.|
$ xxd p1.txt
00000000: 3132 3334 3536 3738 3949 4a4b 4c0a            123456789IJKL.
```

## Lab Task 5: Error Propagation – Corrupted Cipher Text

Os passos incluíram criar um ficheiro de texto com, pelo menos, 1000 bytes e utilizar a cifra AES-128 para cifrar o ficheiro. Em seguida, um único bit no 55º byte do ficheiro cifrado foi corrompido, e o objetivo foi descriptografar o ficheiro corrompido com a chave e o IV corretos. A questão-chave era avaliar quanto de informação poderia ser recuperado ao descriptografar o ficheiro corrompido, considerando diferentes modos de criptografia, como ECB, CBC, CFB e OFB. Este exercício sublinhou a importância de compreender como diferentes modos de criptografia lidam com erros e a propagação desses erros durante a descriptografia.

## Lab Task 6: Initial Vector (IV) and Common Mistakes

### Task 6.1. IV Experiment

Enfatiza-se a exigência fundamental de unicidade nos vetores iniciais (IVs). A instrução é cifrar o mesmo texto com dois IVs distintos e o mesmo IV, permitindo observar a importância de IVs únicos para preservar a segurança criptográfica.

### Task 6.2. Common Mistake: Use the Same IV

Aborda-se a problemática do uso repetido do IV no modo OFB (Output Feedback). A análise é realizada considerando a possibilidade de um atacante decifrar mensagens adicionais ao obter conhecimento sobre uma mensagem cifrada e seu equivalente em texto simples, destacando as vulnerabilidades associadas a essa prática.

```bash
Plaintext  (P1): This is a known message!
Ciphertext (C1): a469b1c502c1cab966965e50425438e1bb1b5f9037a4c159
Plaintext  (P2): (unknown to you)
Ciphertext (C2): bf73bcd3509299d566c35b5d450337e1bb175f903fafc159
```

### Task 6.3. Common Mistake: Use a Predictable IV

Explora-se o cenário onde um IV previsível compromete a segurança do sistema. Considerando um oráculo de cifragem simulado, o objetivo é mostrar como um IV previsível pode permitir a dedução do conteúdo de mensagens cifradas, enfatizando a importância da imprevisibilidade nos IVs.

Podemos obter acesso ao oráculo executando o seguinte comando:

```bash
$ nc 10.9.0.80 3000
Bob’s secret message is either "Yes" or "No", without quotations.
Bob’s ciphertex: 54601f27c6605da997865f62765117ce
The IV used    : d27d724f59a84d9b61c0f2883efa7bbc
Next IV        : d34c739f59a84d9b61c0f2883efa7bbc

Your plaintext : 11223344aabbccdd
Your ciphertext: 05291d3169b2921f08fe34449ddc3611
Next IV        : cd9f1ee659a84d9b61c0f2883efa7bbc
Your plaintext : <your input>
```

### Additional Readings

São fornecidos materiais suplementares para uma compreensão mais avançada sobre as questões relacionadas aos IVs, incluindo leituras adicionais que exploram criptoanálises mais avançadas. Em última análise, o uso apropriado de IVs, garantindo unicidade e imprevisibilidade, é crucial para a robustez da segurança na encriptação.

## Lab Task 7: Programming using the Crypto Library

É para desenvolver um programa para descobrir a chave de encriptação utilizada num cenário AES-128-CBC. São fornecidos um texto simples, um texto cifrado e um IV, e o objetivo é encontrar a palavra-chave inglesa com menos de 16 caracteres, seguida de sinais de libra (#) para completar 128 bits. O programa deve utilizar a biblioteca de criptografia, e os estudantes são desencorajados de simplesmente usar comandos openssl. O desafio é garantir que o tratamento da mensagem seja adequado, especialmente quando armazenada num ficheiro, e garantir que o programa seja capaz de realizar a tarefa sem depender de comandos externos. O código de amostra está disponível para orientação, e os instrutores são incentivados a gerar novos casos para evitar respostas pré-existentes.

- A cifra aes-128-cbc é utilizada para a encriptação.
- A chave utilizada para encriptar este texto simples é uma palavra inglesa com menos de 16 caracteres; a palavra pode ser encontrada num dicionário típico de inglês. Como a palavra tem menos de 16 caracteres (ou seja, 128 bits), os sinais de libra (#: o valor hexadecimal é 0x23) são anexados ao fim da palavra para formar uma chave de 128 bits.


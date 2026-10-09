# Identificacao e compra de reposicao

## Por que este arquivo existe

A wiki da comunidade documenta todas as geracoes de BMCU no mesmo lugar. Isso a torna
confusa e faz gente aplicar procedimento de uma versao em outra. Aqui o alvo e **so o
370C DM**. As outras variantes aparecem apenas como lista de exclusao, para voce
confirmar que a placa na mao nao e uma delas.

## Confirmando que e um 370C DM

Olhe a **sub-placa de um canal** (a plaquinha preta ao lado do motor, ligada por cabo
flat). Num 370C DM voce encontra:

| O que | Onde | Como confirmar |
|---|---|---|
| Dois micro-switches | borda da placa, um no meio e um perto do conector flat | pressione com a unha: **tem que clicar** |
| Chip AS5600 | SOIC de 8 pinos, acima do switch de baixo | o nome `AS5600` esta impresso nele |
| Dois LEDs WS2812 | quadradinhos claros acima do switch do meio | um e status do canal, outro do buffer |
| Conector flat | borda inferior | vai para a placa-mae |

Na **placa-mae**: microcontrolador CH32V203 em encapsulamento QFP, botoes `R` e `B`,
header de gravacao 2x2 ao lado deles, indutor marcado `100`, quatro drivers de motor em
fila, quatro conectores flat bege numerados 1 a 4, um LED WS2812 e o conector da
impressora na ponta.

## Lista de exclusao — se for alguma destas, esta skill nao se aplica

| Variante | Como reconhecer |
|---|---|
| 130 / 180 motor | motor pequeno, mecanismo de embreagem com placa triangular |
| 370 early | deteccao de filamento **optica** (furo com sensor de forquilha) |
| 370-B esfera de aco | esfera de aco no lugar do furo optico, sem micro-switches |
| 370x microswitch (Xingchen) | micro-switch, mas **placa-mae propria com CH340 embutido** |
| 370C Hall sem DM | buffer com ima e sensor Hall, **sem** os dois micro-switches |

O teste do clique separa a maioria dos casos em cinco segundos.

## Variantes de placa-mae

Existem tres, e elas **nao** sao intercambiaveis sem atencao:

| Variante | Diferenca | Como reconhecer |
|---|---|---|
| **Original (oficial)** | sem gravacao embutida | nenhum conector USB; precisa de adaptador CH340 externo |
| **Type-C (oficial)** | chip CH340 embutido | tem porta USB-C; grava sem adaptador |
| **370x do Xingchen** | especifica da modificacao microswitch dele | tambem tem CH340 embutido |

Uma Type-C real recebida como reposicao veio marcada **"BMCU V2.2"**, **sem os botoes `R` e
`B`** (o Flasher entra no bootloader sozinho pelo USB) e com o header de depuracao no verso
marcado `SCLK SWIO ERx ETx Gnd 3.3`, alem do `GND 24V / B A`. Quem procura os botoes para
gravar nao vai achar, e nao precisa.

A wiki diz que a Type-C e "identica a versao padrao" fora o CH340 embutido. Se voce for
trocar uma Original queimada, **a Type-C e o upgrade obvio**: mesma compatibilidade e
acaba com a fiacao solta na hora de gravar.

## Comprando sem errar

**A placa-mae nao tem codigo de versao impresso.** Nem na frente nem no verso — foi
verificado numa unidade real. Nao existe numero para conferir num anuncio. Entao a
identificacao e por impressao digital visual.

Peca ao vendedor **foto dos dois lados** e compare:

**Frente:** quatro conectores flat bege numerados 1 a 4 na mesma borda · botoes `R` e `B`
no canto com o header 2x2 ao lado · indutor marcado `100` · quatro drivers de motor em
fila, cada um com seu resistor de medicao · um WS2812 na ponta · entrada marcada
`+ IN −` · dois furos de parafuso.

**Verso:** bloco `GND 24V / B A` (conector da impressora) · bloco
`IO CLK ETx ERx / Tx Rx V+ GND` (gravacao e SWD) · furos de fixacao nas mesmas posicoes.

Confira tambem que os **furos de fixacao batem com a carcaca impressa** que voce ja tem.

### A conta que vale fazer antes

Compare o preco da **placa avulsa** com o de uma **unidade completa**. Se a placa sozinha
passar de metade do valor da unidade, compensa comprar a unidade inteira e guardar a
antiga como estoque de pecas — sub-placas, motores e mecanica sao exatamente o que
desgasta.

## Upgrades que existem

O 370C e o topo da linha: o mercado vende kit de conversao **370X -> 370C**, ou seja,
o 370C e o destino, nao a origem. Nao ha modelo superior para comprar. O que existe sao
melhorias:

| Upgrade | Ganho | Quando vale |
|---|---|---|
| Cursor e alavanca impressos de reposicao | sobressalente do que desgasta | sempre; custo de filamento |
| Buffer bidirecional | duplo buffer linear; recua o filamento por completo | se travar na juncao durante troca de cor |
| Engrenagem 242a de alto torque | 1,7x o torque; TPU, PETG-CF, ABS sem patinar | so se imprimir esses materiais; exige tambem os quadros de alto torque |
| Mola de alavanca mais forte | fim do estalo e da patinacao ao carregar | se ouvir rangido no carregamento |
| Pecas reforcadas | suportes e tampas mais rigidos | se aparecer folga ou trinca |

O buffer bidirecional e a reconstrucao do corpo dos quatro canais, nao uma peca avulsa.
Os arquivos costumam vir em dois 3MF porque os canais sao espelhados (A-B e C-D).

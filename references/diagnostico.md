# Diagnostico

## Como medir, para quem nunca usou multimetro

As instrucoes de medicao estao **dentro de cada teste**, com a posicao do seletor e o ponto
exato onde encostar. Isto aqui e so o basico comum a todos.

**Preparacao:** ponta **preta** no borne **COM** (o do meio), ponta **vermelha** no borne
marcado **VΩmA** (o da direita).

**Continuidade** — seletor no simbolo de ondas sonoras `•)))`, que costuma dividir posicao
com o simbolo de diodo (um triangulo com uma barra). Serve para saber se dois pontos estao
ligados: apita se estiverem.

**Tensao continua** — seletor no lado marcado `V` com uma barra cheia sobre tracinhos.
Escolha a escala pelo valor esperado: **20** para medir 3,3 V, **200** para medir 24 V.
Usar a escala de 20 para medir 24 V estoura e mostra leitura invalida.

> **Teste de sanidade, sempre.** Em continuidade, encoste as duas pontas uma na outra: tem
> que apitar. Se nao apitar, o seletor ou os bornes estao errados. **Nunca confie numa
> medicao negativa sem esse teste** — "nao apitou" pode significar "aparelho mal
> configurado".

> **Toda medida precisa de um controle.** Valor absoluto sozinho nao prova nada, ainda mais
> em placa montada, onde a corrente do multimetro pode achar caminho por outra trilha. O
> controle e a mesma medida com a variavel removida, ou no canal que funciona.

---

## Canal alimenta e nao para

Sintoma: o filamento chega ao bico, o motor continua empurrando e patinando, e depois de um
tempo desiste sozinho.

Ordem de eliminacao, do mais barato ao mais caro. **Nao pule para troca de peca.**

### 1. Reset do canal (gratis)

Desconecte o canal, reinicie o BMCU, reconecte, reinicie de novo. Zera a auto-deteccao.

### 2. Limpeza (gratis)

Ver `manutencao.md`. Num caso real isto resolveu um dos dois canais defeituosos — o outro
tinha causa diferente. Suje e atrito no curso do cursor impedem o movimento de completar.

### 3. Micro-switches: estao fechando contato?

Placa **desenergizada**, multimetro em **continuidade**.

Cada micro-switch tem quatro peninhas. **As duas do mesmo lado sao ligadas de fabrica e
apitam sempre** — medir nelas nao diz nada, e e o erro classico. Meca nos **lados opostos**:

- Solto: **nao** pode apitar
- Apertado: **tem** que apitar

Se nao achar de primeira, teste as combinacoes ate achar o par que muda de estado.

Resultado: os dois clicam e fecham contato? O switch esta bom, siga. Um clica e nao fecha?
Achou — troca ou ressolda.

### 4. Trocar a sub-placa com a de um canal bom

- Defeito **acompanhou a placa** → e a placa
- Defeito **ficou na posicao** → nao e a placa

### 5. Trocar o modulo inteiro de posicao

Placa, motor, corpo, mecanismo — tudo. Se o defeito **continuar na mesma posicao**, nada do
que existe dentro do modulo explica.

### 6. Regravar o firmware

Chegou aqui, a causa e **estado gravado**. O firmware armazena o ponto de deteccao
**separadamente para cada canal**; calibracao ruim gravada no slot fica presa a posicao e
sobrevive a qualquer troca de hardware.

Gravacao com apagamento total limpa a NVM, e a calibracao no primeiro boot refaz os valores.
Ver `firmware.md`.

> **Caso real:** canais 2 e 4 falhavam. O 4 era sujeira e saiu com limpeza. O 2 sobreviveu a
> troca de sub-placa, de motor e do modulo inteiro — e saiu com regravacao. Duas causas
> diferentes no mesmo sintoma.

## Codigos da impressora com BMCU

| Codigo | Significa | O que fazer |
|---|---|---|
| HMS `05XX-0400-0001-0044` | "AMS nao certificado / firmware nao atualizado". `XX` e o slot: `00`-`03` = AMS A-D, `10` = AMS Lite | **Nada.** Aviso permanente com qualquer firmware de BMCU; volta a cada boot. Na A1 ate 01.07.02 nao afeta a impressao (README do autor). Nenhuma variante de build o remove |
| print_error `0500-409D` (`83902621`) | "firmware do AMS nao corresponde a impressora; nao pode continuar" | **Bloqueia.** Vem da impressora em 01.08+. Voltar para 01.07, ou usar OrcaStudio. Regravar o BMCU nao resolve |
| Falha na atualizacao `301` | a impressora nao conseguiu atualizar/reverter | Codigo nao documentado. Desconecte o BMCU antes (a impressora tenta atualizar os modulos ligados), depois cartao SD com espaco, outra rede, ou iniciar pelo Bambu Studio |

O que suprime o 0044 so na tela do PC e um fatiador modificado (`BambuStudio-BMCU`, do mesmo
autor). O aviso continua na impressora: o fatiador so deixa de mostrar.

Para ler os codigos sem depender da tela: `print_error` e a lista `hms` vem no relatorio
MQTT da impressora (`gcode_state` = `RUNNING` com `print_error` 0 e o sinal de que imprime).

## Tabela de LEDs

> **Atencao a origem:** esta tabela e do manual do firmware do **fabricante** (linha 27,
> placa de duplo micro-gatilho). E a placa certa, mas **pode nao ser o firmware que esta
> rodando**. Na V10.5 com LED RGB desligado, o LED da frente indica estado do buffer —
> posicao neutra, pressao demais, pressao de menos. Use como referencia, nao como lei.

| Cor | Status do canal |
|---|---|
| Verde | alimentando |
| Roxo | puxando de volta |
| Azul | ocioso |
| Branco | canal em uso |
| Verde-claro | filamento detectado pelo tool head e sendo mordido |
| **Laranja / dourado** | **somente o micro-switch externo acionado** |
| Ciano | somente o micro-switch interno acionado |
| Vermelho | falha do AS5600 ou ima ausente — canal travado. Tambem e a cor de sistema offline na placa-mae |
| Apagado | offline e sem filamento |

O status do buffer so mostra cor de material quando ha **pressao normal e os dois
micro-switches acionados ao mesmo tempo**. Travar em laranja significa que so metade da
condicao foi satisfeita.

---

## BMCU nao liga

Nenhuma luz, e a impressora nao reconhece que ele esta conectado.

### Passo 1 — o trivial

Impressora **desligada**: desencaixe e reencaixe o cabo de 4 vias nas duas pontas. Confira
tambem se o Tipo de AMS continua configurado na impressora.

### Passo 2 — a energia chega?

Separa "problema do BMCU" de "problema da impressora". Os pinos estao identificados no
**verso da placa**, no bloco `GND 24V / B A`.

Desconecte o BMCU e deixe so o cabo saindo da impressora, com ela **ligada**. Seletor em
**tensao continua, escala 200**. Preta no pino de terra do conector, vermelha no de 24 V.

| Leitura | Conclusao |
|---|---|
| perto de **24 V** | a energia chega; o problema e do BMCU |
| **0 V** | o problema e da impressora ou do cabo |

Cuidado para nao encostar as duas pontas uma na outra dentro do conector.

### Passo 3 — a placa esta em curto? (placa morta, sem energia)

Faca **antes** de qualquer teste energizado. Multimetro em continuidade:

- Entre `3V3` e `GND`: **nao pode apitar**
- Entre `24V` e `GND`: **nao pode apitar**

Apitou? Ha curto. Olhe a placa contra a luz procurando componente estufado, rachado ou com
marca preta, trilha escurecida, e cheiro de queimado concentrado num ponto.

### Passo 4 — o microcontrolador sobreviveu?

Este teste alimenta a placa pelos **3,3 V do adaptador CH340**, pulando todo o circuito de
24 V. Se o bootloader responde, o cerebro esta vivo e o dano esta antes dele.

Ligue o CH340 no header de gravacao como em `firmware.md`, BMCU **desconectado da
impressora**. Abra o Flasher em modo TTL, clique `Gravar`, e quando pedir o bootloader:
segure `B`, toque `R`, solte `B`.

| Resultado | Conclusao |
|---|---|
| `bootloader detected (ISP active)` | **MCU vivo.** O dano esta no caminho dos 24 V |
| `timeout waiting for cmd=0xa1` | nao respondeu — siga para o passo 5 |

Observe tambem: com o CH340 ligado, **o BMCU acende alguma luz?** Numa placa sadia acende
vermelho (sistema offline, que e correto sem impressora). Luz nenhuma indica que os 3,3 V
nao estao entrando.

### Passo 5 — quanto o trilho de 3,3 V cai (teste A/B)

Este e o teste que fecha o diagnostico de curto. **Meca nos pinos do modulo CH340**, que
sao espacados 2,54 mm e tem nome impresso — nao nas trilhas finas da placa, onde uma ponteira
escorregando fecha curto e mata o que sobrou.

Seletor em **tensao continua, escala 20**.

**Medicao A** — modulo no USB, os quatro fios ligados ao BMCU. Ponta preta no pino `GND` do
modulo, vermelha no `3V3`. Sao os dois pinos das pontas da fileira de cinco. Anote.

**Medicao B** — tire **so o fio do `+`** do header do BMCU, deixando o modulo plugado. Meca
de novo nos mesmos dois pinos. Anote.

| A (ligado) | B (solto) | Conclusao |
|---|---|---|
| ~3,3 V | ~3,3 V | o BMCU nao puxa o trilho. Nao e curto |
| **baixo** (< 2 V) | ~3,3 V | **o BMCU esta em curto no 3,3 V** |
| baixo | baixo | o problema e do modulo, nao do BMCU |

A medicao B e o controle. Sem ela, um numero baixo nao prova nada.

### Passo 6 — isolar o curto

Se deu curto, descubra se e na placa-mae ou numa sub-placa. **Solte os quatro cabos flat**
e repita a medicao A:

| Leitura | Conclusao |
|---|---|
| voltou a ~3,3 V | o curto esta **numa sub-placa** — religue uma por vez para achar qual |
| continua baixo | o curto e **na placa-mae** |

Curto no 3,3 V da placa-mae depois de surto costuma ser o conversor que baixa 24 V para
3,3 V: quando ele morre, joga os 24 V no trilho e leva o microcontrolador junto. Conserto a
nivel de componente exige retrabalho com ar quente.

**A boa noticia nesse cenario:** sub-placas, motores, engrenagens, buffers, molas, tubos e
toda a mecanica ficam **fora** do curto. Perde-se so a placa-mae.

---

## Antes de instalar uma placa de reposicao depois de surto

Nao conecte a placa nova sem validar o que sobrou. O surto pode ter comprometido o caminho
que vai alimenta-la, e a placa nova morre igual.

1. **Cabo de 4 vias, a frio** — com tudo desligado, continuidade ponta a ponta em cada um
   dos quatro condutores, e **entre condutores vizinhos nao pode apitar** (curto no cabo).
2. **Porta da impressora, energizada** — o passo 2 acima: tem que entregar perto de 24 V,
   estavel. Se oscilar ou vier baixo, o problema e da impressora.
3. **Sub-placas** — passo 6 acima, uma a uma, para garantir que nenhuma esta em curto antes
   de pendura-las na placa nova.

So depois dos tres conecte a reposicao.

**E proteja a instalacao:** o BMCU e alimentado pela impressora. Num surto, a impressora
costuma aguentar e o BMCU nao. Um nobreak, ou pelo menos um filtro de linha decente, sai mais
barato que uma placa.

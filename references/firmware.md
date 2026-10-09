# Firmware: gravacao e calibracao

## Qual firmware

| Pacote | Origem | Impressora |
|---|---|---|
| 0020 | fabricante | A1 1.04 / 1.05 |
| 0027 | fabricante (4061N), linha Hall | — |
| 27-5 | fabricante (Stardust), linha duplo micro-switch | A1 1.07 |
| V10.5 | Pawel Jarczak (2026-04) | A1 1.07 |
| **V11** | **Pawel Jarczak (2026-10)** | A1 ate 1.07.02 (ver abaixo) |

Nao afirme "a mais recente" de memoria: confira as releases do repositorio antes. O Flasher
no modo `Online` baixa os binarios do branch `main`, que pode ter correcoes depois da tag —
hash diferente do da tag nao e sinal de arquivo adulterado.

### Firmware da impressora: o que realmente decide se imprime

Desde o firmware 01.05 a A1 exige certificacao do AMS (frames `0x040D` e `0x040E`) e
**nenhum firmware de BMCU passa nela**. O efeito depende da versao da impressora, nao da
do BMCU:

| A1 | Efeito |
|---|---|
| 01.05 a 01.07.02 | imprime normal; fica so o aviso HMS `0500-0400-0001-0044` |
| **01.08 ou maior** | **bloqueia** a impressao com `0500-409D`. Trocar o firmware do BMCU nao resolve |

Na 01.08 a unica saida documentada pelo autor e fatiar e mandar pelo **OrcaStudio** (fork
dele que ignora o erro). A saida limpa e voltar a impressora para 01.07 — testado num caso
real: o bloqueio sumiu e a impressao rodou. Depois disso, **recuse** a oferta de atualizacao
que a impressora faz a cada reinicio. Na impressora o tipo deve ser **AMS**, nao AMS Lite.

Ver `diagnostico.md` para os codigos.

Um detalhe que salva tempo: o prefixo do nome indica a **linha**, nao a versao.
`0027` e a linha Hall do 4061N; `27` e a linha duplo micro-switch do Stardust. `27-5` nao
e "mais novo" que `0027`, e outro ramo.

## O que voce precisa

- Modulo **USB-TTL CH340G** — pinos `5V · 3V3 · TXD · RXD · GND`
- Quatro jumpers dupont femea-femea
- **BMCU Flasher** (<https://github.com/jarczakpawel/BMCU-Flasher>) — executavel Windows,
  Linux, macOS e Android; baixa o firmware sozinho
- Driver CH340 da WCH, se o Windows nao reconhecer o modulo
- Multimetro, para as conferencias

Se a placa-mae for a variante **Type-C**, nada disso e necessario: e so um cabo USB, e o
Flasher cuida de BOOT e RESET sozinho no modo **USB (AutoDI)**. No Windows ela aparece como
`USB-SERIAL CH340` (VID `1A86`, PID `7523`). Essa placa nem tem botoes `R` e `B`. Use o
Flasher **v1.4 ou maior**: a v1.3 nao conhece as opcoes da V11.

## A ligacao

**Mostre `assets/ligacao-gravacao.jpg` ao operador.** E a foto oficial da wiki com os
quatro fios ligados. `assets/legenda-header.jpg` e a ampliacao que mostra o bloco de
legenda. Descrever fiacao em palavras nao funciona.

| BMCU | Modulo CH340G |
|---|---|
| `R` | **TXD** |
| `T` | **RXD** |
| `+` (3V3) | 3V3 |
| `−` (GND) | GND |

`R` e `T` **cruzam**. O `5V` do modulo nao e usado.

### Achando os pinos na placa

O header de gravacao sao **quatro furos metalizados em 2x2**, ao lado dos botoes `R` e `B`.

**Os furos nao sao marcados individualmente.** A marcacao e um bloquinho de legenda 2x2
impresso **ao lado** do header, que mapeia posicao por posicao:

```
+   −
T   R
```

Quem procura nome em cima de cada furo nao acha e conclui que a placa nao tem marcacao.

> **Armadilha:** a letra `R` aparece duas vezes com significados diferentes. O **botao R**
> e o Reset. O **pad R** do header e o RX. Ficam a centimetros um do outro.

### Se a legenda estiver ilegivel

Da para identificar com multimetro, sem adivinhar:

1. Multimetro em **continuidade** — o simbolo de ondas sonoras `•)))`, que costuma dividir
   posicao com o simbolo de diodo. Ponta preta no borne **COM**, vermelha no borne **VΩmA**.
2. **Teste o aparelho primeiro:** encoste as duas pontas uma na outra. Tem que apitar. Sem
   esse teste, uma leitura negativa nao significa nada.
3. Ponta preta no pad `−` **grande da entrada de energia** (aquele marcado `−` no meio da
   placa, bem visivel), vermelha em cada um dos quatro furos. **O que apitar e o GND.**
4. Achado o GND, o `+` e o furo diagonalmente oposto — pela geometria da legenda, `+` e `R`
   ficam em diagonal, e `−` e `T` tambem.

**Só `+` e `−` sao perigosos de errar.** Se `T` e `R` ficarem trocados, nada queima: o
Flasher nao acha a placa, voce inverte os dois fios e repete.

## Conferencias antes de plugar

Com tudo desconectado e sem energia, multimetro em continuidade:

- Entre `3V3` e `GND` da placa: **nao pode apitar**
- Entre `24V` e `GND`: **nao pode apitar**

Se apitar, ha curto — e conectar o modulo pode danificar o modulo e a porta USB do PC.
Os pinos `24V` e `GND` estao identificados no **verso** da placa, no bloco `GND 24V / B A`.

Sobre o jumper `S1` do modulo, que seleciona o nivel de tensao: a documentacao oficial
anota, junto dele, que **os dois niveis gravam normalmente**. Prefira `3V3` por ser o
conservador, mas nao e o risco que parece.

## A sequencia

1. **BMCU desconectado da impressora, impressora desligada.** O BMCU e alimentado pelos
   3,3 V do modulo durante a gravacao.
2. Ligue os quatro fios e plugue o modulo no PC. **Empurre cada jumper ate o fim do furo.**
3. Abra o Flasher, modo **TTL (manual BOOT+RESET)**.
4. `Atualizar` e selecione a porta (aparece como `USB-SERIAL CH340`).
5. Firmware: **`Online`** — baixa a versao mais recente e confere o hash.
6. Clique **`Gravar`**. So depois disso ele fica esperando o bootloader.
7. Quando o log disser `enter bootloader now`: **segure `B`, toque `R`, solte `B`.**

Ele pode pedir para reentrar no bootloader uma segunda vez, ao aplicar os option bytes.
E normal.

### As escolhas, e o porque de cada uma

| Opcao | Escolha | Motivo |
|---|---|---|
| Forca de carga | **Standard (A1)** | `high_force` e para P1S ou PTFE muito curvo; `soft_load` e paliativo para mola de alavanca fraca |
| Autoload | **ligado** | a placa e DM, dois micro-switches; e o modo dela |
| LED RGB | **desligado** | com ele ligado o LED passa a mostrar a cor do filamento e some o instrumento de diagnostico. Ligue so quando nao estiver investigando nada |
| Slot | **AMS_AUTO** (V11) ou `AMS_A` | a V11 removeu o `SOLO`. `AMS_AUTO` e o recomendado; `AMS_A` a `AMS_D` sao o modo legado com endereco fixo. Na V10.5 era `SOLO`. Nenhum dos dois muda o aviso de certificacao |
| Retracao | depende da topologia | ver abaixo |

**Retracao — o erro facil.** O que decide nao e o comprimento do tubo, e **onde fica o
ponto de juncao**:

- **Quatro tubos separados ate a impressora** (topologia do A1): cada filamento so precisa
  recuar ate limpar o divisor que fica **na impressora**. Curto — cerca de 9,5 cm. O
  comprimento dos tubos nao entra na conta.
- **Um tubo so, com as quatro vias juntando no BMCU** (topologia tipica de P1S): o
  filamento precisa recuar o caminho inteiro ate liberar a juncao. Ai sim escolhe-se 60 a
  90 cm.

Escolher errado nao quebra nada: quebra a troca de cor, e regravar leva dois minutos.

### Sinal de sucesso

No log, `verify ok` seguido de `OK total=...`, com a contagem de blocos fechando
(`verify blocks=894/894`). **Nao basta "nao deu erro"** — a conferencia tem que ser
positiva e completa.

## Erros de gravacao e a causa real

| Mensagem | Significa | Correcao |
|---|---|---|
| `ports: 0` | nenhuma porta serial | modo em TTL e clicar `Atualizar`. Se persistir, instalar o driver CH340 |
| `timeout waiting for cmd=0xa1` | o chip nao respondeu ao identificar | voce clicou `Gravar`? Ele so espera o bootloader depois disso. Se sim, contato |
| `timeout waiting for cmd=0xa5` | falhou ao escrever | **contato frouxo nos jumpers** |
| `timeout waiting for cmd=0xa6` | falhou ao conferir | mesma causa; se persistir, desmarque `Verificar` |
| `PermissionError: acesso negado` na porta | porta presa pela tentativa anterior | feche o Flasher inteiro, desplugue e replugue o modulo |

**O padrao que resolve a maioria:** se os comandos curtos funcionam — identificar, ler
config, destravar, apagar — e a falha cai na transferencia dos ~50 KB, **e contato**. Um
jumper macho num furo metalizado, sem carcaca e sem pressao, faz contato bom para um
comando de milissegundos e falha num fluxo continuo. Pressione os quatro fios com o dedo
durante toda a gravacao. Num caso real isso resolveu depois de tres tentativas falhas, sem
mudar mais nada.

Se ainda falhar, marque `Sem fast` — mantem tudo em 115200 e elimina a troca para 1 Mbaud.

### Falhou no meio: nao e tijolo

O apagamento acontece antes da escrita, entao a placa pode ficar sem firmware. Isso
assusta e nao e grave: **o bootloader ISP do CH32V203 fica em memoria de sistema e nao e
apagavel.** Ele continua respondendo a `B`+`R` quantas vezes precisar. Religue e repita.

---

# Calibracao

A documentacao e explicita: **sem calibracao correta o BMCU nao funciona direito.** Nao e
passo opcional.

## Interativa, no primeiro boot (V11)

Depois de uma gravacao com apagamento total, o proximo boot **e** o primeiro boot, e a
calibracao **nao e passiva**: ela espera o operador mexer em cada buffer. Quem so liga e
espera fica olhando LED piscar para sempre.

1. **Tire todo o filamento dos quatro canais**, inclusive o que esta so na entrada.
2. Conecte a impressora com ela desligada, depois ligue.
3. Em cada canal, siga o LED. O buffer se manipula pelo **conector metalico do topo**:
   - **Azul piscando** → empurre o buffer ate o fim, segure cerca de 1 s, solte.
   - **Vermelho piscando** → puxe o buffer ate o fim, solte.

## Como saber que terminou

**Terminou quando os quatro canais ficam com luz fixa, sem nenhum piscar.** Enquanto houver
azul ou vermelho piscando em qualquer canal, aquele canal ainda espera o gesto.

## Recalibracao manual

Com os quatro canais vazios, segure um buffer em posicao por cerca de 5 segundos: ele volta
ao modo de calibracao acima. Nao faca isso com filamento carregado — levantar o buffer a mao
tambem e o gesto de descarregar.

## Depois de carregar: o que muda com AUTOLOAD

O carregamento fica diferente e **isso e o projeto, nao defeito**:

1. Voce empurra o filamento a mao ate acionar o primeiro micro-switch.
2. O BMCU assume e alimenta ate o **segundo** switch — a condicao de "totalmente dentro"
   exige os dois.
3. Depois avanca mais 120 mm para deixar pronto para imprimir.

Durante esse avanco ele monitora o buffer. Se o filamento enrosca, recua, tenta de novo,
ate duas vezes. Na terceira desiste e recua o filamento para fora. **Isso e protecao.**

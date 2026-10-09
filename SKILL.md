---
name: bmcu-370c
description: Use para qualquer coisa envolvendo o BMCU — o trocador de filamento de codigo aberto que substitui o AMS Lite em impressoras Bambu Lab A1 e P1. Cobre identificar qual variante de hardware e a sua, gravar firmware pelo adaptador CH340, calibrar, diagnosticar canal que nao para de alimentar ou que nao carrega, achar curto depois de surto de energia, limpar e lubrificar, e escolher placa de reposicao sem comprar errado. TRIGGERS (PT) - bmcu, bmcu370, 370C, AMS clone, trocador de filamento, canal nao para de alimentar, motor girando em falso, filamento nao trava, gravar firmware do bmcu, CH340, bootloader, calibrar bmcu, buffer, micro-switch, AS5600, sub-placa, bmcu nao liga, placa queimada, curto no 3.3V, qual placa comprar. TRIGGERS (EN) - bmcu, bmcu 370c, ams clone, channel keeps feeding, filament wont stop, flash bmcu firmware, ch340, bootloader, calibrate bmcu, buffer, microswitch, AS5600, bmcu wont power on, shorted board.
---

# BMCU 370C — manutencao

Trocador de filamento de codigo aberto que faz o papel do AMS Lite. Esta skill cobre
**a variante 370C com sub-placa DM (duplo micro-switch)**. O conteudo nasceu de uma
investigacao real, da primeira falha ate a placa-mae queimar num surto.

## A primeira coisa, sempre: confirme a variante

Existem varias geracoes de BMCU e **a maior parte da documentacao publica e de outra
versao que nao a sua**. Seguir o procedimento da variante errada custa horas e pode
levar a trocar peca que nao existe no seu hardware.

Antes de qualquer procedimento, confirme que o que esta na bancada e um 370C DM:

- **Duas peninhas pretas quadradas na sub-placa que CLICAM** quando pressionadas — sao
  micro-switches. Se nao clicam, nao e DM.
- **Chip AS5600** com o nome legivel na sub-placa (encoder magnetico).
- **Dois LEDs** por canal.
- Na placa-mae: botoes **R** e **B**, quatro conectores flat numerados 1 a 4, indutor
  marcado `100`, quatro drivers de motor.

Nao e isso? Pare. Leia `references/identificacao.md`, que lista as outras variantes
justamente para voce descartar.

## Duas regras que vieram de erro caro

**1. Defeito que fica na posicao depois de trocar todo o hardware daquela posicao nao
e hardware.** E estado gravado — firmware, calibracao ou configuracao. Num caso real,
o canal 2 falhava; trocou-se sub-placa, motor e depois o modulo inteiro de posicao, e o
defeito continuou no canal 2. Era calibracao por canal armazenada na NVM. A correcao
foi regravar com apagamento total, nao trocar peca.

**2. Alimentacao manual funciona e carga de impressao falha?** O problema esta na
conversa entre BMCU e impressora — firmware ou calibracao — nao na mecanica do canal.
Sao caminhos diferentes: o comando manual e local, a carga de impressao roda o
protocolo completo com a impressora participando da decisao de parar.

## Como usar esta skill

| Situacao | Leia |
|---|---|
| Nao sei qual variante e, ou preciso comprar reposicao | `references/identificacao.md` |
| Gravar firmware, ou a gravacao falhou | `references/firmware.md` |
| Calibrar, ou os LEDs estao piscando estranho | `references/firmware.md` |
| Canal nao para de alimentar, nao carrega, ou nao liga | `references/diagnostico.md` |
| Impressora mostra HMS `0044`, bloqueia com `0500-409D`, ou falha ao atualizar | `references/diagnostico.md` e `references/firmware.md` |
| Limpeza, lubrificante, pecas de desgaste | `references/manutencao.md` |

**Imagens em `assets/`** — `ligacao-gravacao.jpg` e a foto oficial da fiacao do CH340,
e `legenda-header.jpg` e a ampliacao que mostra o bloco de legenda dos pinos. Mostre
essas imagens ao operador; descrever fiacao em palavras nao funciona.

## O que nunca fazer

Quatro avisos do fabricante. Os tres primeiros queimam placa:

1. Nao conectar nem desconectar fio nenhum com a impressora ligada.
2. Nao tocar na placa energizada — estatica causa curto.
3. **Nao inserir o cabo de conexao da impressora na porta de gravacao.** Sao conectores
   parecidos e a poucos centimetros um do outro. Isso queima a placa-mae.
4. Nao conectar os terminais da impressora invertidos.

E mais um, do proprio gravador: **nunca plugar o USB nem gravar com o BMCU conectado a
impressora.**

## Principio de escrita desta skill

Quando instruir alguem a mexer em alguma coisa, **diga onde a peca fica fisicamente**.
"Meca entre 3,3 V e GND na placa" nao e instrucao — e adivinhacao, porque a pessoa nao
sabe onde esses pontos estao. Dizer "ponta preta no pino `GND` do modulo, o da ponta da
fileira de cinco" e instrucao.

O mesmo vale para medicao: **valor absoluto sozinho nao prova nada**. Toda medida precisa
de um controle — a mesma medida com a variavel removida, ou no canal que funciona.

## Onde estao as fontes oficiais

- Wiki da comunidade: <https://bmcu.wanzii.cn/en/> — montagem, versoes, gravacao.
  Instavel; devolveu erro 502 varias vezes. Nao dependa so dela.
- Firmware em uso (Pawel Jarczak): <https://github.com/jarczakpawel/BMCU-C-PJARCZAK>
- Gravador: <https://github.com/jarczakpawel/BMCU-Flasher>
- Drive do fabricante (publico) com manual, videos de gravacao e firmwares:
  <https://drive.google.com/drive/folders/1hdiSyvxWcIvtZG92uE7YCp_mhSPL9PZN>

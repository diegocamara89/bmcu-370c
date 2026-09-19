# Manutencao

## Limpeza dos canais

Po de filamento acumulado no curso do cursor cria atrito suficiente para o canal nao
completar o movimento. **Num caso real isso sozinho explicou um canal defeituoso**, que
voltou ao normal so com limpeza — sem troca de peca e sem firmware.

1. Desmonte a tampa de tras do canal e retire mola, cursor e haste.
2. Limpe haste e cursor com alcool isopropilico.
3. Limpe tambem a fenda dos micro-switches e a aba do cursor que passa por eles. Use ar
   comprimido e cotonete fino. Nada de metal raspando.

**Criterio de aceitacao** — nao e "parece limpo":

- Montado, o filamento passa pelo cilindro **quase sem resistencia**
- O cursor volta sozinho, **na hora**, quando voce solta

Compare sempre com um canal que funciona. "Parece normal" sozinho nao significa nada.

## Lubrificante

> **Regra acima de todas:** nada de lubrificante no furo por onde o filamento passa. O que
> grudar no filamento vai para o hotend.

| Produto | Veredito | Por que |
|---|---|---|
| WD-40 | **Nao** | Nao e lubrificante, e desengripante. Evapora, deixa filme que **atrai** po, pode atacar plastico |
| Graxa azul | **Nao** | Base de petroleo, espessa e pegajosa. Ima de po |
| Graxa branca de litio | **Depende** | Nao na haste. **Serve** na caixa de engrenagens metalica do motor |
| Silicone spray | **So assim** | Nunca esguichar dentro do conjunto: borrifar num cotonete e passar so na haste |
| **Graxa de silicone** | **O certo** | Transparente, e o que vem de fabrica. Cotonete, filme fino, so na haste |

Por que spray e perigoso aqui: a poucos milimetros da peca que desliza estao **dois
micro-switches abertos, o chip AS5600 e o conector flat**. Spray nao tem endereco. Silicone
em contato eletrico causa falha intermitente, que e dos defeitos mais dificeis de reverter.

**Monte limpo e seco primeiro.** Se o cursor voltar sozinho e na hora so com a limpeza, nao
precisa de lubrificante nenhum. Lubrificar antes de medir tambem apaga a prova: se a peca
esta dura e por isso nao completa o curso, a dureza e o sintoma.

## Pecas que desgastam

Vale imprimir sobressalente e guardar na gaveta, sem instalar. Transforma um conserto de fim
de semana em quinze minutos:

- **Cursor** — a peca que suja e trava. Existe uma revisao otimizada nos pacotes de pecas
  reforcadas
- **Alavanca**
- **Buffer**

## Emaranhado de filamento no retorno

Quando o BMCU recua o filamento numa troca de cor, alguem precisa **recolher a sobra**. Um
suporte de carretel que gira livre nao volta sozinho: a sobra vira alca solta ao lado do
rolo, e na proxima alimentacao ela aperta, escapa pela lateral e prende embaixo de uma volta.

A solucao e um **eixo rotativo com rebobinamento automatico** — mola que gira o rolo de volta
e recolhe a sobra. Um auto-rewind tipico devolve 80 a 90 graus de rotacao, o que num rolo
cheio de 85 mm de raio da cerca de 133 mm de recolhimento.

**Escolha o modelo pelo seu suporte, nao pelo nome.** Existe versao especifica para BMCU-370
e versao para AMS Lite, e elas **nao** sao intercambiaveis — se fossem, nao existiriam as
duas. Antes de imprimir, confirme como os rolos estao apoiados hoje: em suportes encaixados
no proprio BMCU, num rack separado, numa caixa seca, ou soltos na bancada. Se for o ultimo
caso, o suporte e o conserto principal.

Ferragem tipica desses modelos: arruela ondulada e mola helicoidal de torcao, centavos.

**O que imprimir nao resolve:** bobinamento cruzado de fabrica. Se o filamento ja veio preso
embaixo de si mesmo no rolo, nenhum suporte conserta — e o sintoma e travar sempre no mesmo
ponto do rolo, independente do canal.

## Protecao eletrica

O BMCU e alimentado pela impressora. Num surto de energia a impressora costuma aguentar e o
BMCU nao — o conversor de 24 V para 3,3 V morre e leva o microcontrolador junto.

Nobreak, ou pelo menos filtro de linha decente, sai mais barato que uma placa-mae.

## Antes de atualizar o firmware da impressora

**Desconecte o BMCU.** E instrucao do proprio gravador: atualizar a impressora e mais seguro
com o BMCU desconectado.

E pese a decisao: atualizacao de impressora e o mecanismo classico pelo qual a compatibilidade
com BMCU quebra. Se a maquina esta funcionando e voce nao precisa de nada da versao nova, o
ganho e zero e o risco nao. Se for atualizar, confirme antes que o firmware do BMCU em uso
declara suporte a versao de destino.

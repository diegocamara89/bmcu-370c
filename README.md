# bmcu-370c

Skill de manutenção do **BMCU 370C com sub-placa DM (duplo micro-switch)** — o trocador
de filamento de código aberto que substitui o AMS Lite em impressoras Bambu Lab A1 e P1.

Cobre identificação de variante, gravação de firmware pelo adaptador CH340, calibração,
diagnóstico (inclusive elétrico, depois de surto), limpeza e compra de reposição.

## Por que ela existe

A documentação pública do BMCU mistura todas as gerações no mesmo lugar, e seguir o
procedimento da variante errada custa horas. Esta skill vai na direção oposta: documenta
**uma** variante a fundo e lista as outras apenas para você descartar.

O conteúdo veio de uma investigação real, do primeiro sintoma até a placa-mãe queimar num
surto. O que está aqui é o que **não** está na wiki: qual documentação se aplica a qual
hardware, a causa real por trás de erros que parecem de software, e instruções de medição
concretas o bastante para executar sem adivinhar.

## Estrutura

```
SKILL.md                    roteador, identificação rápida, regras de ouro
references/
  identificacao.md          variantes, lista de exclusão, compra de reposição
  firmware.md               gravação, escolhas, erros reais, calibração
  diagnostico.md            escada de eliminação, LEDs, testes elétricos, pós-surto
  manutencao.md             limpeza, lubrificante, desgaste, emaranhado, proteção
assets/
  ligacao-gravacao.jpg      fiação do CH340 (foto da wiki da comunidade)
  legenda-header.jpg        ampliação do bloco de legenda dos pinos
```

## Instalação

Copie a pasta para o diretório de skills do seu agente. No Claude Code:

```
~/.claude/skills/bmcu-370c/
```

## Créditos das imagens

As duas imagens em `assets/` vêm da **wiki da comunidade BMCU** (bmcu.wanzii.cn),
reproduzidas para uso de manutenção com crédito. `legenda-header.jpg` é um recorte
ampliado e rotacionado da mesma foto. Não são obra deste repositório e não estão cobertas
pela licença do texto.

## Fontes

- Wiki da comunidade: https://bmcu.wanzii.cn/en/
- Firmware (Paweł Jarczak): https://github.com/jarczakpawel/BMCU-C-PJARCZAK
- Gravador: https://github.com/jarczakpawel/BMCU-Flasher
- Drive do fabricante: https://drive.google.com/drive/folders/1hdiSyvxWcIvtZG92uE7YCp_mhSPL9PZN

## Aviso

Mexer em eletrônica energizada e gravar firmware podem danificar hardware. Os
procedimentos aqui foram executados numa unidade real, mas a sua pode diferir. Confirme a
variante antes de seguir qualquer passo.

# Instruções para agentes

Este repositório é uma skill de manutenção do BMCU 370C DM.

## Como usar

Comece por `SKILL.md`: ele traz a confirmação de variante, duas regras de diagnóstico e o
roteador para os arquivos de `references/`. Leia só a referência que a tarefa pede — elas
são independentes.

## Regras ao trabalhar neste conteúdo

**Diga onde a peça fica.** Toda instrução que mande mexer em algo precisa dizer a posição
física. "Meça entre 3,3 V e GND na placa" não é instrução; "ponta preta no pino `GND` do
módulo, o da ponta da fileira de cinco" é.

**Toda medição precisa de um controle.** Valor absoluto sozinho não prova nada. O controle
é a mesma medida com a variável removida, ou no canal que funciona.

**Marque o que é incerto.** O conteúdo distingue o que foi verificado do que é dedução. Se
adicionar algo não confirmado, marque como não confirmado em vez de afirmar.

**Escopo é o 370C DM.** Não expanda para outras variantes: cobrir tudo é o que torna a
documentação pública confusa. Outras variantes existem só como lista de exclusão em
`references/identificacao.md`.

**Não duplique a wiki.** Entra aqui o que é específico desta variante, corretivo de onde a
documentação pública erra, ou aprendido na prática e não documentado. Procedimento padrão
vira link.

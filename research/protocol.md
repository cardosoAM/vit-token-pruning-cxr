# Protocolo experimental — rascunho, não executado

## Antes de congelar
Resolver D05–D08 em decisions.md, atualizar revisão de literatura e registrar orçamento de GPU. Não usar o conjunto de teste para decisões.

## Estimando principal proposto
Diferença entre macro-AUROC do modelo podado e do controle, calculada por estudo e promediada entre sementes pareadas. Não confundir essa média de métricas com métrica de ensemble.

Usar uma regra determinística de agregação das imagens frontais do mesmo estudo (proposta: média das probabilidades) e reportar análise por imagem como secundária. Ambas as variantes usam as mesmas imagens.

## Controle e intervenção
Controle: ViT-S/16 sem poda. Intervenção: mesmo backbone e pesos iniciais, com seletor progressivo explicitamente especificado.
Retenções finais candidatas: 0,70; 0,50; 0,35. Registrar quantidades inteiras de tokens por estágio: uma retenção por estágio não equivale à retenção final.
Compartilhar dados, resolução, aumentos, critério de checkpoint, orçamento de busca, precisão e limite de treinamento. Se houver distillation, perdas auxiliares ou treino extra, controlar ou ablar esses fatores.

## Seleção
Piloto apenas em treino/validação. Congelar taxa e arquitetura antes de teste. Escolher candidato de menor custo entre os que atendam ao critério predefinido na validação; em empate, maior macro-AUROC. Se nenhum atender, registrar ausência de candidato e avaliação exploratória sem confirmação da hipótese.

## Inferência
H0: diferença <= -0,01. H1: diferença > -0,01.
NI demonstrada se limite inferior do IC bilateral 95% > -0,01, correspondente a teste unilateral de 2,5%.
Meta conjunta exige também redução >=25% de FLOPs, incluindo seleção e reorganização.
IC atravessando a margem: inconclusivo. IC totalmente abaixo da margem: evidência de perda superior à tolerância.
A margem é computacional e não implica aceitabilidade clínica.

## Incerteza
Cinco sementes de treinamento pareadas planejadas; não constituem prova de poder estatístico.
Bootstrap pareado por paciente, incluindo todos os estudos e mantendo a estrutura multirrótulo. Proposta: 10.000 reamostragens, registrando semente, réplicas sem suporte de classe e estabilidade dos limites.
Apresentar variabilidade entre sementes separadamente. Usar bootstrap hierárquico/crossed de sementes e pacientes como sensibilidade; cinco sementes limitam essa inferência.
Dimensionar precisão pelo piloto/validação, sem alterar margem em função do teste.

## Tempo de inferência
Mesmo hardware, modo avaliação, sem gradientes, precisão e versões fixas. Lotes 1 e 16, quando couberem.
Proposta inicial: 50 aquecimentos e 200 iterações em 5 sessões independentes; confirmar estabilidade no piloto. Sincronizar GPU e alternar ordem de métodos. Medir somente modelo e também pipeline completo separadamente. Reportar mediana, p95, throughput e memória. Sessões, não iterações correlacionadas, sustentam análise entre repetições.

## Condição de entrada
Testes de splits, rótulos, perda mascarada, equivalência com retenção 100%, seleção física de tokens e métricas devem passar antes dos experimentos.

# Relatório técnico-científico: diagnóstico da proposta e planejamento

Data: 16 de novembro de 2026. Autor da proposta: Matheus Cardoso de Araujo.
Natureza deste documento: análise documental e recomendações; nenhum experimento foi executado.
A confirmação do autor de que existe apenas uma proposta substitui a premissa inicial de “protótipo quase funcional”.

## 1. Resumo executivo do diagnóstico

A proposta apresenta problema computacional delimitado, controle principal adequado e hipótese falsificável: comparar ViT-S/16 com e sem poda progressiva na classificação de cinco achados em radiografias, buscando redução de operações e não inferioridade da macro-AUROC. Sua principal qualidade é separar avaliação computacional de validação clínica.

O estágio comprovado é planejamento documental. Não há modelo implementado, pipeline de dados, ambiente de treinamento, testes ou resultados no material disponibilizado. Não é possível certificar maturidade de software, desempenho ou viabilidade de treinamento.

Prioridades: atualizar a lacuna científica; assegurar acesso e recursos; corrigir unidade de análise e inferência; especificar rótulos, resolução e mecanismo de poda; implementar baseline auditável. A contribuição provável é avaliação empírica controlada, não necessariamente um novo algoritmo.

## 2. Caracterização e estágio atual do projeto

Materiais examinados: proposta_pesquisa/main.tex, proposta_pesquisa/references.bib e apresentacao/main.tex. Os PDFs existentes foram preservados como entregas anteriores, sem nova compilação. Diretórios de imagens renderizadas, caches e executável de compilação não constituem protótipo de ML e não foram publicados.

| Aspecto | Evidência / classificação |
|---|---|
| Problema | Verificado no texto: custo do ViT e risco de perder informação ao descartar tokens |
| Objetivo tecnológico | Planejado: classificador com poda e instrumentação |
| Objetivo científico | Verificado como intenção: estimar compromisso custo–desempenho |
| Funcionalidades implementadas | Nenhuma implementação de ML disponibilizada |
| Funcionalidades validadas | Nenhuma evidência fornecida |
| Dados | MIMIC-CXR-JPG escolhido; acesso e download não comprovados |
| Resultados | Ausentes |
| Recursos | Hardware, orçamento, disponibilidade semanal e orientação não informados |
| Histórico | Sem repositório Git local encontrado na inspeção; o novo histórico começa com a organização documental |
| Maturidade | Proposta escrita; não atribuir percentual de conclusão de software |

A documentação já define cinco achados, comparador, métricas, três retenções e cinco sementes. Isso é especificação preliminar, não evidência de execução.

## 3. Análise técnica e arquitetural do protótipo

Não existe protótipo a inspecionar. A arquitetura prevista é imagem -> patches -> ViT-S/16 -> cinco saídas sigmoid, com seleção de tokens em estágios intermediários. A implementação deverá separar ingestão, treinamento, poda, avaliação e benchmark.

| Decisão prevista | Justificativa | Pendência verificável |
|---|---|---|
| Mesmo ViT como controle | Isolar efeito da intervenção | Fixar variante, checkpoint, licença e hash |
| Poda progressiva | Reduzir sequência nas camadas posteriores | Seleção treinável ou heurística? Quais camadas? |
| Retenções finais 70%, 50%, 35% | Explorar curva de compromisso | Definir tokens inteiros por estágio |
| Cinco rótulos | Escopo administrável e multirrótulo | Fixar ordem, codificação e população avaliável |
| Imagens frontais | Reduzir heterogeneidade de vistas | Definir AP/PA e estudos com mais de uma frontal |
| Incerteza mascarada | Evitar imposição arbitrária no treino | Separar ausentes, negativos e incertos |
| Cinco sementes | Observar instabilidade de treinamento | Medir custo; não presumir suficiência estatística |

O componente quadrático da atenção não é o custo completo do ViT: projeções, MLPs, seleção e movimentação de tensores também importam. Zerar tokens sem encurtar a sequência pode não acelerar inferência. Um seletor dependente da imagem com quantidade fixa de tokens é dinâmico no conteúdo, mas não necessariamente no orçamento.

Não adicionar interface clínica, API de serviço, cadastro de pacientes ou implantação nesta etapa: esses componentes não respondem à pergunta central.

## 4. Avaliação do enquadramento científico

| Dimensão | Trabalho | Evidência de conclusão |
|---|---|---|
| Desenvolvimento tecnológico | Dataset loader, baseline, poda, métricas e benchmark | Testes e execução reproduzível |
| Investigação científica | Saber quando a poda preserva desempenho e reduz custo | Perguntas, comparações e justificativa de novidade |
| Validação experimental | Comparações pareadas e incerteza | Resultados rastreáveis e interpretação limitada ao desenho |

O enquadramento recomendado é pesquisa aplicada, quantitativa e experimental. Design Science Research não é necessária apenas porque há um artefato: o objeto principal é testar um efeito sob controle, e não avaliar uma solução organizacional em ciclos de intervenção. Estudo de caso clínico e avaliação com usuários não são necessários para a pergunta atual.

Os objetivos são em grande parte claros, mas “melhor modelo” deve significar uma regra de seleção predefinida, e não o modelo com melhor teste. “Sem perda relevante” depende de justificar a margem.

## 5. Estado da arte e lacunas de pesquisa

A investigação realizada é uma busca focal de fontes primárias, não revisão sistemática exaustiva. Não houve busca autenticada em Scopus/Web of Science, nem extração completa de todos os artigos. Os resultados abaixo não provam ausência de trabalhos equivalentes.

| Trabalho verificável | Método / resultado consultado | Relação e limite |
|---|---|---|
| Rao et al., 2021, DynamicViT [R1] | Seleção progressiva de tokens; relata redução de 31–37% de FLOPs e aumento de throughput superior a 40% com pequena perda em imagens naturais | Referência de implementação; números não transferíveis ao MIMIC |
| Xiao et al., 2023, Delving Into Masked Autoencoders... [R2] | MAE pré-treinado em 266.340 radiografias; ViT competitivo com DenseNet-121 | Sustenta uso de ViT no domínio; não testa a mesma intervenção |
| Rubab et al., 2026, Dyna-ViT [R3] | Poda anterior ao codificador sem parâmetros adicionais | Alternativa de menor complexidade; não confundir com DynamicViT |
| Byun et al., 2026, Adaptive Inference... [R4] | Combina redução de tokens e saída antecipada; resumo relata 71,4% de redução média de FLOPs e perda de 0,1 ponto percentual de acurácia em cinco datasets médicos | Mostra que redução adaptativa em imagens médicas já é investigada; não é evidência para a NI multirrótulo aqui proposta |
| Johnson et al., 2024, MIMIC-CXR-JPG v2.1.0 [R5] | Dataset, rótulos e particionamento documentados | Fonte dos dados e restrições; não valida hipótese de poda |

EViT e ViT original constam da bibliografia fornecida; sua leitura integral continua pendente nesta análise. Também foi localizado “Class-attention pooling and token sparsity based vision transformers for chest X-ray interpretation”, DOI 10.1038/s41598-026-37109-6, mas o texto não ficou acessível na consulta. Tratar como candidato prioritário, sem atribuir resultados não verificados.

Lacuna candidata: falta, ainda a confirmar, de avaliação com mesmo backbone, unidade por estudo, dependência por paciente, cinco achados, política explícita de incerteza, não inferioridade predefinida e custo real de execução. Não alegar “primeira aplicação em radiografias”.

Recomendação: revisão narrativa estruturada com busca rastreável, suficiente para delimitar este experimento. Se houver muitos métodos concorrentes, converter a etapa em mapeamento sistemático. Revisão sistemática como entrega própria aumentaria substancialmente o escopo.

Protocolo de busca proposto: combinações de (“token pruning” OR “token reduction” OR “token merging” OR sparsification) AND (“vision transformer”) AND (“chest x-ray” OR radiograph OR “medical imaging”). Buscar desde 2020 até a data de execução em ACM, IEEE/CVF, PMLR, Springer e índices disponíveis; fazer snowballing de referências e citações. Incluir avaliação empírica com redução de tokens e custo reportado; registrar exclusões, duplicatas, datasets, splits, sementes, baseline, hardware, métrica e acesso ao código. Um segundo leitor revisa estudos centrais; sem ele, declarar limitação. Não chamar a busca atual de sistemática.

## 6. Problema científico, objetivos, perguntas e hipóteses

Objetivo geral recomendado: estimar o efeito da poda dinâmica no compromisso entre desempenho multirrótulo e custo, em condições explicitamente controladas.

| RQ | Pergunta | Hipótese / desfecho |
|---|---|---|
| RQ1 | Há configuração com redução >=25% e NI em macro-AUROC? | Delta > -0,01 e custo <=75% do controle |
| RQ2 | Redução teórica produz ganho real de inferência? | Medir razão de latência e throughput; sem presumir aceleração |
| RQ3 | Seleção informada supera descarte aleatório no mesmo orçamento? | Diferença de desempenho entre mecanismos |
| RQ4 | A conclusão muda por classe, semente ou política de incerteza? | Robustez; análises secundárias exploratórias |

Para RQ1: H0: Delta <= -0,01; H1: Delta > -0,01. A margem é absoluta, não “1% relativo”. É preciso justificá-la por utilidade computacional, variabilidade e discussão com orientação; ela não é limiar clínico estabelecido.

Objetivos específicos: auditar dados; estabelecer baseline; medir curva de retenção; testar comparação principal; explicar custo real e limitações; disponibilizar artefatos autorizados.

Escopo exclui diagnóstico autônomo, segurança clínica, novo estado da arte garantido e generalização para hospitais não avaliados.

## 7. Metodologia científica recomendada

Etapas: revisão -> protocolo preliminar -> acesso/recursos -> pipeline -> baseline -> piloto -> intervenção -> seleção na validação -> congelamento -> teste -> análise.

Variáveis independentes: mecanismo e taxa de retenção. Dependentes: macro-AUROC, macro-AUPRC, métricas por classe e custo. Controles: imagens, splits, pesos iniciais, resolução, aumentos, orçamento de ajuste, critério de checkpoint, hardware e precisão.

Unidade de separação e reamostragem: paciente. Unidade de avaliação proposta: estudo, com média das probabilidades das imagens frontais do estudo. Isso precisa ser adotado antes dos experimentos; a alternativa por imagem deve ser secundária. Sem agregação, estudos com mais imagens ganham maior peso.

Não redividir aleatoriamente imagens. Auditar o split fornecido. Decidir referência de rótulos de avaliação separadamente da política de treinamento. Comparar políticas de treino sobre o mesmo conjunto avaliável quando se deseja isolar esse efeito.

Controlar confusão por distillation, perdas auxiliares ou treinamento extra. Igualar só épocas não garante igualdade de custo nem de otimização; reportar horas GPU e orçamento de busca.

A estimativa principal proposta é a média das diferenças de macro-AUROC entre sementes pareadas, não a métrica de um ensemble. Preservar o pareamento de pacientes, estudos e sementes.

## 8. Plano detalhado de testes e experimentos

### 8.1 Testes técnicos necessários, ainda não executados

| ID | Teste e procedimento | Critério de conclusão |
|---|---|---|
| T01 | Interseção de subject_id entre splits | Interseções vazias |
| T02 | Junções imagem/estudo/paciente e vistas | Sem duplicações silenciosas; exclusões contadas |
| T03 | Casos sintéticos de rótulos 1/0/incerto/ausente | Codificação e máscaras correspondem ao protocolo |
| T04 | BCE mascarada com lote totalmente mascarado | Sem NaN; contribuição nula tratada explicitamente |
| T05 | Cinco logits, ordem de classes e sigmoid | Dimensões, gradientes e rótulos consistentes |
| T06 | Retenção 100% em modo avaliação | Equivalência numérica ao backbone na tolerância da precisão |
| T07 | Poda com imagens distintas e contagem por camada | Sem perda do token especial ou índices inválidos; sequência reduzida |
| T08 | Métricas em exemplos pequenos conhecidos e classe constante | Valor correto ou estado “não estimável”, nunca zero inventado |
| T09 | Checkpoint e retomada curta | Estado/seed/otimizador recuperados dentro de tolerância declarada |
| T10 | Imagem corrompida, caminho ausente e rótulo inválido | Falha explícita/log; nenhuma exclusão silenciosa |
| T11 | Benchmark com sincronização e lote fixo | Inclui seletor, sem gradientes e com aquecimento |
| T12 | Revisão dos arquivos destinados ao Git | Sem credenciais ou dados de acesso controlado |

Não há serviço web implementado; pentest de API e teste de carga de usuários não são prioritários. Escalabilidade pertinente é variar lote/resolução dentro da capacidade real.

### 8.2 Experimentos científicos

Todos os procedimentos abaixo são propostas.

**E01 — Baseline e piloto (RQ1/RQ2).**
Objetivo: estabelecer referência válida e estimar custo/precisão. Dados: treino e validação autorizados; teste não consultado. Controle: ViT sem poda; variáveis: sementes e orçamento fixado. Passos: (1) auditar split; (2) validar pipeline com dados sintéticos; (3) executar treino curto; (4) medir memória e tempo por época; (5) executar treino completo piloto e examinar validação; (6) estimar orçamento e precisão necessária. Uma semente inicial diagnostica viabilidade; cinco são planejadas para comparação final. Métricas: curvas de perda/AUROC, recursos e prevalências. Sucesso operacional: treino estável e rastreável, não uma AUROC inventada. Produto: tabela de ambiente e curvas. Riscos: baseline subotimizado e pré-treino incompatível. Se inviável, revisar escopo antes do teste.

**E02 — Curva de retenção e escolha (RQ1).**
Objetivo: escolher uma configuração sem usar teste. Comparar 100%, 70%, 50% e 35% finais com mesmo treino. Passos: (1) fixar camadas e contagens; (2) aplicar mesma busca; (3) treinar variantes; (4) calcular métricas na validação e FLOPs completos; (5) selecionar pela regra do protocolo; (6) congelar. Planejar cinco sementes por configuração quando viável: 20 treinos finais para quatro configurações, além do piloto e busca. Não multiplicar resultados por sementes para simular pacientes independentes. Gráfico: retenção × macro-AUROC/AUPRC e fronte de Pareto. Risco: seleção favorável por muitos ajustes; registrar todas as tentativas. Nenhum candidato elegível é resultado possível e deve ser registrado.

**E03 — Avaliação confirmatória de NI (RQ1).**
Objetivo: avaliar a configuração congelada contra o controle. Dados: teste mantido sem consulta até esta etapa. Passos: (1) verificar commit e hashes; (2) gerar predições das cinco sementes de cada método; (3) agregar por estudo; (4) calcular média das diferenças; (5) bootstrap pareado por paciente; (6) confrontar IC com margem e custo com meta. Proposta inicial de 10.000 réplicas; verificar estabilidade computacional dos limites. Critério: limite inferior de IC bilateral 95% acima de -0,01 e redução >=25%. Gráfico: diferenças/IC e margem; tabela por classe/semente. Ameaças: pequeno suporte, rótulos ruidosos e múltiplos exames. Resultados possíveis: NI demonstrada; perda além da margem; ou inconclusivo. Um resultado agregado não comprova segurança em cada classe.

**E04 — Eficiência efetiva (RQ2).**
Objetivo: confrontar FLOPs com latência, throughput e memória. Reutilizar modelos de E03. Passos: fixar hardware/precisão/lotes; alternar ordem; aquecer; medir modelo e pipeline completo separadamente; registrar consumo e falhas. Proposta: 50 aquecimentos e 200 iterações por sessão, cinco sessões, lotes 1 e 16 se viáveis. Ajustar aquecimento no piloto, não após observar o melhor resultado. Métricas: mediana e p95 da latência, imagens/s, memória pico e razão podado/controle. Não tratar iterações autocorrelacionadas como experimentos independentes. Gráficos: distribuição por sessão e Pareto. Menos FLOPs sem ganho de tempo significa redução teórica sem aceleração demonstrada no hardware testado.

**E05 — Ablação do seletor (RQ3).**
Objetivo: isolar utilidade da escolha de tokens. Baselines: poda aleatória com mesmo número de tokens e, se viável, seleção fixa. Passos: manter backbone, orçamento e treinamento; variar somente critério; executar cinco sementes pareadas; analisar diferenças e IC. O comparador aleatório também deve ser treinado sob a política declarada; removê-los só no teste pode criar desvantagem artificial. Tabela: qualidade/custo dos mecanismos. Se não houver diferença precisa, não atribuir ganho à inteligência do seletor. Opcionalmente ablar distillation quando utilizada.

**E06 — Sensibilidade e heterogeneidade (RQ4).**
Objetivo: avaliar dependência da conclusão em relação a políticas e classes. Comparar treino com máscara, U-Zero e U-One usando referência de avaliação comum. Separar isso de uma análise que altera a própria referência. Repetições desejadas: cinco sementes por política nos modelos principais; se orçamento insuficiente, declarar exploratório e limitar escopo antes do teste. Reportar por classe; estratos demográficos só se dados e uso forem autorizados e houver suporte. Usar IC, contagens e evitar alegação de equidade clínica. Gráficos: forest plots por classe e política. Uma conclusão sensível à codificação enfraquece a generalidade da hipótese.

Conjunto mínimo defensável: E01–E04, com documentação das ameaças. E05 torna-se necessário para atribuir contribuição ao mecanismo de seleção. E06 é recomendável; validação externa é extensão.

## 9. Métricas e estratégias de análise dos dados

Macro-AUROC: média não ponderada das cinco AUROCs; adequada ao endpoint delimitado, mas pode ocultar perdas por classe. Macro-AUPRC: complementar para classes raras; explicitar se usa average precision ou integração trapezoidal e manter convenção. Prevalência deve acompanhar PR.

F1, precisão e recall exigem limiar escolhido na validação; não são endpoints principais automáticos. Acurácia global não resume adequadamente cinco rótulos desbalanceados. Calibração é opcional para extensão orientada a probabilidades.

Redução de FLOPs = 1 - FLOPs_poda/FLOPs_controle. Declarar convenção de contagem, resolução, custos do seletor e operações não suportadas pelo contador. Aceleração = tempo_controle/tempo_poda. Vazão = imagens processadas/tempo medido; não inferir vazão de lote 16 a partir da latência de lote 1.

Bootstrap por paciente: sortear pacientes com reposição e carregar seus estudos; preservar predições de todos os métodos/seed e correlação multirrótulo. Classes sem positivos/negativos podem impedir AUROC. Registrar frequência de réplicas inválidas e insuficiência de suporte; não descartar silenciosamente nem mudar endpoint após resultados.

Cinco seeds não garantem poder. Avaliar largura de IC e probabilidade de concluir NI em simulações baseadas em validação, com várias diferenças plausíveis. Mais réplicas bootstrap diminuem erro Monte Carlo, não criam dados clínicos independentes. Se o teste fixo for impreciso, aceitar conclusão inconclusiva.

A média entre sementes estima o procedimento de treinamento; apresentar variação entre sementes e sensibilidade que incorpore ambas as fontes de incerteza. IC condicionado aos modelos treinados deve ser identificado como tal. Holm é apropriado para família explicitamente definida de testes secundários; não transforma IC comum em IC simultâneo. Endpoint primário único e candidato congelado reduzem multiplicidade.

## 10. Lacunas, riscos e ameaças à validade

| Validade | Risco | Mitigação |
|---|---|---|
| Interna | Baseline pior otimizado ou distillation só na poda | Orçamentos transparentes e ablações |
| Interna | Pacientes sobrepostos | Auditoria automática de splits |
| Construto | Rótulo extraído de laudo tratado como diagnóstico | Delimitar classificação de achados |
| Construto | FLOPs tratados como velocidade | Benchmark real separado |
| Conclusão | Reamostragem por estudo com pacientes repetidos | Bootstrap por paciente |
| Conclusão | Ausência de NI chamada de inferioridade | Interpretação de IC em três regiões |
| Externa | Um hospital e um hardware | Limitar domínio das conclusões |
| Externa | Apenas cinco classes e frontais | Declarar população efetivamente avaliada |
| Operacional | Credenciamento/GPU indisponível | Marco de viabilidade antes de treinos caros |
| Científica | Literatura concorrente já responde à pergunta | Revisão atualizada e contribuição mais específica |

Não há evidência de defeitos de código porque código não foi disponibilizado. As deficiências constatadas são documentais/metodológicas.

## 11. Plano de evolução técnica e científica

Obrigatório: acesso e decisão institucional; revisão do gap; protocolo; auditoria de dados; baseline; implementação verificável da intervenção; avaliação congelada; relato de incerteza e limitações.

Recomendado: ablação, sensibilidade de rótulos, revisão de scripts por outra pessoa e registro público do protocolo quando os direitos permitirem.

Opcional: segundo hospital/dataset, segundo hardware, análise de resolução, DenseNet-121 contextual. Esses itens não devem impedir o núcleo.

Ferramentas propostas: Git para versões; framework de ML escolhido conforme implementação de referência e hardware; configurações serializadas; scripts de métricas e profiler. Fixar versões após piloto, evitando inventar ambiente não executado. Não há necessidade demonstrada de plataforma MLOps complexa.

## 12. Roadmap priorizado com dependências

| Ordem | Atividade | Por que / problema resolvido | Dependência | Entrega e aceite |
|---|---|---|---|---|
| 1 | Revisão dos concorrentes | Evitar novidade falsa | nenhuma | Matriz com fonte e método |
| 2 | Inventário de recursos e acesso | Viabilizar execução | nenhuma | GPU, disco, horas e autorizações |
| 3 | Fechar protocolo | Evitar decisões pós-teste | 1–2 | Endpoints, margem, unidades e rótulos |
| 4 | Pipeline auditável | Impedir leakage e joins incorretos | 3 e dados | Manifesto local e T01–T04 |
| 5 | Baseline | Referência justa | 4 | T05/T08–T10 e E01 |
| 6 | Poda e instrumentação | Intervenção real | 5 | T06/T07/T11 |
| 7 | Seleção e congelamento | Proteger teste | 6 | E02 e commit |
| 8 | Experimentos finais | Produzir evidência | 7 | E03–E05 e logs protegidos |
| 9 | Sensibilidade e análise | Delimitar conclusão | 8 | E06, tabelas e IC |
| 10 | Relato e reprodução | Tornar contribuição verificável | 9 | Relatório, scripts e checklist |

Cada tarefa deve virar issue com RQ, procedimento, recursos e critério de aceite. A quantidade de commits não mede contribuição científica.

## 13. Cronograma de execução

Estimativa proposta de 24 semanas com 10–12 horas semanais, orientação e GPU disponíveis; não é compromisso de prazo.

| Fase | Semanas | Entregáveis / conclusão |
|---|---|---|
| Diagnóstico e fundamentação | 1–3 | Revisão e protocolo preliminar |
| Consolidação do futuro protótipo | 4–8 | Baseline e testes críticos |
| Preparação experimental | 9–11 | Piloto, poda, orçamento e protocolo congelável |
| Execução | 12–17 | Seleção, congelamento e resultados finais |
| Análise científica | 18–20 | Estimativas, IC, ablação e limites |
| Consolidação/divulgação | 21–24 | Relatório e artefatos reproduzíveis |

Cronograma de treinamento deve ser calculado: soma de número de execuções × épocas × tempo por época, mais busca, validação e reexecuções justificadas. Medir no E01. Não assumir GPU gratuita sempre disponível.

Caminho crítico: acesso -> dados auditados -> baseline -> poda -> seleção -> teste -> análise. Revisão e documentação podem ocorrer paralelamente. Atraso no acesso não autoriza substituição silenciosa do dataset.

## 14. Boas práticas de documentação e reprodutibilidade

Software: instalação testada, arquitetura, interfaces, testes, dependências e comandos.
Protocolo: hipóteses, unidade, seleção, exclusões, endpoints e inferência.
Relatório: o que foi executado, desvios, resultados, incerteza e limites.

Para cada execução registrar run_id, commit, configuração, checksum do checkpoint inicial, versões, GPU/CPU/driver, precisão, sementes, dataset/split, transformações, duração, falhas e hashes de saídas. Guardar resultados brutos e predições em ambiente autorizado. Publicar apenas agregados e artefatos permitidos. Manter scripts que reconstruam tabelas.

O repositório agora organiza os documentos originais, protocolo em elaboração, roadmap, decisões, templates e regras de dados. Não há badge de testes aprovados, release de software validado ou DOI inventado.

### Regras institucionais versus recomendações
O CNPq possui programas institucionais e regulamentação própria; as fontes consultadas incluem a Portaria 2.634/2026, que altera a Portaria 2.539/2025 [R6]. O edital específico, termo de concessão e orientações da instituição devem definir obrigações e prazos. Não foi fornecido edital do projeto; não presumir vínculo, bolsa ou cadastro.

Cinco sementes, bootstrap, margem 0,01, GitHub e roadmap de 24 semanas são recomendações/escolhas metodológicas deste projeto, não exigências oficiais identificadas do CNPq. Tampouco presumir regras universais de CAPES/FAPEMIG sem o instrumento aplicável.

A fonte oficial de saúde consultada descreve atualmente o Sinep, com CEPs e Inaep [R7]. Assim, a referência genérica ao sistema CEP/CONEP deve ser atualizada conforme orientação institucional vigente. Não há aprovação ou dispensa documentada. Verificar enquadramento de uso secundário, LGPD, Lei 14.874/2024 e Decreto 12.651/2025 com a instituição; desidentificação não autoriza automaticamente qualquer uso ou transferência.

## 15. Resultados esperados e possibilidades de publicação

Contribuições realistas: curva de compromisso para protocolo específico; evidência favorável, negativa ou inconclusiva de NI; diferença entre economia teórica e real; ablação do seletor; pacote de execução autorizado.

| Alegação futura | Evidência necessária |
|---|---|
| Poda preserva desempenho dentro da margem | E03 e IC adequado |
| Poda acelera inferência | E04 no hardware declarado |
| Seleção é melhor que descarte arbitrário | E05 |
| Resultado é robusto à incerteza | E06 com referência controlada |
| Generaliza a outros hospitais | Dataset externo, atualmente fora do núcleo |

Relatório de IC e apresentação institucional são saídas plausíveis. Workshops de iniciação científica, eventos de visão computacional e informática em saúde podem ser adequados; MIDL é exemplo de veículo com trabalhos diretamente relacionados [R4]. A adequação de trilha, prazo, anonimização e formato exige consulta à chamada no momento de submissão. Não há garantia de aceitação.

Para artigo, é necessário ir além de executar uma técnica conhecida: comparação justa, ablação, inferência correta e recorte que dialogue com concorrentes. TCC pode ampliar validação externa ou hardware; evitar acumular extensões sem concluir o núcleo.

## 16. Avaliação crítica final do orientador

1. Problema científico definido? Sim, no plano documental; falta operacionalização completa.
2. Originalidade? Potencial empírico específico, ainda não comprovado por revisão abrangente.
3. Implementação adequada? Não avaliável: inexiste implementação fornecida.
4. Deficiências metodológicas? Unidade de análise, política de ausentes, seleção, margem e orçamento não completamente definidos.
5. Maiores riscos? Leakage, dependência por paciente, baseline fraco e interpretação indevida de NI.
6. Trabalho sem contribuição direta? Interface clínica e implantação seriam distrações; não foram encontrados como implementados.
7. Prioridade imediata? Revisão focal, viabilidade e protocolo antes de treinos.
8. Alterações antes dos experimentos? Corrigir reamostragem, três interpretações, definir labels e freeze.
9. Experimentos mínimos? E01–E04; E05 para alegar mérito da seleção; E06 recomendado.
10. Caminho eficiente? Uma comparação principal forte, com recursos medidos, antes de expandir métodos e datasets.

Parecer: proposta promissora como estudo experimental de Computação, porém ainda não pronta para executar avaliação confirmatória. Aprovar início de preparação condicionado a protocolo e viabilidade, não tratar proposta como pesquisa concluída.

## 17. Plano de ação imediato

1. Ler integralmente os concorrentes de 2026 e atualizar a matriz de literatura.
2. Reunir com orientação para validar contribuição e margem, sem alegação clínica.
3. Registrar GPU, memória, armazenamento, horas disponíveis e acesso ao PhysioNet.
4. Obter orientação institucional sobre ética e tratamento de dados.
5. Definir unidade por estudo, agrupamento por paciente e políticas de rótulos.
6. Escolher checkpoint, resolução, seletor e camadas; documentar custos auxiliares.
7. Implementar testes sintéticos e auditoria do dataset.
8. Implementar e medir o baseline; estimar orçamento real.
9. Revisar cronograma com medições e congelar o protocolo antes do teste.
10. Executar comparação principal e relatar inclusive resultados negativos ou inconclusivos.

## Referências verificadas e limites de uso

[R1] Rao, Y.; Zhao, W.; Liu, B.; Lu, J.; Zhou, J.; Hsieh, C.-J. DynamicViT: Efficient Vision Transformers with Dynamic Token Sparsification. NeurIPS, 2021.
https://proceedings.neurips.cc/paper_files/paper/2021/hash/747d3443e319a22747fbb873e8b2f9f2-Abstract.html

[R2] Xiao, J.; Bai, Y.; Yuille, A.; Zhou, Z. Delving Into Masked Autoencoders for Multi-Label Thorax Disease Classification. WACV, 2023, 3588–3600.
https://openaccess.thecvf.com/content/WACV2023/html/Xiao_Delving_Into_Masked_Autoencoders_for_Multi-Label_Thorax_Disease_Classification_WACV_2023_paper.html

[R3] Rubab, S. F.; Ghaffar, A. A.; Gul, M. J. J.; Murtala, S.; Lee, I.; Choi, G. S. Dyna-ViT: Parameter-Free Pre-Encoder Token Pruning for Efficient Vision Transformers. CVPR Findings, 2026, 2844–2851.
https://openaccess.thecvf.com/content/CVPR2026F/html/Rubab_Dyna-ViT_Parameter-Free_Pre-Encoder_Token_Pruning_for_Efficient_Vision_Transformers_CVPRF_2026_paper.html

[R4] Byun, J. Y.; Lee, H.; Shuff, J.; Venkatesh, R.; Shekhawat, N. S.; Parikh, K. S.; Chellappa, R. Adaptive Inference for Medical Vision Transformers: Token Reduction or Early Exit? MIDL, PMLR 315, 2026, 2171–2191. Resumo e metadados consultados; resultados próprios não reproduzidos.
https://proceedings.mlr.press/v315/byun26b.html

[R5] Johnson, A. et al. MIMIC-CXR-JPG — chest radiographs with structured labels, v2.1.0. PhysioNet, 2024.
https://doi.org/10.13026/jsn5-t979
https://physionet.org/content/mimic-cxr-jpg/2.1.0/

[R6] CNPq. Portaria 2.634/2026, alteração de critérios dos programas institucionais.
https://portal-adm.cnpq.br/web/guest/view/-/journal_content/56_INSTANCE_0oED/10157/23080702
Contexto institucional: https://www.gov.br/cnpq/pt-br/acesso-a-informacao/acoes-e-programas/programas/programas-ict

[R7] Ministério da Saúde. Instância Nacional de Ética em Pesquisa.
https://www.gov.br/saude/pt-br/composicao/orgaos-colegiados/inaep
Decreto 12.651/2025: https://legis.senado.gov.br/norma/41536927/publicacao/41544486

[R8] Piaggio et al. Reporting of Noninferiority and Equivalence Randomized Trials: Extension of the CONSORT 2010 Statement. JAMA, 2012.
https://jamanetwork.com/journals/jama/fullarticle/1487502
Referência para lógica de margens e inferência; este projeto não é ensaio clínico e não adota CONSORT como obrigação.

[R9] ACM SIGOPS. Artifact evaluation, present and future, 2022.
https://www.sigops.org/2022/artifact-evaluation-present-and-future/
Referência para disponibilização e avaliação de artefatos; não implica certificação deste repositório.

Os capítulos de Wazlawick constam como referência na proposta original, mas não foram disponibilizados para nova conferência integral. Não atribuir passagens ou exigências específicas sem consultar a edição.

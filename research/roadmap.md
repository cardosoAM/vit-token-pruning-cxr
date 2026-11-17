# Roadmap priorizado

Estimativa condicional: 24 semanas, 10–12 horas semanais do estudante, orientação regular e acesso a GPU. Aprovações e credenciamento têm duração externa não garantida.

| Prioridade | Atividade | Tipo | Depende de | Entrega / conclusão |
|---|---|---|---|---|
| P0 | Revisar literatura diretamente concorrente | Científica | nenhuma | matriz de trabalhos e lacuna delimitada |
| P0 | Verificar GPU, armazenamento e acesso aos dados | Técnica/institucional | nenhuma | orçamento e permissões documentados |
| P0 | Resolver unidade, rótulos e margem | Científica | revisão | protocolo revisado pelo orientador |
| P1 | Implementar manifestos e auditoria de splits | Técnica | acesso | zero pacientes compartilhados e contagens auditadas |
| P1 | Implementar baseline e testes críticos | Técnica | protocolo, dados | execução curta reproduzível com dados sintéticos e autorizados |
| P1 | Medir piloto e orçamento | Experimental | baseline | horas/época, memória, viabilidade e precisão estimada |
| P1 | Implementar poda e teste 100% | Técnica | baseline | equivalência e redução física de tokens verificadas |
| P2 | Comparar retenções em validação | Experimental | poda | configuração escolhida sem uso do teste |
| P2 | Congelar protocolo e configuração | Científica | seleção | commit de protocolo e registro das decisões |
| P2 | Treinar sementes e avaliar teste | Experimental | congelamento | métricas, predições protegidas e logs |
| P2 | Ablar seleção e medir custo real | Experimental | modelos | comparação sob mesmo orçamento de tokens |
| P3 | Analisar e redigir | Científica | evidências | relatório com IC, limitações e resultados negativos |
| Opcional | Outro dataset, hardware ou resolução | Experimental | núcleo concluído | análise externa independente |

## Fases
1. Diagnóstico e fundamentação: semanas 1–3.
2. Baseline e consolidação: semanas 4–8.
3. Preparação, piloto e protocolo: semanas 9–11.
4. Experimentos: semanas 12–17.
5. Análise: semanas 18–20.
6. Redação e pacote reproduzível: semanas 21–24.

Caminho crítico: acesso e protocolo -> dados auditados -> baseline -> poda -> seleção/congelamento -> teste -> análise.
Revisão bibliográfica, documentação e procedimentos institucionais podem avançar em paralelo.

Se o piloto inviabilizar a execução completa, reduzir escopo antes do teste e registrar emenda: uma taxa principal, menor busca e ablações limitadas. Reduzir sementes enfraquece análise de estabilidade e deve ser declarado.

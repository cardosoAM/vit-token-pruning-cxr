# Poda dinâmica de tokens em Vision Transformers para radiografias de tórax

**Estado: proposta de pesquisa; sem protótipo implementado e sem resultados experimentais.**

Autor: Matheus Cardoso de Araujo. Origem: atividade de BCC502 — Metodologia Científica para Ciência da Computação.

## Pergunta central
Em condições controladas, a poda dinâmica de tokens em um ViT-S/16 reduz o custo computacional da classificação de cinco achados no MIMIC-CXR-JPG sem perda relevante de desempenho?

## Materiais
- [Proposta original em PDF](docs/pdf/proposta.pdf)
- [Apresentação original em PDF](docs/pdf/apresentacao.pdf)
- [Fontes LaTeX e bibliografia](docs/proposta/)
- [Diagnóstico e planejamento científico](docs/relatorio.md)
- [Protocolo experimental em elaboração](research/protocol.md)
- [Roadmap](research/roadmap.md)
- [Política de dados](data/README.md)
- [Como contribuir](CONTRIBUTING.md)

Os arquivos originais foram preservados byte a byte. Correções recomendadas estão em documentos separados e ainda precisam ser incorporadas à proposta sob revisão do autor/orientador.

## Hipótese proposta, ainda não testada
Redução de pelo menos 25% nas operações por imagem e não inferioridade da macro-AUROC, com margem absoluta de 0,01. A margem requer justificativa anterior à avaliação final. Não demonstrar não inferioridade não equivale a demonstrar inferioridade.

## Escopo
ViT-S/16; MIMIC-CXR-JPG v2.1.0; imagens frontais; atelectasia, cardiomegalia, consolidação, edema e derrame pleural. São escolhas documentais propostas, não configurações executadas.

## Reprodução
Não existem comandos de treinamento, dependências de ML fixadas ou checkpoints neste estágio. Os PDFs existentes foram importados; não foram recompilados nesta organização.
Para recompilar os documentos, utilizar uma instalação LaTeX com os pacotes declarados nos fontes; executar latexmk -pdf main.tex dentro de cada diretório correspondente. Esse procedimento ainda precisa ser validado em ambiente limpo.

## Dados e uso
Não enviar imagens, laudos, identificadores de pacientes, predições individuais, credenciais ou pesos de distribuição não autorizada. O MIMIC-CXR-JPG possui acesso controlado; disponibilidade de uma página pública não significa autorização de redistribuição.
Esta pesquisa não constitui validação clínica ou sistema para uso assistencial.

## Licenciamento e autoria
A licença de redistribuição ainda será definida pelo autor, considerando eventuais direitos institucionais. Nenhuma licença aberta é presumida. Preservar atribuições e licenças de dependências quando o código for implementado.

## Próxima entrega
Fechar protocolo e recursos; implementar carregamento auditável dos dados e baseline sem poda; somente depois implementar a intervenção.

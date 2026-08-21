# Guia do Projeto

## Problema
Transformar um acervo corporativo heterogêneo em uma Wiki Inteligente consultável em linguagem natural usando somente AWS.

## Acervo conhecido pelo desafio
- `raw/ata_reuniao_vendas_sa.pdf`: PDF digital, 5 páginas, camada de texto, sem OCR.
- `raw/ata_resultados_vendas_novos_dados.png`: imagem digitalizada, requer OCR.
- `raw/vendas_sa_dados_ficticios_laboratorio.csv`: 240 oportunidades, 19 colunas.

A Task 01 deve validar o conteúdo real.

## Estratégia
### Originais
Amazon S3 como fonte canônica, com acesso público bloqueado, criptografia, versionamento e rastreabilidade.

### Roteamento
Evento de ingestão → classificação → AWS Step Functions:
- PDF digital → extração de texto;
- PNG → Amazon Textract;
- CSV → fluxo estruturado.

### Processados
Guardar conteúdo normalizado em S3 sem alterar `raw/`.

### Metadados
Considerar:
- document_id;
- source_file/source_uri;
- tipo;
- data;
- participantes;
- tema;
- projeto;
- decisões;
- responsáveis;
- prazos;
- riscos;
- pendências;
- confidencialidade;
- confidence;
- timestamps.

### Documentos
Bedrock Knowledge Bases + embeddings + vector store AWS + RAG.

### CSV
Glue Data Catalog + Athena, preservando as colunas.
Perguntas analíticas devem preferir SQL, não LLM fazendo contas.

### Comportamento sem evidência
A Wiki deve informar insuficiência de evidência e não inventar fatos.

## Portfólio
Entrega forte:
- resposta.md completo;
- README profissional;
- diagrama;
- custos;
- segurança;
- riscos;
- exemplos;
- evidências reais caso exista POC.

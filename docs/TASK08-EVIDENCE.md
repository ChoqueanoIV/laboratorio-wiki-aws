# Evidência sanitizada — Task 08

Data: 21/08/2026  
Região: `us-east-1`

Este registro contém somente fatos observados e omite account ID, ARNs completos, access key, segredo, token e credenciais. Os recursos reais permanecem ativos e inventariados em `docs/AWS-RESOURCES.md` para a Task 09.

## Preservação e integridade

- `ata_reuniao_vendas_sa.pdf`: SHA-256 `E785FC07E682EDBB959004076DDF6B7B28E56F68029ECD41734B4C8D5CF61D59`.
- `ata_resultados_vendas_novos_dados.png`: SHA-256 `B4628DF37CC5495580879A43C34D4B940904845A586406D458D7AE8647A3DB9F`.
- `vendas_sa_dados_ficticios_laboratorio.csv`: SHA-256 `9B6C40CD9A8383C193D1F5E8B93EEB4FA68D7431ABA95808F5786AEA03671A5B`.
- checksums locais foram reconfirmados após a POC e permaneceram idênticos;
- cópias S3 foram enviadas com checksum SHA-256 verificado e versionamento ativo.

## Textract

- uma chamada `AnalyzeDocument` com feature `TABLES` sobre o PNG de uma página;
- 2 blocos `TABLE`, 70 `LINE` e 340 `WORD`;
- 0 blocos marcados `HANDWRITING`;
- consequência: as anotações manuscritas observadas visualmente exigem revisão humana e não podem ser afirmadas a partir dessa saída OCR.

## Knowledge Base e vetores

- PDF extraído diretamente, sem OCR;
- PNG normalizado a partir das linhas reais do Textract;
- CSV preservado, mas não indexado;
- ingestão: 2 documentos escaneados, 2 novos indexados, 0 modificados e 0 falhas;
- índice S3 Vectors: 13 vetores retornados, sem paginação adicional.

## Recuperação e RAG

- tentativa 1 de `Retrieve`: três resultados, todos com fonte; os melhores chunks traziam resumo/identificação, não a lista detalhada de decisões;
- tentativa 2: a resposta chegou ao cliente, mas a AWS CLI falhou ao renderizar um caractere Unicode; foi contabilizada e não repetida;
- tentativa 3: a consulta sobre “ação prioritária” recuperou trechos próximos de ações/prazos, mas não a anotação nem uma associação confiável;
- geração 1: Nova Micro informou corretamente o total de cinco decisões, mas não as enumerou por falta de chunks detalhados;
- geração 2: declarou evidência insuficiente para associar prazo a “ação prioritária”, mas acrescentou interpretação genérica não sustentada e citou o PDF, não o PNG;
- conclusão: a infraestrutura funciona, porém recuperação, chunking, OCR de handwriting e validação de citações precisam de evolução antes de uso confiável.

## Custos e limites

- limite executado: 1 Textract, 1 ingestão, 3 tentativas Retrieve e 2 gerações Nova Micro;
- nenhum recurso provisionado contínuo foi criado;
- custo observado ainda não disponível devido ao atraso normal de faturamento; não foi registrado como zero.

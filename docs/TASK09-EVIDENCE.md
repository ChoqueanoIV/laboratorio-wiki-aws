# Evidências, custos e cleanup — Task 09

Data: 21/08/2026  
Região da POC: `us-east-1`

Este documento registra somente fatos observados. Account ID, ARNs completos, access key, segredo e tokens foram omitidos.

## Evidências visuais

### Antes do cleanup

- Knowledge Base real ativa: [`task09-kb-active.jpg`](evidence/task09-kb-active.jpg).
- Dois buckets S3 reais da POC: [`task09-s3-before-cleanup.jpg`](evidence/task09-s3-before-cleanup.jpg).

### Depois do cleanup

- Console Bedrock exibindo “Nenhuma base de conhecimento”: [`task09-kb-after-cleanup.jpg`](evidence/task09-kb-after-cleanup.jpg).
- Console S3 exibindo zero buckets de uso geral: [`task09-s3-after-cleanup.jpg`](evidence/task09-s3-after-cleanup.jpg).

As capturas foram enquadradas/cortadas para excluir o cabeçalho que continha o account ID. Os nomes físicos da POC foram mantidos porque fazem parte do inventário técnico e não são credenciais.

## Consultas, respostas e fontes

Os resultados completos sanitizados da POC estão em [`TASK08-EVIDENCE.md`](TASK08-EVIDENCE.md):

- uma chamada Textract `AnalyzeDocument(TABLES)`;
- uma ingestão de dois documentos e 13 vetores;
- três tentativas `Retrieve`;
- duas chamadas `RetrieveAndGenerate` com Nova Micro;
- fontes S3 e limitações reais de recuperação/fundamentação.

Não foram realizadas novas consultas Bedrock na Task 09.

## Custo observado

O Cost Explorer foi consultado para o intervalo `2026-08-21`–`2026-08-22`, granularidade diária, métrica `UnblendedCost` e agrupamento por serviço.

Resultado real:

- um período retornado;
- período marcado como `Estimated = true`;
- zero grupos de serviço consolidados;
- nenhum valor atribuível à POC disponível naquele momento.

Conclusão: **custo observado indisponível por atraso de consolidação**. Este documento não afirma custo zero. Como tags de alocação não foram previamente ativadas, um valor futuro por serviço também deve ser interpretado com cautela se houver outro uso na mesma conta.

## Cleanup verificado

| Recurso | Resultado |
|---|---|
| Data source `DVYRKFQ4ZV` | excluído; consulta posterior confirmou ausência |
| Knowledge Base `20R3EKHYB9` | excluída; consulta posterior confirmou ausência |
| 13 vetores | removidos pela política `DELETE`; contagem 0 antes de excluir o índice |
| Índice `wiki-poc-index-d88a9560` | excluído; ausência verificada |
| Vector bucket `wiki-poc-vector-d88a9560` | excluído; ausência verificada |
| Bucket `wiki-poc-raw-d88a9560` | 3 versões removidas; bucket excluído; ausência verificada |
| Bucket `wiki-poc-processed-d88a9560` | 5 versões removidas; bucket excluído; ausência verificada |
| Service role `wiki-poc-kb-role-d88a9560` | política inline removida; role excluída; ausência verificada |
| Deploy role `wiki-poc-deployer-d88a9560` | política inline removida; role excluída; ausência verificada |
| Usuário `wiki-poc-bootstrap-d88a9560` | access key revogada, política removida, usuário excluído; ausência verificada |
| Perfis locais temporários | valores de credencial/assunção esvaziados; login root encerrado |
| Budget da POC | não aplicável; nenhum Budget foi criado pela POC |

O diretório local `raw/` permaneceu fora do alvo de cleanup.

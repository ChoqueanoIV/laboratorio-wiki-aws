# Custos e FinOps

## Objetivo
Manter a POC acadêmica barata e demonstrar responsabilidade financeira.

## Guardrails
- Nenhum recurso AWS antes da Task 08.
- Antes da POC, revisar pricing atual na região escolhida.
- Sugerir AWS Budget de US$ 5.
- Preferir pay-per-use/serverless.
- Evitar recursos com cobrança contínua sem necessidade.
- Registrar todos os recursos em `docs/AWS-RESOURCES.md`.
- Planejar cleanup.

## Atenção por serviço
| Serviço | Atenção |
|---|---|
| S3 | baixo volume |
| Lambda | baixo volume |
| Step Functions | poucas execuções |
| Textract | controlar páginas |
| Bedrock | controlar modelo e chamadas |
| Athena | limitar scans |
| S3 Vectors | confirmar preço atual |
| OpenSearch Serverless | evitar sem justificativa de custo |

Budget é alerta, não garantia de interrupção automática de gastos.

## Preflight da Task 08 — 21/08/2026

- Região proposta: `us-east-1`, pois o endpoint atual do Textract não está listado em `sa-east-1`; manter a POC em uma região evita transferência e IAM cross-Region.
- Escopo máximo: 3 arquivos S3, 1 página Textract com tabela, 2 documentos na Knowledge Base, 3 retrieves e 2 gerações condicionais.
- Vector store: S3 Vectors; nenhum OpenSearch, Aurora ou capacidade provisionada.
- Estimativa: materialmente abaixo de US$ 5 e provavelmente abaixo de US$ 1 sob essas premissas; não é custo observado nem garantia.
- Gate: reconfirmar preço antes da execução e parar se a estimativa atualizada ultrapassar US$ 1.
- Budget sugerido: US$ 5 com alerta por e-mail; depende de endereço fornecido pelo usuário.
- Inventário e cleanup detalhados em `docs/AWS-RESOURCES.md`.
- Estado em 21/08/2026: POC e cleanup concluídos. O Cost Explorer retornou o período como estimado e sem grupos de serviço consolidados para o dia da execução. Portanto, custo observado da POC permanece indisponível e não foi registrado como US$ 0. Todos os recursos persistentes inventariados foram removidos para impedir novas cobranças de armazenamento/uso.

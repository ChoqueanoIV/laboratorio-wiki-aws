# Arquitetura de Referência Consolidada

A referência vigente está no diagrama principal em [`diagrams/architecture.md`](../diagrams/architecture.md). Ela representa uma proposta, não uma implantação AWS existente.

## Princípios

- S3 Raw é a fonte canônica, privada, versionada e ligada aos derivados por `document_id`, versão e checksum.
- PDF digital evita OCR; PNG/PDF escaneado usa Textract; CSV permanece estruturado.
- Step Functions explicita rotas, retries e quarentena; Lambda executa validações e transformações curtas.
- S3 Processed mantém extração, normalizados, manifestos e Parquet sem alterar originais.
- Bedrock Knowledge Bases + Titan Text Embeddings V2 + S3 Vectors atendem RAG documental de baixa frequência.
- Glue Data Catalog + Athena atendem agregações determinísticas do CRM.
- Lambda combina RAG e SQL somente em perguntas mistas e mantém fontes separadas.
- Amplify + Cognito + API Gateway fornecem interface autenticada e somente consulta.
- IAM, criptografia, CloudWatch, CloudTrail e FinOps são controles transversais.
- S3 Vectors oferece busca semântica nesta proposta; OpenSearch Serverless só entra com requisito medido de busca híbrida, facetas, QPS ou latência.

## Entrega incremental

1. Preservação e classificação no S3.
2. Extração direta, Textract e CSV validado/Parquet.
3. Glue/Athena para consultas determinísticas.
4. Normalização, metadados e avaliação de qualidade.
5. Knowledge Base/S3 Vectors com poucas consultas autorizadas.
6. Interface autenticada e observabilidade proporcional ao uso.

Cada fase deve ter critérios de aceite, estimativa de custo, inventário e cleanup. Nenhum recurso pode ser criado antes da Task 08 ou sem autorização explícita.

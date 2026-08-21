# Inventário da POC AWS

## Estado

**TASK 09 — CLEANUP CONCLUÍDO E VERIFICADO**

Atualizado em 21/08/2026. O usuário autorizou explicitamente a POC em `us-east-1`, aceitou cobrança variável sem um novo Budget por e-mail e autorizou o cleanup posterior. A POC foi executada pela cadeia bootstrap → role temporária. Na Task 09, todos os recursos persistentes inventariados foram excluídos e sua ausência foi verificada. A access key foi revogada na AWS, os perfis temporários locais foram esvaziados e o login root foi encerrado.

## Região executada

`us-east-1` — US East (N. Virginia).

Motivo: a lista oficial atual de endpoints do Amazon Textract não inclui `sa-east-1`. Bedrock Knowledge Bases com Titan Text Embeddings V2 e S3 Vectors têm suporte documentado nas regiões necessárias, mas usar São Paulo exigiria dividir a POC com outra região para OCR. Como o acervo é inteiramente fictício, `us-east-1` mantém S3, Textract, Bedrock e S3 Vectors na mesma região e simplifica custo, IAM e cleanup.

Não será usado cross-Region inference.

## Escopo autorizado e executado

POC mínima, manual e descartável:

1. preservar os três arquivos em Amazon S3;
2. executar Amazon Textract `AnalyzeDocument` com `TABLES` somente no PNG de uma página;
3. gravar a saída real do Textract no S3 processado;
4. preparar dois textos normalizados para consulta: PDF digital e resultado validado do PNG;
5. criar uma Amazon Bedrock Knowledge Base com Titan Text Embeddings V2 e Amazon S3 Vectors;
6. sincronizar apenas os dois documentos, sem vetorização linha a linha do CSV;
7. executar no máximo 3 chamadas `Retrieve` e, se o modelo de geração estiver disponível e habilitado, no máximo 2 chamadas `RetrieveAndGenerate` com Amazon Nova Micro;
8. registrar respostas, trechos e fontes reais; parar imediatamente após o limite.

O CSV será preservado no S3, mas Glue/Athena, Lambda, Step Functions, EventBridge, DynamoDB, Amplify, Cognito, API Gateway, KMS, OpenSearch e Aurora ficam **fora desta POC mínima**. A arquitetura final continua válida; a POC não precisa criar todos os seus componentes.

## Inventário planejado e real

| Recurso lógico | Serviço/tipo | Quantidade máxima | Cobrança principal | Estado | Nome/ID/ARN real |
|---|---|---:|---|---|---|
| `wiki-poc-bootstrap` | IAM user temporário, política assume-only | 1 | sem tarifa direta | DELETED / POLICY REMOVED / VERIFIED | `wiki-poc-bootstrap-d88a9560` |
| chave do bootstrap | IAM access key temporária em perfil local, nunca no Git | 1 | sem tarifa direta | REVOKED IN AWS / LOCAL VALUES CLEARED / VERIFIED | ID e segredo deliberadamente omitidos |
| `wiki-poc-deployer` | IAM role temporária de implantação | 1 | sem tarifa direta | DELETED / POLICY REMOVED / VERIFIED | `wiki-poc-deployer-d88a9560` |
| `wiki-poc-raw` | Bucket S3 de uso geral | 1 | armazenamento e requests | DELETED / ALL VERSIONS REMOVED / VERIFIED | `wiki-poc-raw-d88a9560` |
| `wiki-poc-processed` | Bucket S3 de uso geral | 1 | armazenamento e requests | DELETED / ALL VERSIONS REMOVED / VERIFIED | `wiki-poc-processed-d88a9560` |
| `wiki-poc-vector` | S3 vector bucket | 1 | armazenamento, PUT e consultas | DELETED / VERIFIED | `wiki-poc-vector-d88a9560` |
| `wiki-poc-vector-index` | S3 vector index, float, dimensão compatível com Titan V2 | 1 | vetores armazenados/consultados | DELETED / VERIFIED; 0 vetores antes da exclusão | `wiki-poc-index-d88a9560`; `float32`, 1024 dimensões, cosseno |
| `wiki-poc-kb-role` | IAM service role + policy mínima para Knowledge Bases | 1 | sem tarifa direta; permissões geram uso dos serviços | DELETED / POLICY REMOVED / VERIFIED | `wiki-poc-kb-role-d88a9560`; acesso somente a `processed/kb/*`, Titan V2 e índice exato |
| `wiki-poc-kb` | Amazon Bedrock Knowledge Base | 1 | ingestão/embeddings, vector store e retrieval | DELETED / VERIFIED | `wiki-poc-kb-d88a9560`; ID `20R3EKHYB9` |
| `wiki-poc-data-source` | Data source S3 da Knowledge Base | 1 | sincronização e embeddings | DELETED / VERIFIED | `wiki-poc-data-source-d88a9560`; ID `DVYRKFQ4ZV`; prefixo exclusivo `kb/` |
| OCR do PNG | Textract `AnalyzeDocument` com `TABLES` | 1 página, 1 chamada | página analisada por feature | COMPLETED — LIMIT REACHED | 1 chamada; 2 blocos `TABLE`, 70 `LINE`, 340 `WORD`; 0 blocos marcados `HANDWRITING` |
| Consultas semânticas | Bedrock Knowledge Bases `Retrieve` | máximo 3 | chamadas de retrieval + consulta vetorial | COMPLETED — LIMIT REACHED | 3 tentativas; a segunda teve resposta recebida, mas falha local de renderização Unicode |
| Respostas RAG | Bedrock `RetrieveAndGenerate` com Nova Micro | máximo 2 | retrieval + tokens de entrada/saída | COMPLETED — LIMIT REACHED | 2 chamadas com `amazon.nova-micro-v1:0` |
| `wiki-poc-budget` | AWS Budget mensal de US$ 5 com alerta | 1 | Budget é alerta, não bloqueio de gasto | SKIPPED — usuário já possui alerta e não autorizou novo e-mail | nenhum criado pela POC |

Nomes físicos foram gerados com sufixo aleatório para unicidade e registrados após cada criação. Nenhuma credencial, secret, token ou arquivo de configuração AWS foi salvo no repositório.

## Cobrança e estimativa responsável

As páginas oficiais consultadas em 21/08/2026 mostram cobrança por uso:

- S3 cobra armazenamento, requests e transferência; não há mínimo geral para o bucket.
- Textract cobra por página e por feature; `AnalyzeDocument` com tabela inclui OCR.
- Bedrock cobra conforme modelo/tokens e operações de Knowledge Bases.
- S3 Vectors cobra armazenamento lógico, PUT, processamento/retorno de consultas e chamadas da API.
- Budget de US$ 5 apenas notifica; não interrompe automaticamente serviços.

Com aproximadamente 3,8 MB de originais, uma página Textract, dois documentos pequenos, no máximo três retrieves e duas gerações curtas, a expectativa é permanecer materialmente abaixo de US$ 5 e provavelmente abaixo de US$ 1. Isso é **estimativa**, não custo observado nem garantia. O preço aplicável será novamente conferido no console/Price List imediatamente antes da primeira criação. Se a estimativa atualizada superar US$ 1 para este escopo, a execução deve parar e pedir nova decisão.

Não será contratado Provisioned Throughput, Savings Plan, capacidade contínua, OpenSearch Serverless ou banco de dados.

Fontes oficiais:

- [Amazon Textract pricing](https://aws.amazon.com/textract/pricing/)
- [Amazon Bedrock pricing](https://aws.amazon.com/bedrock/pricing/)
- [Amazon S3 pricing, incluindo Vectors](https://aws.amazon.com/s3/pricing/)
- [Regiões e endpoints do Textract](https://docs.aws.amazon.com/general/latest/gr/textract.html)
- [Regiões do S3 Vectors](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-vectors-regions-quotas.html)
- [Modelos/regiões de Knowledge Bases](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-supported.html)

## Riscos de custo e operação

| Risco | Controle antes/durante a POC |
|---|---|
| Sincronizações repetidas gerarem embeddings novamente | Uma sincronização inicial; nova sincronização somente após inspeção e autorização dentro do mesmo escopo |
| Loop de consultas Bedrock | Contadores locais rígidos: 3 retrieves e 2 gerações no máximo |
| Modelo indisponível ou acesso não habilitado | Verificar lista/acesso; se Nova Micro não estiver disponível, executar somente `Retrieve` e não trocar por modelo mais caro sem autorização |
| Metadados ou chunking multiplicarem vetores | Indexar apenas dois documentos pequenos e conferir contagem após sincronização |
| Recurso esquecido após teste | Inventário real obrigatório e cleanup em ordem definida |
| Budget não impedir cobrança | Limites de chamadas, nenhuma capacidade provisionada e verificação manual de recursos |
| Custo aparecer com atraso | Não interpretar ausência imediata no Cost Explorer como custo zero; registrar atraso de faturamento |
| Exclusão acidental de dados do usuário | Buckets terão prefixo exclusivo da POC; cleanup só atingirá IDs registrados neste arquivo; `raw/` local nunca será alterado |

## Plano de execução após autorização

1. Verificar identidade e região com `aws sts get-caller-identity` e configuração da CLI, sem exibir ou persistir credenciais.
2. Verificar acesso/quotas de Textract, Bedrock e S3 Vectors.
3. Reconfirmar pricing e abortar se o teto estimado do escopo ultrapassar US$ 1.
4. Criar Budget de US$ 5 se o usuário fornecer e-mail de notificação.
5. Criar buckets S3 com Block Public Access, Versioning e SSE-S3.
6. Enviar cópias dos três originais, validando SHA-256 antes e depois; nunca modificar `raw/` local.
7. Chamar Textract uma vez para o PNG e salvar JSON real no bucket processado.
8. Preparar/validar derivados documentais e seus metadados de fonte.
9. Criar vector bucket/index, role mínima, Knowledge Base e data source.
10. Sincronizar uma vez e registrar job, estado, contagem disponível e erros reais.
11. Executar até três retrieves e até duas respostas geradas condicionais; registrar fontes reais.
12. Atualizar este inventário imediatamente após cada criação ou exclusão.
13. Parar antes da Task 09; evidências, custo observado e cleanup pertencem à etapa seguinte.

## Plano de cleanup

Executado somente sobre recursos cujos IDs reais estejam registrados neste inventário:

1. interromper novas sincronizações e consultas;
2. registrar evidências necessárias antes da exclusão;
3. excluir data source e Knowledge Base, verificando a política de retenção/deleção dos vetores;
4. excluir explicitamente vector index e vector bucket restantes;
5. esvaziar todas as versões e delete markers dos dois buckets da POC e então excluir os buckets;
6. excluir role e políticas IAM exclusivas da POC;
   - excluir primeiro `wiki-poc-kb-role-d88a9560` após a Knowledge Base;
   - remover a política inline e excluir `wiki-poc-deployer-d88a9560` usando a identidade bootstrap/root de cleanup;
   - excluir a access key local/AWS, a política assume-only e `wiki-poc-bootstrap-d88a9560`;
7. excluir ou manter o Budget somente conforme decisão registrada;
8. confirmar por listagens que não restou recurso inventariado;
9. registrar custo observado quando a cobrança estiver disponível, sem tratar atraso como zero;
10. atualizar este arquivo com status `DELETED`, horário e evidência de verificação.

## Resultado observado da Task 08

- identidade operacional confirmada como sessão assumida de `wiki-poc-deployer-d88a9560`, em `us-east-1`;
- dois buckets S3 privados com Block Public Access, Versioning e SSE-S3;
- três originais copiados com checksum SHA-256 conferido; os hashes locais de `raw/` permaneceram idênticos;
- uma chamada Textract `AnalyzeDocument(TABLES)`: 2 blocos `TABLE`, 70 `LINE`, 340 `WORD` e 0 blocos marcados `HANDWRITING`;
- dois derivados documentais e dois sidecars de metadados sob `processed/kb/`, além do JSON real do Textract;
- Knowledge Base `20R3EKHYB9` ativa, data source `DVYRKFQ4ZV` disponível e ingestão `3DNZKDLKMD` completa;
- ingestão: 2 documentos escaneados, 2 novos indexados, 0 modificados e 0 falhas;
- índice com 13 vetores, sem `nextToken` na listagem de até 500 itens;
- três tentativas de `Retrieve` e duas chamadas `RetrieveAndGenerate`; nenhum limite será ampliado nesta task;
- o RAG não enumerou as cinco decisões porque os chunks recuperados traziam sobretudo o resumo; para “ação prioritária”, não recuperou a anotação e produziu uma interpretação genérica não sustentada, embora tenha declarado evidência insuficiente para o prazo;
- custo observado ainda indisponível; ausência imediata no faturamento não será registrada como zero;
- evidência sanitizada: `docs/TASK08-EVIDENCE.md`.

## Resultado observado da Task 09

- o Cost Explorer foi consultado para `2026-08-21` a `2026-08-22` com custo não combinado e agrupamento por serviço;
- a resposta continha um período marcado como estimado e zero grupos consolidados; isso foi registrado como `NOT_AVAILABLE`, não como custo zero;
- screenshots reais foram capturados antes e depois do cleanup e sanitizados para não mostrar account ID;
- data source `DVYRKFQ4ZV` e Knowledge Base `20R3EKHYB9` foram excluídos e consultas posteriores confirmaram ausência;
- a política `DELETE` da fonte removeu os 13 vetores; a listagem imediatamente antes de excluir o índice retornou 0;
- índice e vector bucket foram excluídos e a ausência foi verificada;
- todas as versões dos três objetos originais e dos cinco objetos processados foram removidas; os dois buckets vazios foram excluídos e verificados;
- políticas inline e as duas roles IAM foram removidas;
- a única access key temporária foi revogada, a política assume-only e o usuário bootstrap foram excluídos, e a ausência foi verificada;
- valores dos perfis locais temporários foram esvaziados e a sessão `aws login` root foi encerrada;
- nenhum Budget foi criado pela POC, portanto não havia Budget da POC para excluir;
- evidência sanitizada consolidada: `docs/TASK09-EVIDENCE.md`.

O JSON local de evidência pode permanecer no repositório somente após revisão para remover account ID, ARN desnecessário, credenciais, tokens e dados sensíveis. Screenshots e resultados devem registrar apenas fatos reais.

## Gate de autorização

**AUTORIZADO em 21/08/2026.** O usuário informou que já possui alerta de orçamento e US$ 120 em créditos, autorizou os recursos deste arquivo em `us-east-1`, aceitou o risco de cobrança variável sem novo Budget por e-mail e autorizou o cleanup. Em autorização complementar, permitiu usar root somente para criar/assumir a role temporária de implantação; toda a POC restante deve usar essa role.

**Bootstrap resolvido:** `AssumeRole` inicialmente retornou `AccessDenied` porque contas root não podem assumir roles. Com autorização adicional, foi criado `wiki-poc-bootstrap-d88a9560`, limitado exclusivamente a assumir `wiki-poc-deployer-d88a9560`; a trust policy da role foi restringida ao ARN exato desse usuário. A access key temporária existe apenas no perfil local e será removida no cleanup.

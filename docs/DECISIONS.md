# Registro de Decisões Arquiteturais

## ADR-001 — Tratar formatos pela natureza
**Status:** Aceita
PDF digital, imagem e CSV terão pipelines diferentes.

## ADR-002 — Preservar raw/
**Status:** Aceita
`raw/` é somente leitura.

## ADR-003 — Evitar OCR no PDF digital
**Status:** Aceita
A camada textual já existe.

## ADR-004 — Textract para imagem
**Status:** Aceita
A imagem precisa de OCR e pode conter estruturas visuais/handwriting.

## ADR-005 — CSV permanece estruturado
**Status:** Aceita
Agregações devem ser determinísticas quando possível.

## ADR-006 — Priorizar serverless/pay-per-use
**Status:** Aceita
Compatível com baixo volume e objetivo acadêmico.

## ADR-007 — Não inventar resposta sem evidência
**Status:** Aceita
A Wiki deve sinalizar insuficiência de fontes.

## ADR-008 — Separar originais de derivados no Amazon S3
**Status:** Aceita
Originais ficam em um bucket privado canônico e resultados, manifestos e quarentena em outro bucket. O `document_id` deriva do SHA-256 do conteúdo e liga todas as saídas à chave e à versão do objeto original.

## ADR-009 — Orquestrar ingestão idempotente com AWS Step Functions
**Status:** Aceita
EventBridge inicia uma máquina de estados que valida, classifica e encaminha PDF digital, imagem/PDF escaneado e CSV por rotas diferentes. Manifesto e `document_id` evitam repetir processamento já concluído.

## ADR-010 — Colocar falhas em quarentena lógica
**Status:** Aceita
Uma falha não move nem altera o original. O pipeline grava estado, diagnóstico sanitizado e referência à versão original no bucket de processados, permitindo revisão e nova tentativa rastreável.

## ADR-011 — Armazenar metadados conforme a função de acesso
**Status:** Aceita
S3 guarda conteúdo e metadados completos/versionados, DynamoDB atende estado e filtros interativos, e Glue Data Catalog descreve o CSV/Parquet para Athena. O pequeno acervo pode começar apenas com os componentes necessários e adicionar DynamoDB ou Knowledge Bases quando o padrão de consulta justificar.

## ADR-012 — Registrar proveniência e confiança por campo
**Status:** Aceita
Cada valor relevante informa se foi observado, derivado deterministicamente, inferido por IA ou validado por pessoa, além do localizador da evidência. Confiança do Textract não é confundida com confiança do enriquecimento pelo Bedrock.

## ADR-013 — Não usar LLM para agregações do CRM
**Status:** Aceita
O CSV permanece catalogado para consultas determinísticas com Glue/Athena. Bedrock pode interpretar a pergunta ou resumir um resultado calculado, mas não estimar contagens, somas ou taxas a partir de texto.

## ADR-014 — Priorizar S3 Vectors para a primeira base vetorial
**Status:** Aceita
O acervo acadêmico tem baixa frequência e pequeno volume, favorecendo S3 Vectors com Bedrock Knowledge Bases. A solução aceita busca apenas semântica nesse vector store; OpenSearch Serverless só será adotado se busca híbrida, facetas, latência ou frequência medidas justificarem maior custo e complexidade.

## ADR-015 — Rotear consultas documentais e analíticas
**Status:** Aceita
Perguntas documentais usam RAG com Knowledge Bases; agregações do CRM usam Glue/Athena; perguntas mistas combinam resultados rotulados e mantêm fontes separadas.

## ADR-016 — Exigir fonte ou declarar evidência insuficiente
**Status:** Aceita
Toda afirmação factual deve ser ligada a chunk/página ou resultado determinístico do Athena. Ausência, conflito, baixa confiança ou falta de autorização resultam em resposta limitada, revisão ou declaração de evidência insuficiente.

## ADR-017 — Usar interface web serverless e somente consulta
**Status:** Aceita
Amplify Hosting, Cognito, API Gateway e Lambda compõem a interface inicial. Não haverá agente com ações operacionais; a primeira versão consulta documentos/CRM e coleta feedback.

## ADR-018 — Entregar a arquitetura em fases com guardrails de custo
**Status:** Aceita
A implantação futura começa por preservação e extração, adiciona analytics e só então RAG/interface quando o aceite justificar. Cada fase exige estimativa, Budget de US$ 5, inventário, limites e cleanup; a arquitetura completa não implica criar todos os serviços na POC.

## ADR-019 — Executar a POC mínima em us-east-1
**Status:** Aceita e executada na Task 08
A lista atual de endpoints do Textract não inclui `sa-east-1`. Como o acervo é fictício, a POC concentrou S3, Textract, Bedrock Knowledge Bases e S3 Vectors em `us-east-1`, sem inferência cross-Region. O usuário autorizou região, cobrança, inventário e cleanup. A execução confirmou a viabilidade técnica, mas também mostrou limitações reais: handwriting não detectado, recuperação insuficiente para enumerar decisões e interpretação RAG parcialmente não sustentada. Os recursos permanecem ativos somente até a Task 09.

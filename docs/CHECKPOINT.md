# Checkpoint

## Tasks
- [x] 01 Discovery
- [x] 02 Quest 1
- [x] 03 Quest 2
- [x] 04 Quest 3
- [x] 05 Quest 4
- [x] 06 Arquitetura final
- [x] 07 README/portfólio
- [x] 08 POC AWS controlada
- [x] 09 Evidências/custos/cleanup
- [x] 10 Revisão final

## Registro

### Task 01 — Discovery (concluída em 20/08/2026)

- **Arquivos analisados:** `README.md`, `resposta.md`, documentos de orientação e tasks em `docs/`, além dos três arquivos de `raw/`: o PDF digital de 5 páginas, o PNG digitalizado de 1900 × 2700 pixels e o CSV com 240 registros e 19 colunas.
- **Mudanças:** criado `docs/ACERVO.md` com inventário, conteúdo, estrutura, dificuldades, perfil do CSV e diferenças de tratamento baseados somente no que foi observado. Nenhuma alteração em `resposta.md`.
- **Decisões:** nenhuma nova decisão arquitetural; permanecem válidas as ADRs existentes. A Task 01 apenas confirmou no acervo as naturezas distintas já previstas para PDF, PNG e CSV.
- **Validações:** os três arquivos foram analisados; a camada textual e as cinco páginas do PDF foram verificadas; tabela e manuscritos do PNG foram inspecionados visualmente; esquema, preenchimento e consistências básicas do CSV foram perfilados; hashes SHA-256 foram registrados em `docs/ACERVO.md` para conferir a integridade de `raw/`.
- **Pendências:** a Quest 1 de `resposta.md` continua intencionalmente sem preenchimento, pois pertence à Task 02. Nenhum recurso AWS foi criado.
- **Próximo passo:** Task 02 — Quest 1, somente após nova solicitação do usuário.

### Task 02 — Quest 1 (concluída em 20/08/2026)

- **Arquivos analisados:** `docs/tasks/02-quest-1.md`, `docs/ACERVO.md`, `resposta.md` e `docs/CHECKPOINT.md`.
- **Mudanças:** preenchidas somente as seções 1.1 a 1.4 da Quest 1 em `resposta.md`, cobrindo os três formatos reais, desafios específicos, informações a extrair e classificação sem depender de subpastas.
- **Decisões:** nenhuma ADR nova. A resposta aplica as decisões já aceitas de tratar PDF digital, imagem digitalizada e CSV por suas naturezas, preservar `raw/` e manter o CSV estruturado.
- **Validações:** os três arquivos reais foram nomeados; os tratamentos diferentes foram justificados; a classificação considera assinatura binária, extensão, MIME, camada textual e estrutura; não restou `Preencha aqui` na Quest 1.
- **Pendências:** as Quests 2 a 4 e as demais seções de `resposta.md` permanecem intencionalmente sem preenchimento. Nenhum recurso AWS foi criado.
- **Próximo passo:** Task 03 — Quest 2, somente após nova solicitação do usuário.

### Task 03 — Quest 2 (concluída em 20/08/2026)

- **Arquivos analisados:** `docs/tasks/03-quest-2.md`, `docs/ARCHITECTURE.md`, `docs/COSTS.md`, `docs/SECURITY.md`, `docs/DECISIONS.md`, `docs/PROJECT-GUIDE.md`, `docs/ACERVO.md` e a Quest 2 de `resposta.md`.
- **Mudanças:** preenchidas somente as seções 2.1 a 2.4 de `resposta.md`; adicionadas as ADR-008 a ADR-010 em `docs/DECISIONS.md`.
- **Decisões:** separar buckets de originais e derivados; usar SHA-256 como base do `document_id`; orquestrar ingestão idempotente com EventBridge, Step Functions e Lambda; manter falhas em quarentena lógica sem mover o original.
- **Validações:** foram documentados fluxos separados para PDF digital sem OCR, PNG/PDF escaneado com Textract e CSV estruturado; preservação com checksum, Versioning, criptografia e rastreabilidade; S3 processado, retries limitados, CloudWatch, DLQ, manifesto e quarentena; custos e complexidade foram considerados. A seção distingue explicitamente proposta de implementação real.
- **Pendências:** as Quests 3 e 4 e as demais seções posteriores continuam intencionalmente sem preenchimento. Pricing regional deve ser verificado antes de eventual POC, que permanece bloqueada até a Task 08 e exige autorização explícita. Nenhum recurso AWS foi criado.
- **Próximo passo:** Task 04 — Quest 3, somente após nova solicitação do usuário.

### Task 04 — Quest 3 (concluída em 20/08/2026)

- **Arquivos analisados:** `docs/tasks/04-quest-3.md`, `docs/PROJECT-GUIDE.md`, `docs/SECURITY.md`, `docs/DECISIONS.md`, `docs/ACERVO.md` e a Quest 3 de `resposta.md`.
- **Mudanças:** preenchidas somente as seções 3.1 a 3.4 de `resposta.md`; adicionadas as ADR-011 a ADR-013 em `docs/DECISIONS.md`.
- **Decisões:** contrato JSON normalizado e versionado; metadados distribuídos por função entre S3, DynamoDB e Glue; proveniência e confiança por campo; inferências do Bedrock não substituem fatos observados; agregações do CRM permanecem em Glue/Athena.
- **Validações:** o contrato inclui `document_id`, fonte/versionamento, conteúdo localizável, status e timestamps; a tabela cobre data, tema, participantes, decisões, responsáveis, próximos passos, riscos, confidencialidade, confiança e rastreabilidade; o uso de Bedrock prevê JSON validado, evidência, revisão humana e distinção entre observado e inferido; todos os metadados se conectam à versão e ao checksum do original.
- **Pendências:** Quest 4 e seções posteriores permanecem intencionalmente sem preenchimento. Modelo e pricing do Bedrock serão confirmados antes de eventual POC na Task 08, ainda bloqueada sem autorização explícita. Nenhum recurso AWS foi criado.
- **Próximo passo:** Task 05 — Quest 4, somente após nova solicitação do usuário.

### Task 05 — Quest 4 (concluída em 20/08/2026)

- **Arquivos analisados:** `docs/tasks/05-quest-4.md`, `docs/ARCHITECTURE.md`, `docs/COSTS.md`, `docs/SECURITY.md`, `docs/DECISIONS.md`, `docs/ACERVO.md`, a Quest 4 de `resposta.md` e documentação oficial atual da AWS sobre Knowledge Bases, chunking, recuperação/citações, Titan Text Embeddings V2, S3 Vectors e Cognito/API Gateway.
- **Mudanças:** preenchidas somente as seções 4.1 a 4.5 de `resposta.md`; adicionadas as ADR-014 a ADR-017 em `docs/DECISIONS.md`.
- **Decisões:** chunking lógico com baseline fixo; S3 Vectors como vector store inicial de baixa frequência; busca semântica combinada com filtros/exatidão; roteamento RAG versus SQL/Athena; fontes obrigatórias e recusa por evidência insuficiente; interface serverless somente consulta.
- **Validações:** a Quest cobre embeddings Bedrock, Knowledge Bases, limitações do vector store, metadados/filtros, RAG e citações, rota híbrida para CSV, Cognito/IAM/KMS, CloudWatch/CloudTrail, qualidade, segurança, custos e cleanup. A proposta distingue capacidades atuais verificadas de componentes ainda não implementados.
- **Pendências:** a seção Arquitetura Final e posteriores permanecem intencionalmente sem preenchimento. Região e pricing serão revistos imediatamente antes de eventual POC; a Task 08 continua bloqueada sem autorização explícita. Nenhum recurso AWS foi criado.
- **Próximo passo:** Task 06 — Arquitetura Final, somente após nova solicitação do usuário.

### Task 06 — Arquitetura Final (concluída em 20/08/2026)

- **Arquivos analisados:** `docs/tasks/06-architecture-final.md`, quests preenchidas em `resposta.md`, `docs/ARCHITECTURE.md`, `diagrams/architecture.md`, `docs/COSTS.md`, `docs/SECURITY.md` e `docs/DECISIONS.md`.
- **Mudanças:** preenchida toda a seção Arquitetura Final de `resposta.md`; consolidado o Mermaid em `diagrams/architecture.md`; atualizado `docs/ARCHITECTURE.md`; adicionada ADR-018.
- **Decisões:** arquitetura dividida em ingestão, conhecimento documental, analytics do CRM, consulta e controles transversais; implantação futura incremental com gate de aceite/custo por fase; a POC não precisa criar todos os componentes do desenho final.
- **Validações:** visão geral, serviços, fluxo, diagrama textual, riscos e melhorias foram preenchidos; cada serviço lista motivo, chamador, entrada, saída e alternativa/condição de uso; PDF/PNG/CSV mantêm rotas distintas; RAG e Athena permanecem rastreáveis; proposta e implementação real estão explicitamente separadas.
- **Pendências:** identificação, checklist e conclusão pertencem a etapas posteriores; README/portfólio permanece para Task 07. Região/pricing e recursos reais só serão definidos antes da Task 08, que continua bloqueada sem autorização explícita. Nenhum recurso AWS foi criado.
- **Próximo passo:** Task 07 — README e Portfólio, somente após nova solicitação do usuário.

### Task 07 — README e Portfólio (concluída em 21/08/2026)

- **Arquivos analisados:** `docs/tasks/07-readme-portfolio.md`, `README.md`, `resposta.md`, `docs/ACERVO.md`, `docs/ARCHITECTURE.md`, `diagrams/architecture.md`, `docs/COSTS.md`, `docs/SECURITY.md`, `docs/DECISIONS.md` e `docs/CHECKPOINT.md`.
- **Mudanças:** o README original do enunciado foi refatorado para uma apresentação profissional da solução, com estado real, acervo, Mermaid, fluxo, serviços e justificativas, RAG + Athena, metadados, segurança, observabilidade, custos, falhas, perguntas, riscos, aprendizados, estrutura e próximos passos.
- **Decisões:** nenhuma ADR nova; o README consolida as ADR-001 a ADR-018 sem alterar a arquitetura aceita.
- **Validações:** PDF/PNG/CSV e seus tratamentos estão explícitos; RAG documental e SQL determinístico estão separados; Mermaid e exemplos de perguntas foram incluídos; links locais foram conferidos; POC, recursos, custos e resultados inexistentes não são apresentados como realizados.
- **Pendências:** Task 08 permanece bloqueada por padrão e requer autorização explícita após apresentação de região, recursos, cobrança, risco e cleanup. A identificação e a conclusão de `resposta.md` ainda pertencem à revisão posterior.
- **Próximo passo:** Task 08 — POC AWS controlada, somente mediante autorização explícita do usuário; sem essa autorização, não criar recursos e permanecer parado.

### Task 08 — POC AWS controlada (concluída em 21/08/2026)

- **Autorização:** usuário autorizou os recursos de `docs/AWS-RESOURCES.md` em `us-east-1`, cobrança variável sem novo Budget por e-mail, cleanup posterior e a cadeia IAM temporária necessária para não executar a POC como root.
- **Recursos reais:** usuário IAM bootstrap assume-only, role temporária de implantação, service role da Knowledge Base, dois buckets S3, um vector bucket/index, uma Knowledge Base e um data source; nomes e IDs sanitizados/inventariados em `docs/AWS-RESOURCES.md`.
- **Operações pagas limitadas:** 1 `AnalyzeDocument(TABLES)`, 1 ingestão de 2 documentos, 3 tentativas `Retrieve` e 2 `RetrieveAndGenerate` com Nova Micro; limites atingidos e não ampliados.
- **Resultados:** Textract retornou 2 tabelas, 70 linhas, 340 palavras e 0 handwriting; ingestão completou com 2 novos documentos e 0 falhas; índice contém 13 vetores.
- **Qualidade:** o RAG não enumerou as cinco decisões e não sustentou corretamente a interpretação de “ação prioritária”; a evidência negativa foi preservada em vez de apresentada como sucesso.
- **Integridade:** hashes dos três arquivos de `raw/` reconfirmados sem alteração; CSV preservado no S3, mas não vetorizado.
- **Custos:** pricing reconfirmado antes da criação; custo observado ainda indisponível e não considerado zero.
- **Pendências:** recursos permanecem ativos para evidências/custo e cleanup da Task 09; credencial bootstrap local e recursos IAM também estão inventariados.
- **Próximo passo:** Task 09 — evidências, custo observado e cleanup, somente após nova solicitação do usuário. Parar aqui.

### Task 09 — Evidências, custos e cleanup (concluída em 21/08/2026)

- **Evidências:** criadas quatro capturas sanitizadas em `docs/evidence/`, mostrando Knowledge Base ativa, dois buckets S3 antes do cleanup, nenhuma Knowledge Base depois e zero buckets S3 depois.
- **Consultas e respostas:** resultados reais de Textract, ingestão, Retrieve e RetrieveAndGenerate foram consolidados em `docs/TASK08-EVIDENCE.md` e referenciados em `docs/TASK09-EVIDENCE.md`.
- **Custo:** Cost Explorer consultado para o dia da POC; período ainda estimado e sem grupos de serviço consolidados. Custo observado registrado como indisponível, nunca como zero.
- **Cleanup:** data source, Knowledge Base, vetores, índice, vector bucket, todas as versões S3, dois buckets, service role, deploy role, access key, política assume-only e usuário bootstrap foram removidos.
- **Verificação:** APIs retornaram ausência após cada exclusão; screenshots pós-cleanup confirmam nenhuma Knowledge Base e zero buckets de uso geral. Perfis locais temporários foram esvaziados e login root encerrado.
- **Integridade:** `raw/` local não foi alterado; somente cópias/versionamentos AWS inventariados foram removidos.
- **Pendência:** revisão final Guardião, preenchimento dos campos finais e auditoria completa pertencem à Task 10.
- **Próximo passo:** Task 10 — Revisão Final Guardião, somente após nova solicitação do usuário. Parar aqui.

### Task 10 — Revisão Final Guardião (concluída em 23/08/2026)

- **Auditoria:** revisados integralmente `MASTER-PROMPT.md`, `AGENTS.md`, `README.md`, `resposta.md`, todo o conteúdo textual de `docs/` e a task atual.
- **Mudanças:** preenchidos identificação, checklist e conclusão em `resposta.md`; corrigidas referências históricas que confundiam arquitetura proposta com POC executada; atualizado o estado no README e no diagrama; criado `docs/FINAL-REVIEW.md`.
- **Decisões:** nenhuma nova ADR. A revisão apenas aplica e consolida as decisões existentes, preservando a distinção entre proposta, evidência real e limitação observada.
- **Validações:** hashes de `raw/` idênticos ao inventário; nenhum link local quebrado; Mermaid consistente; nenhum padrão de credencial encontrado; somente serviços AWS; todos os critérios Guardião estão OK ou explicitamente justificados.
- **Limitações preservadas:** custo observado e latência consolidada indisponíveis; handwriting não detectado; recuperação e citações insuficientes em parte da POC; arquitetura completa não implantada.
- **Estado final:** entrega concluída no escopo das dez tasks. Qualquer commit ou `git push` posterior exige ação/autorização própria.

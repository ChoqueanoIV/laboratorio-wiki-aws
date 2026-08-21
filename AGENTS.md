# AGENTS.md — Laboratório Wiki AWS

## Missão
Conduzir este repositório até uma entrega de nível máximo “Guardião da Wiki Perdida”.

## Leia antes de agir
1. README.md
2. resposta.md
3. docs/PROJECT-GUIDE.md
4. docs/ARCHITECTURE.md
5. docs/COSTS.md
6. docs/SECURITY.md
7. docs/DECISIONS.md
8. docs/CHECKPOINT.md
9. a task atual em docs/tasks/

## Regras imutáveis
- `raw/` é SOMENTE LEITURA. Nunca alterar, mover, renomear, excluir ou sobrescrever.
- Usar apenas serviços AWS.
- Não usar OCR, IA ou banco vetorial externo.
- Não inventar fatos observados, resultados AWS, custos, prints, ARNs ou IDs.
- Diferenciar proposta arquitetural de implementação real.
- PDF digital, imagem digitalizada e CSV devem ter tratamentos diferentes.
- Não criar recursos AWS antes da Task 08.
- A Task 08 exige autorização explícita do usuário.
- Nunca salvar credenciais no Git.
- Preferir arquitetura simples, serverless e pay-per-use quando suficiente.
- Justificar “por que” cada serviço existe.
- Não deixar `Preencha aqui` na entrega final.
- Não executar `git push` sem autorização.

## Natureza dos dados
### PDF digital
Possui camada de texto. Extrair texto sem OCR sempre que possível.

### PNG digitalizado
Usar Amazon Textract para OCR, considerando tabelas, handwriting e confiança.

### CSV de CRM
Manter estruturado. Não transformar cegamente toda a tabela em texto para RAG.
Consultas agregadas devem preferir SQL/analytics determinístico.

## Meta Guardião
A solução final deve cobrir:
- arquitetura completa;
- ponta a ponta;
- RAG;
- busca semântica;
- metadados e filtros;
- fontes/rastreabilidade;
- falhas;
- segurança;
- governança;
- monitoramento;
- custos;
- escalabilidade;
- riscos;
- evolução futura;
- apresentação profissional.

## Execução
Trabalhe uma task por vez.
Ao concluir cada uma:
1. valide critérios;
2. atualize docs/DECISIONS.md se houver decisão nova;
3. atualize docs/CHECKPOINT.md;
4. pare antes da task seguinte.

## Custos
Antes de qualquer POC:
- avaliar pricing atual;
- informar risco de custo;
- preferir baixo custo;
- sugerir Budget de US$ 5;
- inventariar recursos;
- planejar cleanup.

# 📝 Resposta do Laboratório: A Wiki Perdida dos Arquivos Corporativos

> Preencha este arquivo com a sua proposta de solução.
>
> Sua resposta deve explicar como transformar os documentos brutos da pasta `raw/` em uma Wiki Corporativa Inteligente, pesquisável e segura usando apenas serviços da AWS.

---

## 👤 Identificação

**Nome:**  
Preencha aqui

**Data:**  
Preencha aqui

**Link do repositório:**  
Preencha aqui

---

# ✅ Quest 1: O Mapa dos Arquivos Perdidos

## 1.1 Formatos encontrados na pasta `raw/`

Descreva quais tipos de arquivos existem dentro da pasta `raw/`.

```md
Exemplo de como responder, com o formato e o que ele implica:
- <extensao>: <nasce digital ou precisa de OCR?>, <o que da para extrair>
```

> Abra a pasta e liste o que voce encontrou de fato. Esta quest avalia a sua
> leitura do acervo, entao a resposta certa e a que corresponde aos arquivos.

**Sua resposta:**

```md
- `.pdf` — `ata_reuniao_vendas_sa.pdf`: PDF digital de 5 páginas e 150.295 bytes, com camada de texto selecionável. A ata contém identificação da reunião, participantes, pauta, seis indicadores, cinco decisões (`D-001` a `D-005`), seis ações (`A-001` a `A-006`), quatro riscos (`R-01` a `R-04`) e um bloco estruturado na página final. Como o texto já existe no arquivo, ele deve ser extraído diretamente, sem OCR; ainda assim, tabelas e a continuação do plano de ação entre as páginas 3 e 4 exigem preservação de estrutura.

- `.png` — `ata_resultados_vendas_novos_dados.png`: imagem digitalizada de uma página, com 1900 × 2700 pixels. Não possui camada textual diretamente extraível e precisa de OCR. Além do texto impresso, contém uma tabela de seis indicadores, linhas de assinatura, o carimbo “DOCUMENTO FICTÍCIO” e duas anotações manuscritas: “conferir CRM” e “ação prioritária”. O OCR precisa considerar texto, tabela, handwriting e posição dos elementos.

- `.csv` — `vendas_sa_dados_ficticios_laboratorio.csv`: tabela UTF-8 com BOM, delimitada por vírgulas, com 240 oportunidades e 19 colunas. Possui identificadores, datas, categorias, percentuais, valores monetários, duração, observações e campos opcionais. Não precisa de OCR e não deve ser tratado como texto corrido: sua estrutura tabular deve ser preservada para filtros e cálculos determinísticos.

Os três arquivos estão misturados diretamente em `raw/`, sem subpastas, mas representam naturezas distintas: documento digital paginado, imagem digitalizada e conjunto de dados estruturado.
```

---

## 1.2 Principais desafios encontrados

Explique quais dificuldades esses documentos podem apresentar.

```md
Exemplo:
- Arquivos sem padrão de nomenclatura
- Documentos escaneados com baixa qualidade
- Textos manuscritos ou parcialmente ilegíveis
- Atas com estruturas diferentes
- Informações importantes espalhadas em vários formatos
```

**Sua resposta:**

```md
- **PDF digital:** a extração linear pode separar incorretamente células das tabelas, repetir cabeçalhos, avisos e rodapés e perder a continuidade do plano de ação entre as páginas 3 e 4. Datas, valores, percentuais e identificadores como `D-001` e `A-001` precisam permanecer ligados às respectivas descrições.

- **PNG digitalizado:** sombras nas bordas, fundo acinzentado, pequenos pontos, variação de iluminação e leve inclinação/perspectiva podem reduzir a confiança do OCR. A tabela depende da relação espacial entre indicador, realizado, meta e variação. As anotações manuscritas têm orientação e cor diferentes do texto impresso e acrescentam contexto aos elementos próximos.

- **Possível inconsistência contextual no PNG:** o subtítulo informa “Resultados do 2º semestre”, enquanto a reunião está datada de 15/01/2026. A solução deve preservar os dois valores observados e sinalizar a divergência para revisão, sem corrigir ou inferir automaticamente qual deles estaria certo.

- **CSV estruturado:** campos vazios possuem significado dependente do status. Os 118 registros ganhos ou perdidos têm data de fechamento; os 122 registros ainda abertos têm próxima atividade; somente as 46 oportunidades perdidas têm motivo de perda. Portanto, vazio não significa automaticamente dado inválido. Datas, percentuais e valores em BRL precisam manter tipos próprios.

- **Padronização textual:** há variação de acentuação no CSV, como `Prospecção ativa`, `Indicacao de parceiro` e `Reativacao de cliente`. Uma normalização destinada à busca não pode alterar silenciosamente os valores originais nem juntar categorias distintas por engano.

- **Heterogeneidade:** atas e CRM registram conhecimento em granularidades diferentes. Nos documentos, decisões e ações aparecem em texto e tabelas; no CSV, cada linha representa uma oportunidade. Converter tudo para um único texto plano perderia estrutura, rastreabilidade e precisão analítica.
```

---

## 1.3 Informações importantes a serem extraídas

Liste quais informações precisam ser identificadas para transformar os documentos em conhecimento pesquisável.

**Sua resposta:**

```md
Para as atas em PDF e PNG, eu extrairia:

- identificação do documento, nome do arquivo, tipo, código da ata quando existir e indicação de conteúdo fictício;
- data, horários, local, formato, objetivo e pauta da reunião;
- participantes, cargos, áreas e papéis, distinguindo pessoas nomeadas de grupos genéricos, como “supervisores regionais”;
- temas discutidos, indicadores, metas, resultados, valores, percentuais e regiões mencionadas;
- decisões, respectivos identificadores e status;
- ações, responsáveis, prazos, prioridades, status e dependências;
- riscos, probabilidade, impacto e resposta planejada;
- pendências, próxima reunião e entregas esperadas;
- anotações manuscritas e sua posição contextual, como “conferir CRM” junto aos indicadores e “ação prioritária” junto ao prazo de 28/02/2026;
- referência precisa à fonte, incluindo arquivo, página no PDF ou região da imagem, e confiança da extração.

Para o CSV, eu preservaria as 19 colunas originais e seus tipos conceituais: identificador da oportunidade; datas de criação, fechamento e próxima atividade; cliente fictício; segmento; região; vendedor; origem do lead; produto; campanha; status; probabilidade; valores bruto e líquido; desconto; ciclo em dias; motivo de perda; e observação. Também registraria o número da linha ou outro localizador estável para rastrear cada registro até a fonte.

Essa separação permite responder perguntas documentais sobre decisões, responsáveis e riscos, além de perguntas analíticas sobre contagens, valores, conversão, perdas e desempenho por dimensões do CRM, sem pedir que uma IA faça cálculos que podem ser obtidos diretamente da tabela.
```

---

## 1.4 Estratégia de classificação inicial

Como você classificaria os documentos sem depender de subpastas dentro de `raw/`?

**Sua resposta:**

```md
Como `raw/` não possui subpastas, a classificação deve ser orientada pelo conteúdo e registrada como metadado, não deduzida apenas do nome do arquivo.

1. Para cada objeto, registrar nome original, tamanho, hash, data de ingestão e identificador único.
2. Verificar a assinatura binária (magic bytes) antes da extensão: `%PDF-` para PDF, assinatura PNG `89 50 4E 47 0D 0A 1A 0A` para PNG e conteúdo textual válido com cabeçalho e delimitador consistente para CSV.
3. Comparar assinatura, extensão e MIME declarado (`application/pdf`, `image/png` ou `text/csv`). Divergências devem ser sinalizadas, sem processar o arquivo apenas com base no sufixo.
4. Para PDF, verificar se as páginas possuem camada textual utilizável. Neste acervo, `ata_reuniao_vendas_sa.pdf` seria classificado como `ata_reuniao`, `pdf_digital`, `extracao_direta`, `ocr_required=false`.
5. Para PNG, classificar pela natureza visual. Neste acervo, `ata_resultados_vendas_novos_dados.png` seria `ata_reuniao`, `imagem_digitalizada`, `ocr_required=true`, `table_present=true` e `handwriting_present=true`.
6. Para CSV, validar cabeçalho, delimitador, codificação e quantidade consistente de campos. O arquivo seria `crm_oportunidades`, `dados_estruturados`, `240 registros`, `19 colunas`, `ocr_required=false`, preservando as colunas para consultas tabulares.
7. Manter separados `formato_fisico` (PDF, PNG ou CSV), `natureza` (documento digital, digitalização ou tabela), `tipo_documental` (ata ou exportação de CRM) e `estrategia_processamento` (extração direta, OCR ou processamento estruturado).

Assim, novos arquivos podem continuar no mesmo local de entrada e ser roteados de forma verificável, mesmo que tenham nomes pouco informativos ou extensões incorretas.
```

---

# ✅ Quest 2: O Portal de Entrada na AWS

## 2.1 Armazenamento dos arquivos brutos

Explique como os arquivos da pasta `raw/` seriam enviados e armazenados na AWS.

Serviços que você pode considerar:

- Amazon S3
- AWS IAM
- AWS KMS
- Amazon S3 Versioning
- Amazon S3 Lifecycle

**Sua resposta:**

```md
Esta seção descreve uma arquitetura proposta; nenhum recurso AWS foi criado.

Os três arquivos seriam enviados, por AWS CLI ou console da AWS com uma identidade autenticada, para um bucket Amazon S3 privado dedicado aos originais. Cada objeto usaria uma chave imutável e não dependente de subpastas locais, por exemplo `ingestion/<document_id>/<nome_original>`. O `document_id` seria derivado do SHA-256 do conteúdo; assim, o mesmo arquivo pode ser reconhecido mesmo se for reenviado com outro nome, e uma nova versão de conteúdo recebe outra identidade.

No upload seriam registrados como metadados o nome original, checksum SHA-256, tamanho, MIME declarado, horário de ingestão e origem. O Amazon S3 validaria o checksum fornecido no envio. S3 Block Public Access permaneceria habilitado, com acesso concedido somente a papéis IAM de ingestão e processamento pelo princípio do menor privilégio. Todo tráfego usaria HTTPS.

O bucket teria S3 Versioning habilitado e criptografia em repouso. Para esta POC acadêmica, SSE-S3 é a alternativa inicial mais simples e barata; SSE-KMS seria adotado se houver requisito de chave gerenciada pelo cliente, separação de funções ou auditoria de uso da chave, considerando custo e política KMS adicionais.

Um evento de criação do objeto seria entregue pelo Amazon EventBridge à AWS Step Functions. A máquina de estados chamaria uma AWS Lambda classificadora, que compararia extensão, MIME, assinatura do arquivo e, para PDF, presença de camada textual. O resultado definiria uma das rotas: PDF digital, imagem/PDF escaneado ou CSV estruturado.

Os originais ficariam no bucket canônico; resultados e estados de processamento iriam para outro bucket privado, em chaves como `processed/<document_id>/...`. Essa separação reduz o risco de uma rotina de transformação sobrescrever a fonte. Lifecycle seria usado apenas com regras explícitas: temporários e artefatos de falha poderiam expirar após um prazo aprovado, enquanto originais não teriam expiração automática. Versionamento e retenção também geram armazenamento, portanto versões antigas seriam monitoradas e só receberiam política de ciclo de vida depois de definido o período de retenção.
```

---

## 2.2 Preservação dos arquivos originais

Explique como garantir que os arquivos originais sejam mantidos intactos e rastreáveis.

**Sua resposta:**

```md
O arquivo recebido no bucket de originais seria a fonte canônica e nunca seria editado, convertido ou movido durante o processamento. Todas as correções, textos extraídos e formatos normalizados seriam novos objetos no bucket de processados, ligados ao original pelo mesmo `document_id`.

Os controles propostos são:

- S3 Versioning para que uma gravação acidental na mesma chave não destrua uma versão anterior;
- chaves contendo `document_id` e nome original, evitando colisões entre arquivos com nomes iguais;
- checksum SHA-256 validado no upload e registrado no manifesto de ingestão; não usar apenas o ETag como prova de integridade, pois ele nem sempre representa o MD5 do conteúdo;
- política de bucket negando acesso público, exigindo TLS e impedindo exclusão de objetos e versões originais para os papéis de processamento;
- permissões IAM separadas: o papel de ingestão grava originais, o orquestrador apenas lê originais e grava derivados, e usuários de consulta não acessam a zona bruta diretamente;
- criptografia em repouso e logs/auditoria das chamadas de API relevantes via AWS CloudTrail;
- manifesto em `processed/<document_id>/manifest.json` com `source_bucket`, `source_key`, `source_version_id`, checksum, nome original, formato detectado, timestamps, estado do processamento e chaves dos resultados.

S3 Object Lock em modo Governance seria uma opção quando houver uma exigência formal de retenção contra exclusão. Ele não é obrigatório para o pequeno laboratório, pois exige definição prévia de prazo e aumenta a complexidade operacional. Versioning, negação de exclusão aos papéis comuns e checksum já formam a base de baixo custo; Object Lock seria ativado somente com requisito de imutabilidade regulamentar ou corporativa.

Idempotência também protege os originais e os custos: antes de iniciar trabalho oneroso, a Step Functions consultaria o manifesto pelo `document_id`. Se a mesma versão já estiver concluída, encerraria como duplicata sem repetir OCR ou outras transformações.
```

---

## 2.3 Extração de texto dos documentos

Explique como cada tipo de arquivo seria processado.

Considere:

- PDFs escaneados;
- Imagens;
- PDFs digitais;
- Arquivos `.txt`;
- Arquivos `.docx`;
- Arquivos `.md`.

Serviços que você pode considerar:

- Amazon Textract
- AWS Lambda
- AWS Step Functions
- Amazon S3
- Amazon CloudWatch

**Sua resposta:**

```md
A AWS Step Functions coordenaria estados curtos e explícitos: validar, classificar, processar conforme o tipo, validar a saída e publicar o manifesto final. AWS Lambda seria usada para validações e transformações compatíveis com seus limites; o Amazon Textract seria chamado somente quando o conteúdo realmente exigir OCR.

**1. PDF digital observado no acervo**

`ata_reuniao_vendas_sa.pdf` possui cinco páginas e camada textual. Uma função Lambda executaria extração direta com uma biblioteca de parsing empacotada na própria função, preservando número da página, ordem das seções e, quando possível, relações de tabelas. Não passaria pelo Textract: OCR acrescentaria custo, latência e possibilidade de erro sem necessidade. A saída seria gravada como texto/JSON em `processed/<document_id>/document/`, junto com página, títulos e referência ao original. Cabeçalhos e rodapés repetidos poderiam ser marcados para normalização posterior, sem alterar a evidência bruta extraída.

**2. PNG digitalizado observado no acervo**

`ata_resultados_vendas_novos_dados.png` não possui camada textual. A Step Functions enviaria a imagem do S3 ao Amazon Textract com análise de documento, solicitando texto, tabelas e estruturas relevantes. Os blocos retornados — palavras, linhas, células, geometria, relacionamentos e confiança — seriam preservados em JSON. Isso permite manter a tabela de indicadores e localizar as anotações manuscritas “conferir CRM” e “ação prioritária” no contexto da página. Confianças abaixo do limite definido seriam sinalizadas para revisão humana; nenhum texto incerto seria silenciosamente tratado como fato.

Um PDF escaneado seguiria a mesma família de rota, mas usaria a operação assíncrona do Textract para documentos armazenados no S3 e um estado de espera/consulta na Step Functions até `SUCCEEDED` ou `FAILED`. Para uma imagem pequena e suportada, a operação síncrona pode reduzir estados; a escolha depende de formato, tamanho e quantidade de páginas.

**3. CSV estruturado observado no acervo**

`vendas_sa_dados_ficticios_laboratorio.csv` seria lido por Lambda como UTF-8 com BOM e vírgula, validando cabeçalho, 19 campos por registro, tipos e regras de preenchimento dependentes do status. As 240 linhas e as 19 colunas seriam preservadas. Uma cópia validada poderia ser convertida para Apache Parquet no S3 processado e registrada no AWS Glue Data Catalog para consultas com Amazon Athena. Não haveria OCR nem conversão indiscriminada de todas as linhas em texto para RAG; agregações de valores, status e regiões permaneceriam determinísticas.

**4. Outros formatos previstos, mas não presentes em `raw/`**

- `.txt` e `.md`: Lambda validaria codificação, tamanho e conteúdo e armazenaria texto normalizado com referência ao original.
- `.docx`: Lambda extrairia texto e estrutura do pacote Office Open XML, preservando títulos e tabelas; macros ou formatos inesperados seriam rejeitados pela validação.
- PDF sem camada textual: seria classificado como escaneado e enviado ao Textract, em vez de assumir que todo `.pdf` é digital.

As saídas só receberiam estado `SUCCEEDED` depois de validações mínimas: resultado não vazio para documentos, estrutura tabular esperada para o CSV, referência à versão original e manifesto completo. A escolha por S3, Lambda, Step Functions e Textract mantém a proposta serverless e pay-per-use. No volume atual — um PDF, um PNG e um CSV pequeno — não se justifica manter servidores ou um cluster de processamento; ainda assim, pricing regional e limites atuais seriam verificados antes de qualquer POC na Task 08.
```

---

## 2.4 Tratamento de falhas

Explique como sua solução identificaria e registraria erros de processamento.

**Sua resposta:**

```md
Cada execução usaria o `document_id` como chave de correlação em logs, métricas, manifesto e nomes de saída. A Step Functions registraria o estado de cada etapa e aplicaria `Retry` com espera exponencial apenas a falhas transitórias, como throttling ou indisponibilidade temporária. Erros de validação, formato não suportado, arquivo corrompido ou saída estruturalmente inválida não seriam repetidos automaticamente, evitando loops e custos desnecessários.

Todos os ramos teriam `Catch` para um estado comum de falha. Esse estado:

1. manteria o original intacto no bucket canônico;
2. removeria ou marcaria como incompletos somente artefatos temporários daquela execução;
3. gravaria um registro em `quarantine/<document_id>/error.json` no bucket de processados, contendo etapa, categoria do erro, horário, tentativa, `source_key`, `source_version_id` e mensagem sanitizada, sem credenciais nem conteúdo sensível desnecessário;
4. atualizaria o manifesto para `FAILED`, `PARTIAL` ou `REVIEW_REQUIRED`;
5. publicaria métricas e logs estruturados no Amazon CloudWatch.

A quarentena seria lógica: o arquivo original não seria movido nem apagado; o registro de falha apontaria para sua versão no S3. Após correção do código ou aprovação humana, uma nova execução usaria o mesmo `document_id`, criaria uma nova tentativa no manifesto e não sobrescreveria evidências anteriores.

Alarmes do CloudWatch acompanhariam execuções com falha, duração anormal, throttling, baixa confiança do Textract e volume inesperado. O histórico da Step Functions permitiria localizar o estado que falhou, enquanto o CloudTrail apoiaria a auditoria de chamadas aos serviços e alterações de configuração. Falha na entrega inicial do evento poderia ser direcionada pelo EventBridge a uma Amazon SQS dead-letter queue, com alarme para impedir perda silenciosa de ingestões.

Exemplos de tratamento específico:

- PDF digital sem texto extraído: reclassificar para revisão; somente enviar ao Textract se a inspeção confirmar que é escaneado.
- Textract com falha ou baixa confiança: preservar o JSON retornado, marcar `REVIEW_REQUIRED` e não publicar o texto como confiável.
- CSV com cabeçalho ou número de campos incorreto: bloquear a publicação analítica e registrar as linhas problemáticas sem alterar o CSV original.
- reenvio duplicado: detectar pelo checksum/`document_id` e encerrar sem repetir processamento pago.

Para controlar custo e complexidade, logs teriam retenção definida, alarmes seriam poucos e acionáveis, tentativas seriam limitadas e a quarentena teria Lifecycle após o período de investigação aprovado. Antes de qualquer implementação seria proposto um AWS Budget de US$ 5 — entendido como alerta, não como bloqueio automático — e todos os recursos seriam inventariados e incluídos no plano de cleanup.
```

---

# ✅ Quest 3: A Relíquia dos Metadados

## 3.1 Padronização dos textos processados

Explique como os textos extraídos seriam limpos, normalizados e preparados para consulta.

**Sua resposta:**

```md
A padronização produziria derivados consultáveis sem alterar nem substituir o arquivo original ou a extração bruta. Para cada `document_id`, o S3 processado manteria três camadas: `extracted/` com a saída fiel do parser ou do Textract, `normalized/` com conteúdo limpo e `metadata/` com manifesto e campos estruturados. Assim, uma transformação pode ser refeita e auditada contra a evidência anterior.

Para PDF digital e PNG com OCR, uma função AWS Lambda aplicaria regras determinísticas antes de qualquer enriquecimento por IA:

1. normalizar codificação para UTF-8 e quebras de linha para `\n`;
2. aplicar normalização Unicode sem remover acentos nem mudar o significado;
3. remover espaços excedentes e recompor palavras quebradas apenas quando a regra for inequívoca;
4. identificar cabeçalhos, rodapés e avisos repetidos, mantendo-os na extração bruta e marcando-os como ruído na versão de consulta;
5. preservar títulos, listas, parágrafos, tabelas e IDs como `D-001`, `A-001` e `R-01`, em vez de produzir um bloco de texto plano;
6. manter localizadores de origem em cada bloco: página e coordenadas/bloco no PDF ou na imagem;
7. preservar texto de baixa confiança do Textract com o respectivo `confidence` e estado `REVIEW_REQUIRED`, sem corrigi-lo silenciosamente;
8. detectar conteúdo repetido por hash do bloco, mas apenas marcar duplicidade; a remoção da cópia de consulta não apaga a evidência extraída.

O contrato normalizado proposto seria JSON versionado, por exemplo:

{
  "schema_version": "1.0",
  "document_id": "sha256:<hash>",
  "source": {
    "file": "nome-original.ext",
    "uri": "s3://bucket-raw/ingestion/<document_id>/nome-original.ext",
    "version_id": "<versao-s3>",
    "checksum_sha256": "<hash>"
  },
  "document": {
    "type": "ata_reuniao",
    "physical_format": "pdf_digital|imagem_digitalizada",
    "language": "pt-BR",
    "content": [
      {
        "block_id": "b-001",
        "kind": "heading|paragraph|table|handwriting",
        "text": "...",
        "source_locator": {"page": 1, "block_ids": ["..."]},
        "confidence": 99.0
      }
    ]
  },
  "metadata": {"...": "campos descritos na seção 3.2"},
  "processing": {
    "status": "SUCCEEDED|REVIEW_REQUIRED",
    "extraction_method": "direct_text|textract",
    "created_at": "ISO-8601 UTC",
    "updated_at": "ISO-8601 UTC"
  }
}

O CSV não seria submetido a essa limpeza textual linha por linha. Suas 19 colunas continuariam tipadas em uma tabela validada, preferencialmente convertida também para Parquet. Nomes originais de colunas e valores seriam preservados; uma camada de catálogo poderia registrar descrições, tipos e equivalências de busca sem alterar a fonte. Somente descrições ou observações selecionadas poderiam alimentar busca textual, sempre com `source_locator` apontando para o `oportunidade_id` e a linha lógica.
```

---

## 3.2 Metadados propostos

Defina quais metadados você extrairia de cada documento.

| Metadado | Por que ele é importante? |
|---|---|
| Nome do documento (`source_file`) | Preserva o nome recebido e permite ao usuário reconhecer a fonte citada. |
| Tipo do documento (`document_type`) | Distingue ata de reunião, exportação de CRM e futuros tipos; determina validação e experiência de consulta. |
| Data identificada (`event_date`) | Permite filtros cronológicos. Deve incluir valor, formato normalizado, localizador da evidência e eventual divergência, como a data e o subtítulo do PNG. |
| Tema principal (`main_topic`) | Apoia navegação e recuperação temática; quando inferido pelo Bedrock deve ser marcado como inferência. |
| Participantes (`participants`) | Permite encontrar reuniões por pessoa, cargo ou área, distinguindo nomes observados de grupos não individualizados. |
| Decisões tomadas (`decisions`) | Registra ID, texto, status e localizador da fonte para responder quais decisões foram aprovadas sem perder contexto. |
| Responsáveis (`owners`) | Liga pessoas às respectivas ações, e não apenas ao documento inteiro. |
| Próximos passos (`action_items`) | Estrutura ação, responsável, prazo, prioridade e status para pesquisa e acompanhamento. |
| Nível de confidencialidade (`confidentiality`) | Orienta autorização e exposição. O valor declarado no documento é `observed`; uma classificação sugerida pela IA é `inferred` e exige política ou revisão. |
| Caminho do arquivo original (`source_uri`) | Conecta o registro ao objeto canônico. Deve ser acompanhado por `source_version_id` e checksum, sem armazenar URL pré-assinada permanente. |
| Identificador (`document_id`) | Chave estável baseada no SHA-256 que relaciona original, derivados, metadados, execução e índice. |
| Formato e natureza (`physical_format`, `document_nature`) | Diferenciam PDF digital, imagem digitalizada e tabela estruturada e explicam o método de extração. |
| Localizador da evidência (`source_locator`) | Aponta página/bloco/coordenadas nos documentos ou `oportunidade_id`/linha lógica no CSV. |
| Projetos/campanhas (`projects_campaigns`) | Permite filtros como “Rota 120” e conecta atas a dimensões presentes no CRM. |
| Riscos (`risks`) | Estrutura descrição, probabilidade, impacto e resposta planejada quando esses campos existirem. |
| Pendências (`pending_items`) | Separa assuntos ainda abertos de decisões concluídas. |
| Indicadores (`metrics`) | Preserva nome, valor, unidade, meta, período e localizador, evitando números sem contexto. |
| Método de extração (`extraction_method`) | Informa se o campo veio de texto direto, Textract, parser tabular ou inferência do Bedrock. |
| Confiança (`confidence`) | Mantém confiança por campo/bloco, origem da pontuação e limite de revisão; não deve ser uma média opaca do documento inteiro. |
| Proveniência (`provenance`) | Classifica cada valor como `observed`, `deterministic_derived`, `ai_inferred` ou `human_validated`, com modelo/regra e localizador quando aplicável. |
| Timestamps (`ingested_at`, `processed_at`, `updated_at`) | Registram em ISO-8601 UTC quando cada etapa ocorreu e permitem auditoria e reprocessamento. |
| Versão do esquema (`schema_version`) | Permite evoluir o contrato sem interpretar registros antigos com regras novas. |

Adicione outros metadados, se necessário.

---

## 3.3 Uso de IA para enriquecimento dos documentos

Explique como o Amazon Bedrock poderia ajudar a identificar temas, decisões, responsáveis, pendências e resumos dos documentos.

**Sua resposta:**

```md
O Amazon Bedrock seria usado depois da extração determinística, principalmente nas atas, para sugerir resumo, temas, projetos, participantes, decisões, riscos, responsáveis e pendências. Ele não seria usado para OCR, para descobrir a estrutura do CSV nem para calcular agregações.

A Lambda enviaria ao modelo no Bedrock blocos com seus localizadores e uma instrução fechada: responder somente em JSON conforme um esquema, não completar campos ausentes, retornar `null` quando não houver evidência e citar os `block_id` que sustentam cada campo. Por exemplo, uma ação só seria aceita se a saída mantivesse separadamente texto, responsável, prazo e blocos de origem.

Outra Lambda validaria a resposta antes de publicá-la:

- JSON e tipos devem corresponder ao contrato versionado;
- todo fato inferido deve apontar para ao menos um bloco existente;
- datas, IDs e números seriam conferidos contra o texto de origem por regras determinísticas;
- nomes e decisões não presentes nos blocos seriam rejeitados;
- valores conflitantes ou de baixa confiança receberiam `REVIEW_REQUIRED`;
- campos aceitos seriam marcados `ai_inferred`, com identificador do modelo, versão do prompt, timestamp e confiança separada da confiança do OCR.

O texto observado nunca seria sobrescrito pela sugestão do modelo. Uma revisão humana poderia promover um valor para `human_validated`, registrando quem validou, quando e qual valor anterior foi revisado. Para o PNG, confiança do Textract e confiança/validação do enriquecimento seriam mantidas separadas: boa fluência do resumo não corrige OCR incerto.

Para o CSV, Bedrock poderia ajudar apenas a interpretar perguntas em linguagem natural ou resumir resultados já calculados por consulta determinística. Contagens, somas, médias, taxas e filtros seriam executados com Amazon Athena sobre dados catalogados, nunca estimados pelo modelo a partir de linhas transformadas em texto.

Também seriam aplicadas salvaguardas de custo e qualidade: processar apenas blocos relevantes, limitar tamanho de entrada e saída, usar baixa aleatoriedade quando suportada pelo modelo escolhido, versionar prompts e amostrar resultados para avaliação humana. Modelo, preço regional, limites e disponibilidade seriam confirmados antes da Task 08. Nesta etapa, o uso do Bedrock é apenas proposto; nenhuma chamada foi executada.
```

---

## 3.4 Armazenamento dos metadados

Explique onde os metadados seriam armazenados e como seriam conectados aos documentos originais.

Serviços que você pode considerar:

- Amazon S3
- Amazon DynamoDB
- AWS Glue Data Catalog
- Amazon Bedrock Knowledge Bases

**Sua resposta:**

```md
Cada serviço teria uma função específica, evitando duplicar o mesmo dado como fonte de verdade em vários lugares:

**Amazon S3 — conteúdo completo e histórico**

- o bucket `raw` manteria o objeto original e sua versão;
- o bucket de processados manteria extração bruta, JSON normalizado, texto de consulta, saída integral do Textract, metadados completos e manifestos versionados;
- as chaves seguiriam `processed/<document_id>/<schema_version>/...`;
- cada registro incluiria `source_uri`, `source_version_id` e `checksum_sha256`, permitindo reproduzir a ligação exata com o original.

**Amazon DynamoDB — catálogo operacional e filtros de baixa latência**

- um item principal por `document_id` manteria tipo, data, tema, confidencialidade, estado, timestamps e ponteiros S3;
- itens relacionados ou uma projeção controlada poderiam representar decisões, ações e participantes quando for necessário consultar por esses atributos;
- índices secundários seriam criados apenas para padrões reais de acesso, por exemplo tipo+data ou status+data, evitando custo e complexidade prematuros;
- o DynamoDB não armazenaria o texto integral nem substituiria o S3 como fonte dos derivados.

**AWS Glue Data Catalog — esquema analítico do CSV**

- catalogaria as 19 colunas da versão validada/Parquet, com tipos para datas, percentuais, valores monetários e duração;
- manteria parâmetros de tabela com localização S3, `document_id`, checksum e versão do esquema;
- permitiria ao Amazon Athena executar filtros e agregações determinísticas sem transformar as 240 oportunidades em texto corrido.

**Amazon Bedrock Knowledge Bases — consumo posterior para recuperação documental**

- receberia somente o conteúdo documental normalizado e os metadados de filtro aprovados;
- cada trecho manteria `document_id`, `source_uri`, versão e localizador de página/bloco para que a resposta possa citar a fonte;
- dados tabulares do CRM permaneceriam prioritariamente no fluxo Glue/Athena; somente conteúdo textual selecionado entraria na base de conhecimento quando houver caso de uso semântico claro.

O vínculo ponta a ponta seria:

`objeto S3 raw + version_id + checksum` → `document_id` → `manifesto/JSON no S3 processado` → `item DynamoDB ou tabela Glue` → `trecho recuperado e fonte`.

Uma URL pré-assinada poderia ser gerada no momento da consulta para um usuário autorizado, mas não seria persistida como metadado porque expira. IAM aplicaria menor privilégio, criptografia protegeria os dados em repouso e CloudTrail/CloudWatch apoiariam auditoria e operação. Essa distribuição usa serviços serverless/pay-per-use e pode começar somente com S3 e Glue para o pequeno acervo; DynamoDB e Knowledge Bases entram quando filtros interativos e busca semântica justificarem seu custo.
```

---

# ✅ Quest 4: O Oráculo da Wiki Inteligente

## 4.1 Estratégia de indexação

Explique como os documentos seriam divididos em trechos menores e preparados para busca semântica.

**Sua resposta:**

```md
A indexação usaria somente os documentos normalizados e aprovados no S3 processado; arquivos em `FAILED`, `PARTIAL` ou `REVIEW_REQUIRED` não seriam publicados automaticamente. Cada trecho manteria uma ligação imutável com `document_id`, `source_uri`, `source_version_id`, checksum e localizador de página/bloco.

Para as duas atas, o chunking seria orientado pela estrutura:

- identificação, pauta, resumo executivo e cada subseção temática formariam unidades lógicas;
- uma decisão, ação ou risco não seria separada de seu ID, descrição, responsável, prazo, status, probabilidade ou impacto;
- tabelas pequenas seriam preservadas inteiras ou transformadas em registros autossuficientes que repetem o cabeçalho necessário, por exemplo “Ação A-001 | responsável | prazo | status”;
- a continuação do plano de ação do PDF entre as páginas 3 e 4 seria reunida pela seção, mantendo os localizadores das duas páginas;
- anotações manuscritas do PNG seriam associadas ao bloco próximo, mas continuariam identificadas como handwriting e com confiança própria;
- cabeçalhos e rodapés repetidos não seriam indexados como chunks independentes.

Como ponto de partida, trechos narrativos teriam aproximadamente 300–500 tokens, com sobreposição de 10%–15% apenas entre blocos textuais contíguos. Esses valores não são universais: seriam ajustados com um conjunto de perguntas de teste, avaliando recuperação, contexto, duplicidade e custo. Trechos muito grandes diluem a relevância; trechos muito pequenos podem separar decisão, responsável e prazo.

Cada chunk teria um `chunk_id` determinístico e metadados compactos de filtro: tipo, data, tema/campanha, confidencialidade, página inicial, método de extração, estado de revisão e grupos autorizados. Metadados completos permaneceriam no S3/DynamoDB; o índice receberia somente os campos necessários à recuperação.

O CSV seguiria outra estratégia. As 240 oportunidades permaneceriam em Parquet/Glue/Athena para cálculos. Apenas o dicionário das 19 colunas, descrições de categorias e observações textuais que tenham caso de uso semântico poderiam gerar chunks, sempre ligados a `oportunidade_id` ou ao esquema. Não seriam criados 240 parágrafos artificiais para pedir ao RAG que some valores.

O Amazon Bedrock Knowledge Bases oferece chunking fixo, hierárquico, semântico ou sem chunking. Para a primeira versão com S3 Vectors, eu usaria a preparação lógica acima combinada com chunking fixo controlado, porque é simples, previsível e não adiciona chamadas de modelo apenas para decidir limites. Chunking semântico seria testado somente se a avaliação demonstrar ganho suficiente para justificar custo adicional; o hierárquico não seria a escolha inicial com S3 Vectors devido ao consumo de metadados das relações pai-filho.
```

---

## 4.2 Busca semântica e base vetorial

Explique como embeddings seriam gerados e onde seriam armazenados.

Serviços que você pode considerar:

- Amazon Bedrock Knowledge Bases
- Amazon OpenSearch Serverless
- Amazon Aurora PostgreSQL com pgvector
- Amazon S3 Vectors
- Modelos de embeddings no Amazon Bedrock

**Sua resposta:**

```md
O Amazon Bedrock Knowledge Bases sincronizaria a fonte documental no S3 processado, criaria embeddings dos chunks com um modelo de embeddings disponível na região e gravaria os vetores em um vector store AWS. A primeira opção para esta POC acadêmica seria **Amazon S3 Vectors**, sem infraestrutura provisionada e adequado a um acervo pequeno com consultas pouco frequentes.

Como candidato inicial de embeddings, eu avaliaria o Amazon Titan Text Embeddings V2, que possui suporte multilíngue incluindo português. O mesmo modelo e a mesma dimensão seriam usados na ingestão e na consulta. Antes da POC seriam confirmados disponibilidade conjunta de Bedrock, modelo e S3 Vectors na região escolhida, dimensões aceitas pelo índice, qualidade em português e pricing atual; nenhum modelo ou região foi provisionado nesta proposta.

Fluxo semântico:

1. Knowledge Bases lê um chunk aprovado no S3.
2. O modelo do Bedrock converte o texto em um vetor numérico.
3. S3 Vectors armazena vetor, chave e metadados compactos.
4. A pergunta recebe embedding com o mesmo modelo.
5. A busca por similaridade retorna os chunks mais próximos, já restringidos por filtros autorizados como tipo, data, campanha e confidencialidade.
6. Opcionalmente, um reranker disponível no Bedrock pode reordenar poucos candidatos; ele só seria adicionado se a avaliação mostrar ganho maior que custo e latência.

S3 Vectors com Knowledge Bases oferece busca **semântica**, não busca híbrida nativa entre vetor e texto. Portanto, identificadores e termos exatos, como `D-004`, `A-001` ou `Rota 120`, também seriam tratados por metadados/filtros e por uma busca exata na camada operacional. Já números e agregações do CRM seriam encaminhados ao Athena. Essa composição evita prometer uma capacidade híbrida que o vector store escolhido não fornece.

Os metadados no índice seriam deliberadamente pequenos. Na integração atual entre Knowledge Bases e S3 Vectors, a AWS documenta limite de até 1 KB de metadados customizados e 35 chaves por vetor. O JSON completo não seria copiado para cada vetor: ele permaneceria no S3, referenciado por `document_id` e `chunk_id`.

Alternativas:

- **Amazon OpenSearch Serverless:** seria considerado se surgirem alta frequência de consultas, busca híbrida nativa, pesquisa lexical avançada, facetas ou baixa latência mais exigente. Não é a primeira escolha devido à maior complexidade e ao risco de cobrança contínua.
- **Amazon Aurora PostgreSQL com pgvector:** faria sentido se já existisse um banco relacional Aurora ou se vetores e relações transacionais precisassem compartilhar consultas. Criá-lo apenas para três arquivos não se justifica.
- **S3 Vectors:** favorece armazenamento vetorial econômico, elasticidade e operação serverless para o padrão acadêmico de baixa frequência, aceitando a limitação de busca apenas semântica.

Referências técnicas verificadas em 20/08/2026: documentação oficial da AWS sobre S3 Vectors com Knowledge Bases, configuração de vector stores, chunking e modelos suportados do Bedrock. Pricing e disponibilidade regional continuam sujeitos a mudança e serão revistos antes da Task 08.
```

---

## 4.3 Geração de respostas com IA

Explique como a Wiki responderia perguntas em linguagem natural com base nos documentos originais.

Considere explicar:

- Como a pergunta do usuário seria recebida;
- Como os trechos relevantes seriam recuperados;
- Como o Amazon Bedrock geraria a resposta;
- Como a resposta indicaria as fontes utilizadas.

**Sua resposta:**

```md
A pergunta chegaria autenticada à API e receberia um `query_id`. Uma Lambda aplicaria limites de tamanho, removeria instruções de controle não permitidas e classificaria a intenção em três rotas: documental, analítica do CRM ou mista.

**Pergunta documental — RAG**

1. A Lambda extrairia filtros explícitos, como data, campanha, tipo ou área, e acrescentaria obrigatoriamente os filtros de autorização do usuário.
2. A operação `Retrieve` do Amazon Bedrock Knowledge Bases buscaria os chunks semanticamente relevantes. Para controle maior, a solução desacoplaria recuperação e geração, em vez de entregar diretamente toda a decisão ao modelo.
3. A aplicação validaria se os resultados pertencem ao escopo autorizado, removeria duplicados e montaria contexto com texto, `chunk_id`, página/bloco, arquivo e versão.
4. Um modelo de geração no Amazon Bedrock receberia somente a pergunta, instruções de resposta e os trechos aprovados. O prompt exigiria responder apenas com o contexto, separar fato de inferência e não preencher lacunas.
5. Cada afirmação da resposta seria ligada aos chunks usados. A interface mostraria nome do documento, data, página ou região da imagem e um link temporário autorizado para a versão original.

A operação `RetrieveAndGenerate` do Knowledge Bases já pode retornar citações associadas aos chunks recuperados. Mesmo assim, o backend validaria e transformaria essas referências para o `source_uri` canônico; o usuário não receberia como única fonte o caminho técnico de um chunk derivado.

**Pergunta analítica sobre o CSV — SQL/Amazon Athena**

Perguntas como “quantas oportunidades foram perdidas por região?” ou “qual o valor líquido ganho por campanha?” não passariam pelo vetor. A Lambda mapearia a intenção para consultas SQL somente leitura sobre a tabela do AWS Glue Data Catalog consultada pelo Amazon Athena. O SQL seria limitado a tabelas e colunas permitidas, validado antes da execução, executado em workgroup com limite de bytes escaneados e retornaria também filtros, período e horário da consulta. Para o acervo pequeno em Parquet, isso produz resultado determinístico e reduz leitura.

O Bedrock poderia transformar o resultado tabular já calculado em uma explicação, mas números exibidos viriam da resposta do Athena. A fonte seria citada como nome da exportação, versão/checksum, consulta ou `query_execution_id` interno e parâmetros usados; o identificador AWS não seria exposto publicamente sem necessidade.

**Pergunta mista — estratégia híbrida RAG + SQL**

Para “o que a ata decidiu sobre a Rota 120 e como estão as oportunidades da campanha?”, o backend executaria as duas rotas em paralelo lógico: Knowledge Bases recuperaria as decisões documentais e Athena calcularia o recorte do CRM. O modelo receberia dois blocos claramente rotulados e a resposta separaria “decisões registradas” de “resultado calculado no CRM”, com fontes próprias.

**Ausência ou conflito de evidência**

Se não houver chunks suficientemente relevantes, se o Athena não retornar dados, se a fonte estiver fora da autorização ou se fontes conflitarem sem resolução, a Wiki responderia que não há evidência suficiente e indicaria quais filtros foram aplicados. Ela não completaria nomes, datas ou números. O limiar de relevância seria calibrado com testes, não escolhido como garantia arbitrária. Conteúdo `REVIEW_REQUIRED` seria identificado ou excluído conforme criticidade.

O histórico de conversa fornecido ao modelo seria curto e controlado; perguntas anteriores não poderiam alterar filtros de autorização nem substituir evidências. Guardrails do Amazon Bedrock poderiam filtrar entrada e saída, mas não substituem validação de acesso e de fontes recuperadas.
```

---

## 4.4 Interface de consulta

Proponha como os usuários acessariam essa Wiki Inteligente.

Serviços que você pode considerar:

- Amazon Q Business
- AWS Amplify
- Amazon API Gateway
- AWS Lambda
- Amazon Cognito

**Sua resposta:**

```md
A interface proposta seria uma aplicação web interna e serverless:

`Usuário → frontend no AWS Amplify Hosting → Amazon Cognito → Amazon API Gateway → AWS Lambda → Knowledge Bases/Athena → Amazon Bedrock`

- **AWS Amplify Hosting** publicaria a interface estática com campo de pergunta, filtros, histórico da sessão, resposta, fontes expansíveis e opção de feedback. O frontend não teria credenciais AWS permanentes.
- **Amazon Cognito User Pools** autenticaria usuários. O API Gateway validaria o token e os escopos; grupos ou claims representariam áreas e níveis de acesso.
- **Amazon API Gateway** exporia endpoints como `/query`, `/feedback` e `/sources/{document_id}`, com throttling, limites de payload e autorização obrigatória.
- **AWS Lambda** faria roteamento documental/analítico, aplicaria filtros de autorização, chamaria serviços internos, validaria citações e devolveria um contrato de resposta consistente.
- **S3** forneceria links pré-assinados de curta duração somente após nova verificação de autorização, nunca URLs públicas permanentes.

Uma resposta mostraria: resumo; tipo de resposta (`documental`, `analítica` ou `mista`); decisões/pessoas/datas relevantes; fontes numeradas; nível de revisão; e aviso de insuficiência quando aplicável. O usuário poderia abrir a página do PDF ou a imagem original e ver parâmetros da consulta do CRM.

Para reduzir custo e escopo, a primeira interface teria somente consulta e feedback, sem painel administrativo complexo ou agente autônomo. Amazon Q Business poderia ser avaliado como alternativa gerenciada quando conectores corporativos, permissões e experiência pronta justificarem seu modelo de custo; para este laboratório de três arquivos, API Gateway + Lambda + Cognito oferecem maior transparência sobre a rota híbrida e as citações.

Esta é uma proposta de interface. Nenhum endpoint, User Pool, aplicação Amplify ou função foi criado.
```

---

## 4.5 Segurança, auditoria e monitoramento

Explique como controlar acesso, proteger dados, auditar consultas e monitorar custos, erros e qualidade das respostas.

Serviços que você pode considerar:

- AWS IAM
- AWS KMS
- Amazon Cognito
- AWS CloudTrail
- Amazon CloudWatch
- Amazon Macie
- AWS Cost Explorer

**Sua resposta:**

```md
**Identidade e autorização**

- Cognito exigiria autenticação; MFA e federação com o provedor corporativo seriam habilitadas conforme o risco.
- API Gateway validaria token e escopos. A Lambda converteria grupos/claims em filtros de metadados antes da recuperação, aplicando controle por departamento e confidencialidade.
- A autorização seria repetida ao abrir a fonte; conhecer um `document_id` não daria acesso ao objeto.
- IAM usaria papéis separados e menor privilégio para API, Knowledge Bases, Athena e ingestão. O papel da consulta não poderia alterar `raw/`, índice ou catálogo.
- S3 Block Public Access, TLS e criptografia em repouso permaneceriam obrigatórios. SSE-KMS seria usada quando chave gerenciada pelo cliente, separação de funções ou trilha da chave justificarem custo e operação adicionais.
- Segredos, tokens, prompts completos e conteúdo sensível não seriam gravados no Git nem em logs.

**Defesa da camada de IA**

- instruções fixas delimitariam pergunta e evidências, reduzindo o efeito de prompt injection presente em documentos;
- somente chunks autorizados e aprovados entrariam no contexto;
- citações seriam verificadas contra os chunks e a versão original antes da resposta;
- Amazon Bedrock Guardrails poderia aplicar políticas à entrada e à saída gerada, mas referências recuperadas também precisariam de validação própria;
- respostas sem evidência suficiente seriam recusadas como fato, e baixa confiança/conflito seria encaminhado à revisão humana;
- nenhuma ação operacional seria executada pelo chat; a Wiki seria somente consulta nesta fase.

**Auditoria e privacidade**

- AWS CloudTrail registraria chamadas de controle e, quando configurado para os recursos relevantes, eventos de dados necessários à auditoria;
- CloudWatch receberia logs estruturados com `query_id`, usuário pseudonimizado, rota, latência, modelos/versões, IDs de fontes e status, evitando texto integral por padrão;
- retenção e acesso aos logs seriam definidos; dados de pergunta só seriam armazenados quando houver finalidade, prazo e autorização claros;
- Amazon Macie seria considerado apenas se o acervo real exigir descoberta contínua de dados sensíveis no S3. Para os três arquivos fictícios, ativá-lo sem caso de uso adicionaria custo sem benefício demonstrado.

**Monitoramento operacional e de qualidade**

- métricas: erros e throttling por serviço, latência ponta a ponta, tempo de Athena, bytes escaneados, tokens de entrada/saída, quantidade de chunks, consultas sem resultado e custo estimado;
- qualidade: precisão de recuperação em perguntas de teste, cobertura e validade de citações, respostas sem evidência, divergência entre números citados e Athena, taxa de `REVIEW_REQUIRED` e feedback do usuário;
- alarmes: aumento de erros, gasto anormal, ausência de citações, latência acima do objetivo e volume inesperado;
- um conjunto versionado de perguntas esperadas sobre decisões, responsáveis, riscos e CRM seria executado após mudanças em chunking, embeddings, prompt ou modelo. A promoção exigiria revisão humana de uma amostra, porque fluência não prova correção.

**FinOps e limites**

- AWS Budgets com alerta de US$ 5, lembrando que o alerta não interrompe automaticamente os gastos;
- Cost Explorer e tags por projeto/ambiente; AWS Cost Anomaly Detection se houver uso continuado;
- S3 Vectors como primeira opção de baixa frequência; OpenSearch Serverless somente com requisito medido;
- limites de tokens, resultados recuperados, concorrência Lambda, tentativas e bytes escaneados no workgroup Athena;
- sincronização da Knowledge Base e chamadas Bedrock somente quando necessárias; logs e temporários com retenção definida;
- inventário de recursos e cleanup obrigatório em eventual POC.

Antes da Task 08 seriam confirmados região, preços atuais, modelo, limites, recursos a criar, risco de cobrança e plano de exclusão. A POC continuaria bloqueada até autorização explícita do usuário.
```

---

# 🧩 Arquitetura Final da Solução

Agora reúna tudo em uma visão única.

## 1. Visão geral

Explique em poucas linhas a ideia central da sua arquitetura.

**Sua resposta:**

```md
Esta é uma arquitetura AWS proposta, serverless e pay-per-use; nenhum componente foi implementado até esta etapa.

Os originais entram em um bucket Amazon S3 canônico e privado, recebem `document_id` derivado do SHA-256 e são orquestrados por EventBridge, Step Functions e Lambda. O roteamento respeita a natureza observada: o PDF digital usa extração direta, o PNG usa Amazon Textract e o CSV permanece estruturado em S3/Glue/Athena. Derivados, metadados, confiança e proveniência ficam separados dos originais.

As atas normalizadas alimentam Amazon Bedrock Knowledge Bases, embeddings do Bedrock e Amazon S3 Vectors para recuperação semântica. O CRM atende filtros e agregações com SQL no Amazon Athena. Uma camada Lambda escolhe RAG, SQL ou ambas as rotas e devolve somente respostas fundamentadas, com fonte documental ou resultado analítico rastreável. Amplify, Cognito e API Gateway compõem a interface segura; IAM, criptografia, CloudWatch, CloudTrail e guardrails de custo atravessam todo o fluxo.
```

---

## 2. Serviços AWS utilizados

| Serviço AWS | Por que existe | Quem chama | Entrada | Saída | Alternativa mais simples/barata ou condição de uso |
|---|---|---|---|---|---|
| Amazon S3 | Preservar originais e armazenar extrações, normalizados, Parquet, manifestos e fontes da Knowledge Base. | Ingestão, Lambda, Textract, Glue, Athena e Bedrock. | PDF, PNG, CSV e derivados. | Objetos versionados com checksum e ponteiros rastreáveis. | Serviço-base indispensável; começar com SSE-S3 e poucos buckets/prefixos. SSE-KMS apenas quando o controle da chave justificar. |
| Amazon EventBridge | Entregar eventos de novos objetos à orquestração sem acoplamento direto. | Eventos do S3. | Evento `Object Created`. | Início da execução Step Functions. | Para uma POC manual, iniciar a Step Functions pela CLI/console e omitir EventBridge. |
| Amazon SQS (DLQ) | Reter eventos que o EventBridge não conseguiu entregar. | Regra do EventBridge em falha. | Evento não entregue. | Mensagem para diagnóstico/reprocessamento. | Omitir no teste manual; usar antes de ingestão automática para evitar perda silenciosa. |
| AWS Step Functions | Tornar classificação, ramificações, retries e falhas visíveis e auditáveis. | EventBridge ou operador autorizado. | Referência S3, versão, checksum e `document_id`. | Estados, chamadas de processamento e manifesto final. | Uma única Lambda é mais barata para um teste mínimo, mas perde clareza e pode estourar duração em OCR assíncrono. |
| AWS Lambda | Validar/classificar arquivos, extrair PDF digital, tratar CSV, normalizar, validar IA, rotear perguntas e compor respostas. | Step Functions e API Gateway. | Objetos S3, eventos, perguntas e resultados AWS. | Texto/JSON/Parquet, metadados, SQL validado e resposta com fontes. | Funções pequenas sob demanda; não usar ECS/EC2 enquanto limites de Lambda forem suficientes. |
| Amazon Textract | Extrair texto, tabela, geometria, handwriting e confiança de imagem/PDF escaneado. | Step Functions por SDK/Lambda. | PNG observado ou futuro PDF sem camada textual no S3. | Blocos JSON com relações e confiança. | Não usar no PDF digital; não existe OCR mais barato permitido fora da AWS neste desafio. Revisão humana cobre baixa confiança. |
| Amazon Bedrock | Fornecer embeddings, enriquecimento controlado e geração de resposta fundamentada. | Knowledge Bases e Lambda. | Chunks, pergunta, evidências e esquema/prompt versionado. | Vetores, metadados sugeridos e texto de resposta. | Regras Lambda cobrem extrações determinísticas; chamar modelo apenas quando IA agrega valor e limitar tokens. |
| Amazon Bedrock Knowledge Bases | Gerenciar sincronização, recuperação semântica, filtros e citações dos documentos. | Job de ingestão e Lambda de consulta. | Textos normalizados no S3 e pergunta/filtros. | Chunks relevantes com metadados e referências. | Para três arquivos, uma busca textual simples seria mais barata, mas não demonstra RAG semântico; KB entra somente na fase autorizada. |
| Amazon Titan Text Embeddings V2 no Bedrock | Gerar representação vetorial com suporte a português para documentos e perguntas. | Knowledge Bases. | Chunks e consulta em texto. | Vetores de mesma dimensão. | Validar qualidade/região/preço; outro modelo de embeddings disponível no Bedrock só entra após avaliação comparável. |
| Amazon S3 Vectors | Armazenar e consultar vetores sem provisionar cluster, adequado à baixa frequência. | Bedrock Knowledge Bases. | Vetores e metadados compactos. | Vizinhos semânticos filtrados. | Busca apenas semântica nessa integração; migrar para OpenSearch Serverless somente se busca híbrida, facetas, QPS ou latência medidos justificarem. |
| AWS Glue Data Catalog | Registrar esquema e localização do CRM validado/Parquet. | Pipeline e Athena. | 19 colunas, tipos e localização S3. | Tabela catalogada. | Definir tabela manualmente na POC; crawler só se variação de esquema/volume justificar. |
| Amazon Athena | Responder filtros, contagens, somas e médias do CRM de forma determinística. | Lambda de consulta. | SQL somente leitura sobre tabela Glue. | Resultado tabular, estatísticas e ID de execução. | Para 240 linhas, Lambda poderia calcular localmente; Athena demonstra escala e governança, com Parquet e limite de bytes. |
| Amazon DynamoDB | Manter estado operacional e metadados de filtro de baixa latência. | Pipeline e Lambda de consulta. | `document_id`, estado, datas, tipo e ponteiros S3. | Item/índices consultáveis. | Opcional no acervo mínimo: ler manifestos do S3 enquanto latência e padrões de consulta permitirem. |
| Amazon API Gateway | Expor API autenticada com throttling e limites. | Frontend Amplify. | Token Cognito, pergunta, filtros e feedback. | Requisição autorizada para Lambda e resposta HTTP. | Lambda Function URL reduziria componentes, mas exigiria desenho equivalente de autenticação/limites; API Gateway é preferível para governança. |
| Amazon Cognito | Autenticar usuários e fornecer grupos/claims para autorização. | Frontend e API Gateway. | Credenciais/federação. | Tokens com identidade e escopos. | Para demonstração isolada, IAM poderia autenticar operadores técnicos; Cognito é necessário para experiência de usuário final. |
| AWS Amplify Hosting | Hospedar o frontend interno sem servidores. | Usuário final e pipeline de publicação. | Aplicação web estática. | Interface de consulta, fontes e feedback. | O console/API pode servir à POC técnica; criar frontend somente quando fizer parte do aceite. |
| AWS IAM | Aplicar menor privilégio entre pessoas e serviços. | Todos os serviços AWS. | Políticas, papéis e contexto da chamada. | Decisão de permitir/negar. | Não há alternativa aceitável; evitar políticas amplas e separar ingestão, consulta e administração. |
| AWS KMS | Oferecer chaves gerenciadas pelo cliente quando houver requisito de separação/auditoria. | S3, S3 Vectors e serviços integrados. | Chave e contexto de criptografia. | Dados criptografados e eventos de uso da chave. | SSE-S3 é a base mais simples/barata; KMS é condicional, não decoração arquitetural. |
| Amazon CloudWatch | Centralizar logs, métricas, alarmes e painéis operacionais/qualidade. | Lambda, Step Functions, API Gateway e serviços integrados. | Logs e métricas sanitizados. | Alarmes, consultas e dashboards. | Usar retenção curta e poucas métricas acionáveis na POC para controlar custo. |
| AWS CloudTrail | Auditar chamadas e mudanças de configuração relevantes. | Plano de controle da conta e eventos de dados configurados. | Atividade de API. | Trilha de auditoria. | Usar trilha já existente da organização quando disponível; habilitar eventos de dados seletivamente devido a volume/custo. |
| AWS Budgets e Cost Explorer | Alertar e analisar gasto antes que a POC se expanda. | Conta/equipe responsável. | Uso faturado, orçamento de US$ 5 e tags. | Alertas e relatórios de custo. | Budget é alerta, não bloqueio; limites de serviço e cleanup continuam necessários. |

Adicione, remova ou ajuste os serviços conforme sua proposta.

---

## 3. Fluxo de dados de ponta a ponta

Descreva o caminho dos dados desde a pasta `raw/` até a Wiki Inteligente.

```md
Exemplo de estrutura:

1. Arquivos estão inicialmente na pasta raw/
2. Arquivos são enviados para o Amazon S3
3. Documentos escaneados passam pelo Amazon Textract
4. Arquivos digitais têm seus textos extraídos
5. Textos são limpos e padronizados
6. Metadados são extraídos
7. Conteúdos são indexados em uma base pesquisável
8. Usuário pesquisa na Wiki
9. IA responde com base nos documentos originais
```

**Sua resposta:**

```md
**Ingestão e processamento**

1. Os três arquivos locais permanecem intactos em `raw/` e são enviados por identidade autorizada para o bucket S3 canônico.
2. O upload valida SHA-256; a chave inclui `document_id`; S3 Versioning, Block Public Access, TLS e criptografia protegem o original.
3. Um evento S3 chega ao EventBridge e inicia a Step Functions. Uma DLQ SQS recebe falhas de entrega do evento.
4. Lambda verifica assinatura, MIME, extensão, camada textual, esquema e duplicidade. O manifesto registra versão S3, checksum e estado.
5. O PDF de 5 páginas segue extração direta em Lambda, preservando páginas, seções, tabelas e IDs; não usa OCR.
6. O PNG digitalizado segue Amazon Textract para texto, tabela, handwriting, geometria e confiança. Baixa confiança vira `REVIEW_REQUIRED`.
7. O CSV com 240 registros e 19 colunas segue validação estruturada; Lambda preserva tipos e produz CSV validado/Parquet no S3.
8. Erros transitórios recebem retries limitados; erros definitivos geram manifesto em quarentena lógica sem mover ou alterar o original.

**Normalização, metadados e indexação**

9. Lambda cria JSON normalizado versionado para as atas, mantendo extração bruta, localizadores e proveniência.
10. Bedrock pode sugerir temas, decisões e pendências em JSON; outra Lambda valida evidências. Inferência não substitui texto observado.
11. S3 guarda conteúdo/manifesto completo; DynamoDB, quando necessário, guarda estado e filtros; Glue cataloga o CRM/Parquet.
12. Somente documentos aprovados são divididos por seções, decisões, ações, riscos e tabelas, com chunks em torno de 300–500 tokens como baseline avaliado.
13. Bedrock Knowledge Bases sincroniza os chunks; Titan Text Embeddings V2 cria vetores; S3 Vectors os armazena com metadados compactos.

**Consulta e resposta**

14. O usuário acessa o frontend no Amplify, autentica-se no Cognito e envia pergunta/filtros ao API Gateway.
15. Lambda valida token, escopo, tamanho e intenção e aplica filtros de autorização antes de buscar qualquer fonte.
16. Pergunta documental usa Knowledge Bases `Retrieve`; os chunks autorizados alimentam o modelo de geração do Bedrock.
17. Pergunta analítica usa SQL validado e somente leitura no Athena, com workgroup e limite de bytes sobre a tabela Glue.
18. Pergunta mista executa RAG e SQL e mantém os dois conjuntos de evidência rotulados.
19. Lambda valida citações e números e devolve resumo, fontes, páginas/blocos ou parâmetros analíticos. Sem evidência suficiente, a resposta declara a limitação.
20. Ao abrir uma fonte, a autorização é verificada novamente e S3 gera link pré-assinado curto para a versão original.

**Operação**

21. CloudWatch monitora erros, latência, tokens, bytes do Athena, ausência de fontes, baixa confiança e feedback; CloudTrail apoia auditoria.
22. IAM limita cada papel; SSE-S3 é o padrão econômico e KMS entra sob requisito; Budgets alerta em US$ 5 e o inventário orienta cleanup.
```

---

## 4. Diagrama textual da arquitetura

Crie um diagrama simples usando texto.

```md
Exemplo:

raw/ → Amazon S3 → Lambda/Step Functions → Textract → S3 Processado → Bedrock Knowledge Bases → Interface de Consulta → Usuário Final
```

**Sua resposta:**

```md
INGESTÃO
raw/ (somente leitura)
  → Amazon S3 Raw (Versioning + checksum + criptografia)
  → EventBridge → Step Functions → Lambda classifica
      ├─ PDF digital → Lambda extrai texto sem OCR ──────────────┐
      ├─ PNG/PDF escaneado → Amazon Textract ───────────────────┤
      └─ CSV → Lambda valida → S3 Parquet → Glue → Athena ──────┼─┐
                                                                 │ │
CONHECIMENTO DOCUMENTAL                                          │ │
S3 Processed ← normalização + metadados + proveniência ←─────────┘ │
  → Bedrock Knowledge Bases → Titan Embeddings → S3 Vectors → RAG │
                                                                   │
CONSULTA                                                           │
Usuário → Amplify → Cognito → API Gateway → Lambda roteadora ──────┤
                                      ├─ documentos → RAG ─────────┤
                                      └─ CRM → Athena ─────────────┤
                                                                   ↓
                         resposta fundamentada + fontes → Usuário

TRANSVERSAL
IAM + S3 Block Public Access + SSE-S3/KMS condicional
CloudWatch + CloudTrail + Budgets/Cost Explorer
```

---

## 5. Riscos e limitações

Liste possíveis desafios da sua solução.

```md
Exemplo:
- Documentos ilegíveis podem prejudicar a extração de texto.
- OCR pode gerar erros em documentos com baixa qualidade.
- Custos podem aumentar conforme o volume de documentos.
- Metadados inferidos por IA podem precisar de validação humana.
- Respostas geradas por IA devem sempre referenciar documentos de origem.
```

**Sua resposta:**

```md
- **OCR imperfeito:** ruído, sombra, inclinação, tabela e handwriting do PNG podem produzir erros. Mitigação: preservar JSON/geometria/confiança do Textract, limiar calibrado e revisão humana.
- **Perda de estrutura no PDF:** extração linear pode separar células ou a tabela entre páginas 3 e 4. Mitigação: parser consciente de página/seção e testes com decisões/ações conhecidas do acervo.
- **Inconsistência da fonte:** o PNG combina data de 15/01/2026 com “Resultados do 2º semestre”. Mitigação: armazenar ambos como observados, sinalizar conflito e não corrigi-lo por inferência.
- **Metadados inferidos incorretos:** Bedrock pode omitir ou associar pessoa/prazo errados. Mitigação: JSON fechado, evidência por `block_id`, validação determinística, proveniência `ai_inferred` e revisão.
- **Alucinação na resposta:** linguagem fluente pode não estar sustentada. Mitigação: contexto limitado, fontes obrigatórias, validação de citações e resposta “evidência insuficiente”.
- **Prompt injection documental:** um arquivo pode conter instruções maliciosas. Mitigação: tratar documento como dado, separar instruções do contexto, filtrar entrada/saída e nunca permitir ações operacionais pelo chat.
- **Autorização após recuperação:** filtrar tarde pode expor trechos indevidos ao modelo. Mitigação: derivar filtros dos claims antes de `Retrieve` e revalidar fonte ao abrir o original.
- **Busca apenas semântica no S3 Vectors:** IDs exatos podem ter recuperação fraca. Mitigação: filtros/metadados e busca exata; OpenSearch Serverless somente se necessidade híbrida for medida.
- **Limite de metadados vetoriais:** copiar JSON completo pode exceder limites. Mitigação: metadados compactos no vetor e registro integral no S3/DynamoDB.
- **Cálculos incorretos pelo LLM:** converter o CRM em texto favorece erros. Mitigação: Glue/Athena para agregações e Bedrock apenas para interpretar/resumir resultados calculados.
- **SQL indevido ou caro:** consulta gerada pode acessar colunas proibidas ou escanear demais. Mitigação: allowlist, validação somente leitura, workgroup, Parquet, partições quando houver volume e limite de bytes.
- **Duplicidade e reprocessamento:** eventos repetidos podem gerar OCR/embeddings duplicados. Mitigação: `document_id`, manifesto idempotente e sincronização controlada.
- **Falhas silenciosas:** perda de evento ou etapa parcial pode deixar documento ausente. Mitigação: DLQ, estados Step Functions, quarentena lógica, alarmes e reconciliação entre raw e manifestos.
- **Custos variáveis:** Textract, Bedrock, Knowledge Bases, vetores, Athena e logs crescem com uso. Mitigação: serverless, limites, Budget de US$ 5, tags, retenção, inventário e cleanup; o Budget não interrompe gasto.
- **Disponibilidade regional e evolução de serviços:** modelos, S3 Vectors, limites e preços podem mudar. Mitigação: verificar região/pricing imediatamente antes da POC e manter OpenSearch/Aurora como alternativas, não recursos simultâneos.
- **Escopo atual sem evidência de execução:** toda a arquitetura é proposta. Não há métricas, respostas, custo observado, ARN, print ou recurso AWS real a apresentar nesta etapa.
```

---

## 6. Melhorias futuras

Descreva como a solução poderia evoluir.

```md
Exemplo:
- Criar uma interface web para consulta.
- Criar um chat interno para perguntas sobre atas.
- Adicionar controle de acesso por departamento.
- Criar dashboard de decisões e pendências.
- Gerar alertas automáticos sobre ações em aberto.
- Integrar com ferramentas corporativas.
```

**Sua resposta:**

```md
- Implementar infraestrutura como código com AWS CDK ou AWS CloudFormation, revisão de mudanças e ambientes separados.
- Criar a POC mínima e autorizada em fases: ingestão/preservação, Textract, consulta Athena, depois Knowledge Base e poucas perguntas RAG.
- Construir conjunto de avaliação versionado com perguntas sobre decisões, responsáveis, riscos, handwriting, IDs exatos e agregações do CRM.
- Adicionar tela de revisão humana para OCR/metadados de baixa confiança, mantendo histórico de correções e responsável pela validação.
- Criar dashboard operacional e de qualidade no CloudWatch com cobertura de citações, recuperação, latência, falhas e custo.
- Evoluir para controle de acesso por documento/departamento e federação do Cognito com o diretório corporativo.
- Criar dashboards analíticos do CRM com Amazon QuickSight somente se houver público e métricas definidos; não duplicar a função da Wiki sem necessidade.
- Adicionar alertas de ações vencidas usando EventBridge Scheduler, Lambda e Amazon SNS depois de validar responsáveis e regras de negócio.
- Avaliar OpenSearch Serverless quando medições demonstrarem necessidade de busca híbrida, facetas ou maior QPS; avaliar Aurora pgvector apenas se houver requisito relacional real.
- Automatizar avaliação de regressão após mudança de modelo, prompt, chunking ou embeddings e manter possibilidade de reindexação por `schema_version`.
- Aplicar políticas de retenção, classificação de dados e revisão periódica de acesso quando o acervo deixar de ser fictício.
- Integrar novas fontes corporativas somente por conectores e serviços AWS autorizados, mantendo checksum, proveniência e tratamento específico por formato.
```

---

# 🧠 Checklist Final

Antes de entregar, confirme se sua solução responde:

- [ ] Como transformar documentos escaneados em texto?
- [ ] Como lidar com diferentes formatos dentro da mesma pasta `raw/`?
- [ ] Como armazenar os documentos originais?
- [ ] Como preservar a rastreabilidade entre resposta e documento fonte?
- [ ] Como organizar metadados?
- [ ] Como criar busca semântica?
- [ ] Como usar Amazon Bedrock na solução?
- [ ] Como proteger documentos sensíveis?
- [ ] Como monitorar falhas?
- [ ] Como a empresa usaria essa Wiki no dia a dia?

---

# 🏁 Conclusão

Escreva uma breve conclusão defendendo sua solução como se estivesse apresentando para uma liderança técnica ou de negócio.

**Sua resposta:**

```md
Preencha aqui.
```

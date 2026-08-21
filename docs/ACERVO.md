# Inventário factual do acervo

## Escopo e método

Este inventário registra somente fatos observados nos três arquivos presentes diretamente em `raw/`. Os arquivos foram lidos sem alteração.

- O PDF foi inspecionado por propriedades, extração da camada textual e renderização visual das cinco páginas.
- O PNG foi inspecionado visualmente em sua resolução original. Nenhum resultado de OCR foi produzido nesta task.
- O CSV foi lido como tabela, com contagem de registros e colunas, perfil de preenchimento, domínios categóricos e verificações simples de consistência.

## Arquivos encontrados

| Arquivo | Formato e tamanho | Característica observada | SHA-256 observado antes da entrega |
|---|---:|---|---|
| `ata_reuniao_vendas_sa.pdf` | PDF, 150.295 bytes | Documento digital de 5 páginas, com camada de texto selecionável | `E785FC07E682EDBB959004076DDF6B7B28E56F68029ECD41734B4C8D5CF61D59` |
| `ata_resultados_vendas_novos_dados.png` | PNG, 3.573.154 bytes, 1900 × 2700 pixels, RGB 24 bits | Imagem de uma página digitalizada; o conteúdo está nos pixels | `B4628DF37CC5495580879A43C34D4B940904845A586406D458D7AE8647A3DB9F` |
| `vendas_sa_dados_ficticios_laboratorio.csv` | CSV, 53.030 bytes | Tabela UTF-8 com BOM, delimitada por vírgulas, com cabeçalho, 240 registros e 19 colunas | `9B6C40CD9A8383C193D1F5E8B93EEB4FA68D7431ABA95808F5786AEA03671A5B` |

## 1. PDF digital — `ata_reuniao_vendas_sa.pdf`

### Estrutura observada

- Título: **Ata de Reunião Comercial**, da Vendas S.A., reunião de acompanhamento mensal.
- O próprio documento se declara 100% simulado e destinado a exercícios.
- Reunião em 08/07/2026, das 09h00 às 10h35, por videoconferência, na sala virtual Orion.
- Código da ata: `VSA-COM-2026-07`; classificação declarada: uso didático/dados simulados.
- Seis participantes fictícios são apresentados em tabela com função e papel na reunião: Mariana Costa, Rafael Nunes, Camila Rocha, Bruno Almeida, Fernanda Lima e Livia Mendes.
- Há pauta e objetivos, painel de indicadores, discussões, decisões, plano de ação, riscos, próxima reunião, encerramento e um bloco estruturado para exercícios.
- O painel contém seis indicadores com meta, realizado, atingimento e leitura.
- A seção de decisões contém cinco itens (`D-001` a `D-005`), todos com status “Aprovada por consenso”.
- O plano de ação contém seis itens (`A-001` a `A-006`), com ação, responsável, prazo, prioridade e status.
- A seção de riscos contém quatro itens (`R-01` a `R-04`), com probabilidade, impacto e resposta planejada.
- A próxima reunião está registrada para 03/08/2026 às 09h00.
- A quinta página repete campos-chave em uma tabela simples, incluindo data, horários e totais.

### Conteúdo pesquisável observado

- Campanha fictícia “Rota 120”, aprovada para o terceiro trimestre, com 120 contas-alvo e meta simulada de R$ 6.000.000 em pipeline qualificado até 30/09/2026.
- Decisões sobre revisão semanal do pipeline, alerta após sete dias sem atividade, padronização dos motivos de perda, execução da campanha e criação de painel comercial.
- Ações associadas nominalmente a responsáveis e prazos.
- Riscos relativos a dados incompletos, atraso na seleção de contas-alvo, oportunidades sem próxima atividade e concentração de receita.

### Características e dificuldades observadas

- A camada textual existe e permitiu extrair conteúdo das cinco páginas; portanto, o arquivo não depende de OCR para obtenção do texto.
- O layout usa tabelas, cabeçalhos, listas, destaques e rodapés. A extração linear preserva as palavras, mas pode perder relações entre células ou quebrar frases de tabelas entre linhas e páginas.
- O plano de ação começa na página 3 e continua na página 4; a continuidade precisa ser preservada em qualquer representação estruturada.
- Cabeçalho, aviso didático e número da página se repetem e constituem ruído para uma versão textual normalizada.
- O documento contém acentos, valores monetários, percentuais, datas, horários e identificadores que precisam manter o contexto.

## 2. PNG digitalizado — `ata_resultados_vendas_novos_dados.png`

### Estrutura e conteúdo observados visualmente

- Título: **Vendas S.A. — Ata de Reunião Comercial**; subtítulo: “Resultados do 2º semestre — dados simulados”.
- Data: 15 de janeiro de 2026; horário: 09h00 às 11h10; local: Sala Comercial 3 - Matriz.
- Objetivo declarado: revisão dos resultados do segundo semestre e definição de metas.
- Participantes nomeados: Marina Lopes, Paulo Mendes, Carla Ribeiro, Diego Alves e Renata Souza; o texto também menciona supervisores regionais sem nomeá-los.
- O resumo executivo registra faturamento consolidado de R$ 9,85 milhões e superação da meta em 7,1%.
- Há uma tabela de seis indicadores comerciais: faturamento, pedidos faturados, ticket médio, taxa de conversão, novos clientes e churn de clientes. Ela apresenta realizado, meta e variação/leitura.
- Há desempenho por cinco regiões: Sudeste, Sul, Nordeste, Centro-Oeste e Norte.
- Quatro observações tratam de reativação de clientes, canal digital, serviços premium e desempenho da região Norte.
- O plano de ação possui quatro itens, cada um com responsável e prazo: Paulo Mendes (28/02/2026), Renata Souza (12/02/2026), Diego Alves (20/02/2026) e Carla Ribeiro (05/02/2026).
- O encerramento registra 11h10. Na parte inferior aparecem linhas de assinatura com os nomes Marina Lopes e Paulo Mendes e um carimbo “DOCUMENTO FICTÍCIO”.

### Elementos gráficos e manuscritos observados

- A página apresenta aparência de digitalização: fundo acinzentado, leve inclinação/perspectiva, sombras nas bordas, pequenos pontos e variação de iluminação.
- Existe uma anotação manuscrita azul “conferir CRM”, sublinhada, próxima à tabela de indicadores.
- Existe uma anotação manuscrita vermelha “ação prioritária”, com um contorno sobre o prazo `28/02/2026` do primeiro item do plano de ação.
- Texto impresso, tabela, manuscrito, linhas de assinatura e carimbo coexistem na mesma imagem.

### Características e dificuldades observadas

- Não há camada textual diretamente extraível como no PDF; a leitura automatizada exigirá reconhecimento do conteúdo visual.
- A tabela exige preservação da associação entre indicador, realizado, meta e variação.
- As anotações manuscritas têm cor, orientação e posição diferentes do texto impresso e acrescentam contexto ao conteúdo próximo.
- Sombras, ruído, inclinação e caracteres sem acentos no texto impresso podem afetar o reconhecimento.
- O subtítulo menciona resultados do segundo semestre, enquanto a data registrada é 15/01/2026. O inventário mantém ambos como aparecem, sem tentar corrigir ou inferir a intenção.

## 3. CSV estruturado — `vendas_sa_dados_ficticios_laboratorio.csv`

### Forma e esquema observados

- Há 240 oportunidades e 19 colunas.
- Os 240 valores de `oportunidade_id` são únicos.
- Colunas observadas:

| Coluna | Tipo conceitual observado | Preenchimento/intervalo observado |
|---|---|---|
| `oportunidade_id` | identificador | completo; 240 valores únicos |
| `data_criacao` | data | completo; 2026-07-01 a 2026-09-30 |
| `data_fechamento` | data opcional | 118 preenchidos; 2026-07-19 a 2026-09-30 |
| `cliente_ficticio` | texto/categoria de entidade | completo |
| `segmento` | categoria | completo; 5 valores |
| `regiao` | categoria | completo; 4 valores |
| `vendedor_ficticio` | categoria de pessoa | completo; 12 valores |
| `origem_lead` | categoria | completo; 5 valores |
| `produto` | categoria | completo; 5 valores |
| `campanha` | categoria | completo; 5 valores |
| `status` | categoria | completo; 5 valores |
| `probabilidade_pct` | percentual inteiro | completo; 0 a 100 |
| `valor_bruto_brl` | valor monetário decimal | completo; 45.900,00 a 259.700,00 |
| `desconto_pct` | percentual inteiro | completo; 0 a 15 |
| `valor_liquido_brl` | valor monetário decimal | completo; 42.687,00 a 251.500,00 |
| `ciclo_dias` | duração inteira em dias | completo; 0 a 91 |
| `motivo_perda` | categoria opcional | 46 preenchidos; 194 vazios |
| `proxima_atividade` | data opcional | 122 preenchidos; 2026-07-07 a 2026-10-16 |
| `observacao` | texto categórico curto | completo; 15 textos distintos |

### Domínios e relações observados

- Segmentos: Indústria, Logística, Serviços empresariais, Tecnologia e Varejo.
- Regiões: Centro-Oeste, Nordeste, Sudeste e Sul. Não há registro da região Norte neste CSV.
- Produtos: Analytics de Receita, Automação Comercial, CRM Essencial, CRM Profissional e Integração Enterprise.
- Campanhas: Clientes Inativos, Expansão Regional, Parcerias 360, Rota 120 e Sem campanha.
- Status e contagens: Em negociação (42), Ganha (72), Perdida (46), Proposta enviada (44) e Qualificação (36).
- Os 118 registros com status Ganha ou Perdida têm `data_fechamento`; os demais 122 não têm essa data.
- Somente os 46 registros Perdida têm `motivo_perda`. Os motivos observados são Baixa aderência, Concorrente escolhido, Preço, Prioridade adiada, Sem orçamento e Sem retorno.
- Os 122 registros ainda não fechados têm `proxima_atividade`; os 118 registros fechados não têm esse campo.
- Em todos os registros, `valor_liquido_brl` corresponde a `valor_bruto_brl` após aplicação de `desconto_pct`, na precisão registrada.
- Não foi encontrada `data_fechamento` anterior à respectiva `data_criacao`.

### Características e dificuldades observadas

- O arquivo representa oportunidades como linhas e atributos como colunas; não é texto corrido.
- Campos vazios têm significado dependente do status. Por exemplo, ausência de data de fechamento nos registros abertos e ausência de próxima atividade nos registros fechados não devem ser tratadas automaticamente como o mesmo tipo de erro.
- Datas, percentuais e valores monetários precisam manter seus tipos para filtros, ordenações e cálculos.
- Acentuação não é uniforme no conteúdo: há valores com acento, como `Prospecção ativa`, e outros grafados sem acentos, como `Indicacao de parceiro` e `Reativacao de cliente`.
- O CSV permite agregações determinísticas; transformar cada linha cegamente em texto perderia parte de sua estrutura e facilitaria erros de cálculo.

### Perguntas sustentadas pelas colunas observadas

- Quantas oportunidades existem por status, região, segmento, produto, campanha ou vendedor?
- Qual é o valor bruto ou líquido total e médio por região, campanha, produto ou status?
- Quais oportunidades abertas têm próxima atividade em determinado período?
- Quais motivos de perda são mais frequentes, inclusive por segmento ou origem do lead?
- Como desconto, ciclo de vendas e probabilidade variam entre oportunidades ganhas, perdidas e ainda abertas?
- Quais campanhas e origens de lead estão associadas a oportunidades ganhas ou a maior valor líquido?

Essas são possibilidades de consulta decorrentes do esquema; nenhuma resposta agregada além das contagens e intervalos explicitamente registrados acima foi calculada nesta task.

## Diferenças que determinam o tratamento

| Aspecto | PDF digital | PNG digitalizado | CSV estruturado |
|---|---|---|---|
| Unidade principal | páginas e seções | uma página visual | linhas e colunas |
| Texto diretamente disponível | sim | não | sim, em células |
| Estruturas relevantes | tabelas, listas, seções e continuidade entre páginas | tabela, texto impresso, manuscrito e posição visual | esquema, tipos, relações e valores ausentes |
| Risco principal observado | perda da estrutura ao linearizar o texto | erros de reconhecimento e perda da relação espacial | perda de tipos ou agregações incorretas ao converter em texto |


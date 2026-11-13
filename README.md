Fundamentos de IA para Programar com IA Generativa (Vibecoding)

Apresentação teórica e prática desenvolvida para capacitar pessoas sem formação prévia em tecnologia a construírem soluções funcionais de software utilizando ferramentas de Inteligência Artificial Generativa.

Oferecido por BEZ.

📌 Acesso Rápido à Apresentação

Para visualizar os slides interativos diretamente pelo navegador, acesse o link do GitHub Pages:

👉 Visualizar Apresentação de Slides Online

(Dica: Pressione as teclas ← e → no teclado para navegar ou clique no botão Tela Cheia).

⏱️ Estrutura da Apresentação (15 Minutos)

Abertura: Por que qualquer pessoa pode criar soluções de software hoje.

A Pergunta Central: "Máquinas podem pensar?"

Alan Turing (1950): O Jogo da Imitação e a ideia da máquina análoga à mente de uma criança.

Arthur Samuel (1959): A definição fundacional de Machine Learning.

IA vs. Machine Learning: Regras determinísticas ("Se X, faça Y") vs. modelos probabilísticos ("Se X tiver a maior probabilidade, faça Y").

Mudança de Paradigma: Como a programação tradicional foi invertida pelos dados.

Exemplos do Cotidiano: Filtros de spam adaptativos e detecção de fraudes no iFood (a Regra do Gin).

Modelos Discriminativos vs. Generativos: Diferença intuitiva entre classificar e criar.

Taxonomia Completa de Machine Learning: Diagrama estruturado cobrindo:

Aprendizado Supervisionado (Classificação e Regressão)

Aprendizado Não-Supervisionado (Clustering, Associação e Anomalia)

Aprendizado Semi-Supervisionado

Aprendizado por Reforço

A Diferença Visual: Supervisionado (maçãs rotuladas) vs. Não-Supervisionado (cesto de frutas agrupadas em clusters).

Reforço, Semi e Auto-Supervisionado: A transição para os métodos modernos.

O Que São LLMs? Como grandes modelos de linguagem operam calculando a próxima palavra estatisticamente mais provável.

Evolução da Rotina de Programação: Do sofrimento de sintaxe no StackOverflow para a orquestração em linguagem natural (Vibecoding).

A Soberania da Lógica: Por que pensar com clareza, decompor problemas e validar saídas continua indispensável.

Transição para a Prática: Construção colaborativa de um dashboard interativo.

🛠️ Tecnologias Utilizadas

HTML5 / CSS3 / JavaScript Nativo: Estrutura responsiva com controles de teclado e modo tela cheia.

FontAwesome & Google Fonts: Tipografia moderna (Plus Jakarta Sans e Space Grotesk) e ícones de apoio.

SVG In-line: Todos os diagramas e ilustrações são renderizados nativamente, garantindo nitidez vetorial sem perda de qualidade.

📄 Licença e Uso

Material didático desenvolvido pela startup BEZ para palestras, workshops e capacitação em tecnologia com IA Generativa.


Link para apresentação: https://irving-merath.github.io/fundamentos-ia/


###Link do projeto criado
https://github.com/Irving-Merath/dashdatasus

### 🛠️ Guia Prático de Vibecoding

Clique nas etapas abaixo para copiar os prompts usados na oficina:

<details>
<summary>📱 <b>Prompt 1: Criação via Celular (Android / Chrome)</b> (Clique para expandir)</summary>

```text
Você vai me ajudar a criar, do zero, uma plataforma gratuita com um dashboard de dados do DATASUS. Responda sempre em português do Brasil. O plano abaixo já foi discutido e aprovado por mim; siga-o fielmente.

## MINHAS CONDIÇÕES
1. Desenvolvo apenas pelo celular Android (navegador Chrome). Tudo precisa ser possível pelo celular.
2. Só ferramentas gratuitas, sem cartão de crédito.
3. Tudo deve ser user friendly: qualquer pessoa sem conhecimento de programação precisa conseguir reproduzir.
4. Explique cada etapa (o quê, por quê e como) antes de eu executá-la.
5. Se precisar de alguma permissão (no GitHub, no Streamlit etc.), explique e justifique. Você não precisa de acesso ao meu celular.

## DECISÕES JÁ TOMADAS
- "Tempo real": o DATASUS publica dados periodicamente, não ao vivo. Solução: um robô que busca automaticamente as versões mais recentes toda semana.
- Arquitetura: GitHub (repositório PÚBLICO) + GitHub Actions (robô de coleta) + Streamlit Community Cloud (dashboard). O robô baixa os microdados do FTP do DATASUS, agrega por município/ano e salva arquivos parquet pequenos em dados/ no próprio repositório; o Streamlit lê esses arquivos.
- Abrangência: Brasil inteiro, com filtros por estado e municípios.
- Bases previstas (menu "Tema"): SINASC (nascimentos), SIM (óbitos), SIH (internações), SINAN (doenças de notificação), CNES (estabelecimentos, uma foto por ano em dezembro), SIA (ambulatorial) e PNI (vacinação).
- Período: 2010 a 2024 para SINASC, SIM, SIH, SINAN e CNES; 2022 a 2024 para SIA e PNI (muito pesadas, com resumos mais enxutos).
- As bases são ativadas uma por vez, nesta ordem: SINASC → SIM → SIH → SINAN → CNES → SIA e PNI. Agora, entregue o projeto completo com SINASC ativado; as demais aparecem no dashboard como "em breve" e são ativadas depois, substituindo apenas coletar.py (o app.py reconhece sozinho as bases que já têm dados).
- Uma alternativa com Google Sheets + Looker Studio + Tabnet foi descartada por exigir downloads manuais.

## ARQUIVOS A GERAR (testados antes de entregar)

### 1. coletar.py (robô)
- Configurações no topo: ANO_INICIAL=2010, ANO_FINAL=2024, FTP_HOST="ftp.datasus.gov.br", PASTA_DADOS="dados", PASTA_SAIDA="saida", dicionário código IBGE da UF -> sigla.
- CLI: python coletar.py --base sinasc --uf RJ (coleta um estado) e python coletar.py --consolidar.
- FTP com ftplib (modo passivo), timeout 180 s, reconexão e novas tentativas com espera crescente; listagem de pastas em cache; nomes comparados em maiúsculas.
- SINASC: arquivos DN{UF}{ANO}.DBC, procurados nas pastas, nesta ordem: /dissemin/publicos/SINASC/NOV/DNRES (final), /dissemin/publicos/SINASC/1996_/Dados/DNRES (final), /dissemin/publicos/SINASC/PRELIM/DNRES (preliminar).
- Descompactar DBC com pyreaddbc.dbc2dbf (há wheel pronta para Python 3.12).
- Leitor DBF próprio e rápido: ler cabeçalho (nº de registros bytes 4-7, tamanho do cabeçalho 8-9, tamanho do registro 10-11, descritores de campo de 32 bytes até 0x0D), usar numpy.memmap, ler apenas as colunas necessárias, em blocos de 400 mil registros, ignorar registros apagados ('*'), decodificar latin-1 e remover espaços. Colunas inexistentes vêm vazias.
- Controle incremental: dados/<base>/_controle/<UF>.json guarda a assinatura "pasta/arquivo:tamanho" por ano; se não mudou e o parquet existe, pula.
- Saída por estado e ano: saida/<base>/<UF>_<ANO>.parquet (ou _prelim se veio da pasta preliminar), colunas: ano, cod_mun (6 dígitos), variavel, categoria (texto), n (int32). Também saida/<base>/_uf/<UF>_<ANO>.parquet com o mesmo conteúdo agregado por UF (cod_mun = código de 2 dígitos da UF), usado na visão Brasil.
- Código de município inválido vira "<código UF>0000" (município ignorado).
- Variáveis do SINASC: total; sexo (1/M Masculino, 2/F Feminino); raca_cor (1 Branca, 2 Preta, 3 Amarela, 4 Parda, 5 Indígena); faixa_idade_mae a partir de IDADEMAE válida 10–60 (Menos de 15, 15 a 19, 20 a 24, 25 a 29, 30 a 34, 35 a 39, 40 ou mais); escolaridade_mae via ESCMAE.


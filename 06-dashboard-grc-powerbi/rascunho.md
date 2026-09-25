### **Brief --- Projeto 6: Dashboard de Indicadores de GRC via Power BI**

**Cenário**

Depois de tudo que a CloudFlux passou nos últimos meses (matriz de
risco, auditoria, formalização de change management, incidente de
segurança, e agora formalização de gestão de identidade), o Board pediu
que a área de GRC passasse a apresentar, de forma contínua e visual, o
status de maturidade da empresa, ao invés de reportar isso só quando
algo dá errado ou quando chega época de auditoria. Você, como analista,
é responsável por estruturar esse dashboard no Power BI, usando os
próprios dados que já foram gerados ao longo dos projetos anteriores.

**Objetivo**

Construir um dashboard (relatório) no Power BI Service que visualize
indicadores-chave de GRC da CloudFlux, permitindo que a liderança
acompanhe status de controles, ações corretivas e maturidade geral, sem
depender de um relatório novo cada vez que alguém pergunta.

**Framework de referência**

Não existe um framework único aqui, mas vale usar o conceito de **KPI
(Key Performance Indicator)** e a lógica de dashboards de governança de
mercado real, que geralmente giram em torno de: status de controles
(quantos conformes x não conformes), ações corretivas em aberto x
fechadas, e tempo de resposta a incidentes.

**Escopo (dentro)**

- Estruturar uma base de dados simples (pode ser uma planilha Excel/CSV
  que você mesma cria) reunindo os dados que já existem espalhados pelos
  seus projetos anteriores (os 8 controles da Auditoria com status
  Conforme/Ressalva/Não Conforme, os RNCs abertos, dados do incidente do
  Rafael)

- Importar essa base pro Power BI

- Criar pelo menos 3 visualizações diferentes (ex: gráfico de pizza de
  status de controle, gráfico de barra de RNC por área, indicador
  numérico de algo relevante)

- Montar um dashboard/relatório único, organizado, que conte a história
  de maturidade da CloudFlux de forma visual

**Escopo (fora, não é esperado nesse projeto)**

- Conexão em tempo real com fonte de dado externa (isso é nível avançado
  de administração/engenharia de dado)

- Modelagem de dado complexa com múltiplas tabelas relacionadas (DAX
  avançado, relacionamento many-to-many) --- se o escopo pedir algo
  simples o suficiente pra não precisar disso, ótimo

- Publicação/compartilhamento do dashboard pra \"outras pessoas\" reais
  (você é a única usuária desse ambiente)

**Calibração de expectativa (nível júnior)**

Primeiro contato com Power BI. Não é esperado domínio de DAX (a
linguagem de fórmula do Power BI) nem modelagem de dado sofisticada. O
que importa aqui é a lógica de **storytelling com dado**: você escolher
quais indicadores mostrar, e por quê, pensando em \"o que a liderança
realmente precisa ver pra entender a maturidade da empresa
rapidamente\", não só jogar gráfico bonito sem propósito.

**O que você precisa PESQUISAR (norte, não resposta)**

- \"Power BI Service tutorial iniciante\" no YouTube

- Como importar uma planilha Excel/CSV pro Power BI Service

- Diferença entre \"Relatório\" (Report) e \"Dashboard\" no Power BI
  (parecem sinônimo, não são exatamente)

- O que é um KPI e como escolher indicadores relevantes pra liderança
  (não é sobre mostrar todo dado que existe, é sobre mostrar o que
  importa)

**Avaliação**

Sem gabarito oculto. A avaliação vai olhar pra escolha dos indicadores
(fazem sentido pro que a liderança precisaria ver?) e clareza visual
(alguém de fora entenderia o status da empresa só olhando o painel?).

**Formato de entrega**

Print(s) do dashboard/relatório montado + um resumo seu explicando por
que você escolheu esses indicadores específicos.

# 🧭 Portfólio GRC · Carolina Menezes

> Projetos simulados de **Governança de TI, Risco e Compliance**, construídos em torno de uma mesma empresa fictícia, a CloudFlux, e conectados entre si como uma história contínua.

Venho de marketing e operações de agência, em transição para **Governança de TI e GRC**. Nas agências, eu já organizava processos, controlava acessos e trabalhava de forma preventiva, sem saber o nome técnico disso. Este repositório é a formalização desse conhecimento: **eu já fazia, agora eu sei o nome.**

🎓 Graduação em andamento em Gestão da Tecnologia da Informação (Anhembi Morumbi).

---

## 🏢 A CloudFlux

Um SaaS B2B de gestão de projetos em nuvem, com cerca de **180 funcionários**. Cresceu rápido, sempre priorizando tecnologia em vez de processo, e resolvia os problemas de TI à medida que eles apareciam.

Até que o maior cliente exigiu gestão de riscos formalizada para renovar o contrato. É aí que a série começa.

---

## 🧪 Metodologia

Cada projeto parte de um **brief** que simula uma demanda real: cenário, gatilho, escopo (o que entra e o que fica de fora), prazo e calibração para o nível júnior.

- 🧠 **A análise é minha.** Pesquisa, raciocínio, classificações e decisões são feitos por mim, sem uma IA gerando a solução.
- 🤖 **A IA atua como gestora.** Ela propõe o cenário, orienta a direção da pesquisa sem entregar a resposta e avalia a entrega no final, como um board faria.
- 🔒 **Gabarito oculto.** Nos projetos de avaliação de conformidade, existe um gabarito que eu só vejo depois de entregar, para garantir que estou analisando de verdade e não confirmando algo que já sei.
- 🧰 **Ferramenta real sempre que possível.** ServiceNow, Jira Service Management, Keycloak e Power BI foram usados na prática, em ambientes de teste.

---

## 🗂️ Projetos

| # | Projeto | Tema | Referência · Ferramenta |
|:---:|---|---|---|
| 01 | [📊 Matriz de Gestão de Risco](01-matriz-de-risco/) | Identificação, análise e tratamento de riscos de TI | ISO 31000 · NIST |
| 02 | [🕵️‍♀️ Auditoria Interna de Controles](02-auditoria-de-controles/) | Verificação, com evidência, dos controles prometidos | ISO 19011 · COBIT MEA02 · lab em container |
| 03 | [🛠️ Change Management do WAF](03-change-management-servicenow/) | Mudança formal com CAB, plano faseado e rollback | ITIL 4 · ServiceNow |
| 04 | [🚨 Gestão de Incidente](04-gestao-de-incidente-jira/) | Uso anômalo de credencial, da detecção à causa raiz | ITIL 4 · Jira Service Management |
| 05 | [🔐 Gestão de Identidade e Acesso](05-gestao-de-acesso-keycloak/) | RBAC com privilégio mínimo e política de senha testada | Least privilege · Keycloak |
| 06 | [📈 Dashboard de GRC](06-dashboard-grc-powerbi/) 🚧 | Indicadores de maturidade para o Board | KPIs · Power BI Service |
| 07 | [🏢 SAP e ERP](07-sap-conceitual/) 📘 | ERP, camadas de trabalho e Segregação de Funções | Estudo conceitual |
| 08 | [🛡️ Mapeamento de Dados e RIPD](08-lgpd-mapeamento-ripd/) | Dados pessoais, bases legais e impacto ao titular | LGPD · DPIA |

🚧 Em evolução &nbsp;·&nbsp; 📘 Estudo conceitual, sem hands-on

---

## 🧵 O fio da história

Os projetos não são exercícios soltos. Cada um nasce de um achado do anterior:

1. 📊 **01:** o risco de acesso e identidade é mapeado, mas não entra entre as prioridades imediatas.
2. 🕵️‍♀️ **02:** seis meses depois, a auditoria confirma que o RBAC continua em rascunho e encontra o WAF desligado em produção. A VPN recebe status Conforme.
3. 🛠️ **03:** a ativação do WAF em produção passa por change management formal.
4. 🚨 **04:** um incidente de acesso indevido revela que a VPN, declarada obrigatória, tinha uma lacuna de enforcement.
5. 🔐 **05:** com o risco mapeado, a auditoria e o incidente somados, a identidade é finalmente formalizada.
6. 🛡️ **08:** a privacidade entra em pauta, e o controle proposto para o fluxo de maior risco é o mesmo que conteria o incidente do 04.

### 💡 O conceito que atravessa a série

**Controle declarado não é controle validado.** Uma política no papel, uma área dizendo que está pronto ou um campo marcado como concluído não provam que o controle funciona. Boa parte destes projetos é sobre a distância entre essas duas coisas, e sobre como encontrá-la antes que um incidente encontre.

---

## 🐍 Outros repositórios

Projetos em Python, numa trilha paralela de automação aplicada à segurança:

- [quiz-seguranca-python](https://github.com/cahponto/quiz-seguranca-python): quiz de conscientização em segurança da informação.
- [gerador-senhas-python](https://github.com/cahponto/gerador-senhas-python): gerador de senhas seguras com a biblioteca `secrets`.

---

## 🔄 Um portfólio vivo

Estes projetos são revisitados conforme eu aprendo mais. Inconsistências encontradas na revisão ficam registradas nos próprios READMEs, em vez de apagadas, porque o histórico de evolução também faz parte do trabalho.

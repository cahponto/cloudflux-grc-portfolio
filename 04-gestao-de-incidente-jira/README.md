# 🚨 04 · Gestão de Incidente via Jira Service Management

> Uma credencial válida, um horário estranho e um IP de outro estado. A VPN estava declarada como obrigatória. Estava mesmo?

| | |
|---|---|
| 🗂️ **Tipo** | Projeto simulado, com registro real na ferramenta |
| 🧰 **Ferramenta** | Jira Service Management (template de Gerenciamento de Serviços de TI), aprendida do zero |
| 📚 **Referência** | ITIL 4 · Incident Management |
| 📦 **Entregável** | Ticket **GDI-1**, "Tentativa de acesso indevida", da abertura ao fechamento com causa raiz |

---

## 🧰 A ferramenta

A escolha foi o **Jira Service Management**, e não o Jira Software (feito para desenvolvimento, com sprint e backlog) nem o Jira Work Management (genérico, para times não técnicos). O JSM é o único dos três com o vocabulário de incidente, prioridade e SLA.

O ticket foi estruturado em camadas:
- 📄 **Descrição:** os fatos brutos do relato, mantidos intactos, sem análise misturada.
- 💬 **Comentários:** a análise da analista, separada dos fatos, com a justificativa de classificação, a timeline consolidada, a causa raiz e as recomendações.

Diferente do ServiceNow do [Projeto 03](../03-change-management-servicenow/), aqui não há CAB nem fluxo de aprovação. Isso é característica da prática, e não limitação: **incidente se contém primeiro e se formaliza depois**.

---

## 🏢 Cenário

A CloudFlux usa o JSM como service desk do time de segurança e suporte. A analista recebe, tria e conduz o atendimento de um incidente de **comportamento anômalo de credencial**: um uso legítimo no papel (senha e MFA válidos), mas fora do padrão do dono da conta.

A investigação foi conduzida em formato de perguntas ao "time de TI", que relatava apenas fatos brutos. A classificação, a timeline e a causa raiz foram construídas a partir do que a própria investigação levantou.

---

## 🔎 O que a investigação levantou

| | Fato |
|---|---|
| 👤 **Credencial** | Rafael Nogueira (Infraestrutura), sem permissão de administrador no painel do banco de clientes |
| 🕚 **Horário** | Terça, 23h47, fora do expediente dele (9h às 18h) |
| 📍 **Origem** | IP geolocalizado no Rio de Janeiro. Rafael mora e trabalha em São Paulo, nunca teve login de outro estado e nega ter viajado |
| 🔐 **Autenticação** | Login com senha e MFA válidos |
| 🚪 **Tentativa** | 4 acessos ao painel de administração do banco de clientes em menos de 2 minutos, todos **barrados pelo RBAC** do sistema interno |
| 💾 **Exposição** | Nenhum dado exibido, nenhum malware e nenhum servidor comprometido |
| 🌐 **Varredura ampliada** | Sem atividade fora do padrão em VPN, e-mail, Financeiro, RH, código-fonte ou em outras credenciais. Incidente isolado |
| 🧭 **IP e dispositivo** | IP fora de blacklists e inédito nos logs. User-agent Chrome em Windows, a mesma configuração do notebook dele, mas comum demais para confirmar. Uma única sessão ativa |
| ☕ **Relato do colaborador** | Nega o acesso e diz que estava dormindo. Mencionou por conta própria que usa o notebook da empresa em wifi público |
| 🎓 **Conscientização** | A CloudFlux nunca teve programa formal de conscientização em segurança |

---

## ⚖️ Classificação

A prioridade foi definida pela **superfície de risco**, e não pelo desfecho. O fato de o RBAC ter barrado o acesso não reduz a gravidade do que foi tentado:
- A credencial estava **comprometida**, com senha e segundo fator superados.
- O alvo era o **banco de dados de clientes**, justamente o ativo cujo vazamento derruba contrato e gera sanção da LGPD.
- O invasor estava **dentro do sistema** quando foi barrado. Faltou uma camada só.

---

## 🗓️ Timeline de resposta

| Quando | Fase | O que aconteceu |
|---|---|---|
| Terça, 23h47 | Ocorrência | Login com credencial válida a partir do Rio de Janeiro e 4 tentativas barradas pelo RBAC. O log gera alerta, **mas não notifica ninguém** |
| Terça, após 23h47 | Ocorrência | Nenhuma nova atividade da credencial ou do IP |
| Quarta, ~9h | 🔍 Detecção | Incidente identificado em **revisão manual de log**, cerca de 9 horas depois, e ticket GDI-1 aberto |
| Quarta, manhã | 🛑 Contenção | Senha trocada e MFA re-registrado do zero |
| Quarta | 🔬 Investigação | Conversa com o colaborador, análise de IP, dispositivo e sessões, e varredura ampliada em outros sistemas |
| Quarta | ♻️ Recuperação | Colaborador operando com credenciais novas, sem outros sistemas afetados |
| Fechamento | 📚 Lições aprendidas | Causa raiz registrada e recomendações de melhoria |

---

## 🧩 Causa raiz

**Conclusão mais provável:** a sessão do colaborador foi comprometida em **rede insegura** (wifi público), por **roubo de sessão**. Isso explica como o acesso passou pelo MFA: o invasor reaproveitou uma sessão já autenticada, em vez de precisar do segundo fator.

**O controle declarado que falhou:** a VPN era declarada obrigatória desde a [Auditoria (Projeto 02)](../02-auditoria-de-controles/), onde recebeu status Conforme. A investigação revelou uma **lacuna de enforcement técnico**: nem todo acesso sem VPN estava sendo bloqueado de fato.

**O controle que funcionou:** o **RBAC** do sistema interno, mesmo com a política formal de gestão de acesso ainda em construção.

### 🧷 Fato x hipótese

| ✅ Confirmado | ❔ Não confirmável com as evidências |
|---|---|
| O momento do uso indevido (terça, 23h47) | O momento exato do comprometimento da sessão |
| Que o RBAC barrou as 4 tentativas | Se o dispositivo usado foi o notebook físico do colaborador |
| Que nenhum dado foi exposto | Em qual rede, especificamente, a sessão foi capturada |

---

## 🛡️ Recomendações de melhoria contínua

- 🌐 **Validar tecnicamente o enforcement da VPN**, bloqueando de fato o acesso a sistemas internos fora dela.
- 🔁 **Auditoria técnica periódica de controles declarados**, sem se apoiar só em confirmação declarativa das áreas.
- 🎓 **Programa de conscientização em segurança**, cobrindo uso de wifi público e phishing.
- 📡 **Monitoramento em tempo real de acesso anômalo**, com notificação. O alerta existia, mas ninguém soube dele por 9 horas.

---

## ✅ O que este projeto demonstra

- Conduzir um incidente pelo ciclo do **ITIL 4**: detectar, conter, investigar, recuperar e aprender.
- Classificar pela **superfície de risco**, e não pelo resultado.
- Separar **fato de hipótese** numa investigação com evidência incompleta.
- Identificar a diferença entre **controle declarado** e **controle validado tecnicamente**.
- Estruturar um ticket em que os fatos e a análise não se misturam.

## 🔜 Desdobramentos na série CloudFlux

- 🔙 A VPN, declarada Conforme na [02 · Auditoria de Controles](../02-auditoria-de-controles/), mostra aqui a sua lacuna real.
- 🔐 Com o risco de identidade mapeado desde o Projeto 01, a RNC 01 da Auditoria e agora um incidente, a CloudFlux finalmente formaliza o RBAC em **[05 · Gestão de Acesso via Keycloak](../05-gestao-de-acesso-keycloak/)**.

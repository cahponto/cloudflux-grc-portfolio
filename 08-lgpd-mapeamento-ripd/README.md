# 🛡️ 08 · Mapeamento de Dados Pessoais e RIPD (LGPD)

> Todo mundo olha para o banco de dados. O maior risco à privacidade estava num botão de suporte.

| | |
|---|---|
| 🗂️ **Tipo** | Projeto simulado |
| 📚 **Referências** | LGPD (Lei 13.709/2018) · lógica de DPIA (Data Protection Impact Assessment) |
| 📦 **Entregáveis** | Mapeamento de dados pessoais · RIPD do fluxo de maior risco |
| 🎯 **Público do documento** | Jurídico e Encarregado de Dados (DPO) da CloudFlux |

---

## 🏢 Cenário

A CloudFlux é um SaaS B2B com cerca de 180 empresas clientes e **mais de 12 mil usuários finais**, que são os colaboradores dessas empresas usando a plataforma no dia a dia. A empresa nunca tinha feito um mapeamento formal de dados pessoais nem uma avaliação de impacto à proteção de dados.

Depois da auditoria, do incidente e da formalização de identidade, o Jurídico puxou a LGPD como a próxima frente crítica.

📌 **Escopo:** mapear quais dados pessoais são tratados, de quem, para quê e com qual base legal, e produzir o RIPD de **um** fluxo, o de maior risco aos titulares.

---

## 🗺️ 1. Mapeamento de dados pessoais

A partir do relato de Produto e Engenharia, foram mapeados seis sistemas.

| Sistema | Titular | Dados | Tipo | Finalidade | Base legal |
|---|---|---|---|---|---|
| Plataforma principal | Usuário final | Nome, e-mail corporativo, cargo, telefone, data e hora de login, IP de origem, histórico de ações | Pessoal | Prestação do serviço | Execução de contrato |
| Suporte | Usuário final | Descrição do problema, dados de terceiros visíveis em prints, acesso temporário à conta | Pessoal | Resolução de problema técnico | Execução de contrato |
| Analytics | Usuário final | Comportamento de uso vinculado ao e-mail | Pessoal | Melhoria do produto | Legítimo interesse |
| Faturamento | Responsável financeiro do cliente | Nome e e-mail | Pessoal | Prestação do serviço | Execução de contrato |
| Marketing / CRM | Lead | Dados do formulário de demonstração | Pessoal | Contato comercial inicial | Execução de contrato |
| Marketing / CRM | Lead | Consentimento de comunicação | Pessoal | Envio de comunicação e marketing | Consentimento |
| RH | Colaborador da CloudFlux | Nome, CPF, endereço residencial, dados bancários | Pessoal | Exigências legais | Obrigação legal |
| RH | Colaborador da CloudFlux | Atestado médico | ⚠️ **Sensível** | Exigências legais | Obrigação legal |

📝 Razão social, CNPJ e endereço de cobrança das empresas clientes não entram no mapeamento, porque são dados de pessoa jurídica. O número do cartão de crédito também não, já que fica apenas com o gateway de pagamento terceirizado.

---

## 🔬 2. RIPD · Acesso temporário à conta do usuário

### 🎯 Por que este fluxo

Para reproduzir alguns erros, o Suporte pode entrar na conta do usuário e **ver a plataforma como se fosse ele**, incluindo os dados de colegas que aparecem na interface. É um tratamento que combina visão completa da conta, exposição de pessoas que nunca abriram chamado e nenhum controle registrado.

### 📋 Como funciona hoje

- 📨 O usuário abre um chamado por um recurso integrado à plataforma, conectado via API ao Jira Service Management.
- 👥 Todo o time de Suporte vê os chamados, mas **apenas um subconjunto com permissão elevada** pode acessar a conta do usuário. É a mesma lógica de privilégio mínimo aplicada no [Keycloak (Projeto 05)](../05-gestao-de-acesso-keycloak/).
- 🚫 Não há evidência de **log** do acesso, de **prazo** de duração, de **revogação automática** ao final do atendimento nem de **notificação** ao usuário.

### ⚖️ Necessidade e proporcionalidade

- ✅ **É necessário:** parte dos problemas só aparece no contexto real da conta (configuração, dados armazenados, padrão de uso).
- ⚠️ **Mas não para todo chamado:** só quando relato, print ou vídeo não bastam.
- ❌ **Não é proporcional hoje.** Pode existir alternativa técnica que exponha menos dados, mas isso exigiria investigação com o time de Suporte e TI, fora deste escopo. Independente disso, faltam controles básicos: limite de tempo, log obrigatório e transparência.

### 🚨 Riscos aos titulares e mitigação

| Risco | O que pode acontecer | Probabilidade | Gravidade | Mitigação proposta |
|---|---|:---:|:---:|---|
| **R1 · Exposição sem base legal** | Dados de terceiros (colegas do usuário) são vistos pelo Suporte, sem base legal que cubra esse tratamento e sem que eles tenham tido escolha | Média | Média | Transparência nos termos de uso e na política de privacidade voltada aos clientes, informando que o acesso pode ocorrer e pode incluir dados de terceiros |
| **R2 · Uso indevido por acesso autorizado** | Quem tem acesso legítimo visualiza, retém ou compartilha além do necessário, sem rastro | Média | Alta | **Log obrigatório** de todo acesso: quem, quando e por quanto tempo |
| **R3 · Acesso não revogado** | O acesso continua ativo muito depois do chamado resolvido, ampliando a janela dos riscos R1 e R2 | Alta | Alta | **Expiração automática** do acesso, sem depender de revogação manual |

💡 A mitigação do R1 **reduz** a falta de transparência, mas **não elimina** o risco. Nem todo risco some com um controle, e isso precisa estar dito no documento.

### 🧾 Conclusão

O acesso temporário **se justifica em necessidade**, mas **não é proporcional nem adequado** da forma como é praticado hoje.

A recomendação **não é interromper** o recurso, e sim priorizar as três mitigações antes de considerar o tratamento adequado à LGPD. Até lá, o **risco residual permanece elevado**, concentrado na falta de rastreabilidade (R2) e na persistência de acesso não controlado (R3).

---

## ✅ O que este projeto demonstra

- **Privacidade desde o design:** tratar dado pessoal como algo que precisa de justificativa, e não coletar "só porque pode".
- Escolher o fluxo de maior risco **fora do óbvio**, enxergando os **titulares indiretos** (terceiros expostos sem nunca terem interagido com o processo).
- Separar **necessidade** de **proporcionalidade**: um tratamento pode ser necessário e, ainda assim, inadequado.
- Avaliar risco **ao titular**, e não só à empresa, adaptando a lógica de matriz do [Projeto 01](../01-matriz-de-risco/).
- Trabalhar com **risco residual**, apontado como evolução pendente lá no Projeto 01 e aplicado aqui.

## 🔜 Próximo nível

- ⚖️ **Controlador x operador.** No modelo B2B, para os dados dos usuários da plataforma, a CloudFlux tende a atuar como **operadora**, tratando dados em nome das empresas clientes, que seriam as **controladoras**. Isso muda de quem é a decisão sobre base legal e como o contrato com o cliente precisa tratar o tema.
- 🗄️ **Prazo de retenção.** Logs de acesso, IP e histórico de ações são guardados por tempo indeterminado, sem política de expiração definida.
- 📜 Política de privacidade pública, termos de uso e estrutura formal do DPO, que ficaram fora do escopo deste projeto.

## 🧵 Na série CloudFlux

- ⏱️ A **expiração automática** proposta para o R3 é o mesmo controle apontado como próximo passo no [Keycloak (Projeto 05)](../05-gestao-de-acesso-keycloak/), para conter o **roubo de sessão** do [Incidente (Projeto 04)](../04-gestao-de-incidente-jira/). Um único controle atende a segurança e a privacidade ao mesmo tempo.

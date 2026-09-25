# 🔐 05 · Gestão de Identidade e Acesso via Keycloak

> O risco estava mapeado desde o primeiro projeto. Foi preciso um incidente para ele virar prioridade.

| | |
|---|---|
| 🗂️ **Tipo** | Projeto simulado, com configuração real na ferramenta |
| 🧰 **Ferramentas** | Keycloak (IAM open-source) rodando localmente via Podman |
| 📚 **Referência** | Princípio do privilégio mínimo (least privilege) · RBAC |
| 📦 **Entregável** | Realm da CloudFlux com usuários, roles e política de senha testada |

---

## 🏢 Cenário

A exposição em gestão de identidade e acesso aparece desde a [Matriz de Risco (Projeto 01)](../01-matriz-de-risco/), mas nunca foi priorizada. A [Auditoria (Projeto 02)](../02-auditoria-de-controles/) confirmou o problema na **RNC 01**: o RBAC estava em rascunho (v0.3), cobrindo só Engenharia e Produto, sem aprovação da diretoria.

O estopim veio com o [incidente do Projeto 04](../04-gestao-de-incidente-jira/). Uma credencial comprometida tentou acessar o banco de clientes e só foi barrada pelo RBAC do sistema interno. O controle técnico funcionou, mas a estrutura formal de identidade não existia.

A CloudFlux decide, então, centralizar a gestão de identidade: usuários, papéis e políticas num lugar só.

---

## 🧰 Por que Keycloak

A rota original seria o Microsoft Entra ID, mas o programa de desenvolvedor não liberou elegibilidade e a conta gratuita do Azure exige cartão de crédito. O **Keycloak** entrega a mesma lógica de IAM, é open-source e roda localmente.

```bash
podman run -p 8080:8080 \
  -e KEYCLOAK_ADMIN=admin \
  -e KEYCLOAK_ADMIN_PASSWORD=admin \
  quay.io/keycloak/keycloak:latest start-dev
```

⚠️ Credenciais padrão e modo `start-dev` servem apenas para ambiente local de estudo, e nunca para produção.

---

## 🏛️ Estrutura criada

**Realm `cloudflux`**, um espaço isolado representando a empresa, com os seus próprios usuários, roles e políticas.

### 🎭 Roles e atribuição

Cada role foi desenhado a partir da pergunta: **o que essa pessoa precisa ver ou fazer para exercer a função, nem mais nem menos?**

| Role | Usuário | Área | Lógica de acesso |
|---|---|---|---|
| `admin` | Marina Tavares | Segurança da Informação | Administração completa, restrita a quem responde pela segurança |
| `infraestrutura` | Rafael Nogueira | Infraestrutura | Acesso técnico ao ambiente, sem poder administrativo sobre identidades |
| `usuario-padrao` | Beatriz Andrade | RH | Ferramentas do dia a dia da área, sem acesso técnico |
| `usuario-padrao` | Thiago Martins | Financeiro | Ferramentas do dia a dia da área, sem acesso técnico |
| `executivo` | Fernanda Lima | CFO | Visão estratégica e financeira, sem privilégio técnico |
| `auditoria` | Camila Rocha | Compliance | **Leitura ampla, sem poder de alteração** |

💡 **Por que não um RBAC genérico de três níveis:** nem todo perfil fora de TI é "usuário padrão". Uma CFO precisa enxergar números estratégicos que um analista não precisa, e o Compliance precisa ver quase tudo, mas não pode mudar nada, porque quem audita não deveria poder alterar o que audita. Por isso nasceram os roles `executivo` e `auditoria`.

### 🧱 Realm, Role e Group

- 🏛️ **Realm** é isolamento total, como empresas diferentes que não compartilham nada.
- 👥 **Group** é uma subdivisão dentro do mesmo Realm, como os departamentos de uma empresa.
- 🎭 **Role** é o nível de permissão, e define **o que** a pessoa pode fazer, independente de onde ela está.

---

## 🔑 Política de senha

Configurada em **Authentication → Policies**:

| Regra | Valor |
|---|---|
| Tamanho mínimo | 8 caracteres |
| Letra maiúscula | Pelo menos 1 |
| Número | Pelo menos 1 |
| Caractere especial | Pelo menos 1 |

### 🧪 O teste

Depois de configurar, a política foi **testada tentando violá-la**: a senha `1234` foi rejeitada pelo sistema. Uma política só existe de verdade quando alguém tenta quebrá-la e não consegue. Configurar e presumir que funciona é repetir a lógica do "controle declarado" que a [Auditoria](../02-auditoria-de-controles/) e o [Incidente](../04-gestao-de-incidente-jira/) mostraram ser insuficiente.

---

## 🔧 O que a ferramenta ensinou

- 🔒 **HTTPS por padrão.** O Keycloak exige HTTPS até para acesso local. Foi resolvido acessando por `127.0.0.1` em vez de `localhost`.
- 🗺️ **A política de senha muda de lugar.** Nesta versão, ela fica em Authentication → Policies, e não em Realm Settings, onde aparecem só papéis e grupos padrão. Vale conferir de novo se a versão mudar.

---

## ✅ O que este projeto demonstra

- Aplicar **privilégio mínimo** na prática, e não só no papel.
- Desenhar roles a partir da **necessidade real de cada função**, indo além de um RBAC genérico.
- Aplicar **segregação de funções**: quem audita tem leitura ampla e não altera.
- Validar um controle **testando a sua violação**.

## 🔜 Próximo nível

A política de senha fortalece a credencial, mas a causa raiz do Projeto 04 foi **roubo de sessão**, que passa por cima de senha forte. Os próximos controles naturais no Keycloak seriam:
- ⏱️ **Tempo de expiração de sessão** (inatividade e duração máxima), para reduzir a janela de uso de uma sessão roubada.
- 🚫 **Detecção de força bruta**, com bloqueio após tentativas falhas.
- 📲 **MFA obrigatório** (OTP) no próprio provedor de identidade.

## 🧵 Fechando o ciclo

Risco mapeado no **01** → confirmado incompleto na **02** (RNC 01) → incidente real na **04** → identidade formalizada aqui, na **05**.

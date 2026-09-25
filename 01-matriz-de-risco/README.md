# 📊 01 · Matriz de Gestão de Risco

> Primeiro projeto da série CloudFlux: estruturar do zero a gestão de riscos de TI de uma empresa que nunca teve isso documentado.

| | |
|---|---|
| 🗂️ **Tipo** | Projeto simulado |
| 📚 **Referências** | ISO 31000 (processo de gestão de risco), NIST (categorização), matriz 5x5 |
| 📦 **Entregáveis** | Risk Register · Matriz de Risco · Plano de Tratamento |
| ⏱️ **Prazo simulado** | 1 semana |

---

## 🏢 Cenário

A **CloudFlux** é um SaaS B2B de gestão de projetos em nuvem, com cerca de 180 funcionários. Cresceu rápido, sempre priorizando tecnologia em vez de processo, e trata os problemas de TI à medida que aparecem.

🚨 **O gatilho:** o cliente que representa a maior fatia do faturamento passou a exigir **evidência de gestão de riscos de TI formalizada** como condição para renovar o contrato. O board pediu a entrega em uma semana, antes da reunião com o cliente.

📌 **Escopo definido:** de 6 a 8 riscos, cobrindo no mínimo 3 categorias diferentes.

---

## 📋 1. Risk Register

| ID | Risco | Categoria | Áreas envolvidas | Situação encontrada |
|---|---|---|---|---|
| R01 | Gestão de acesso | Segurança | TI + RH | Permissões concedidas sob demanda, sem revisão periódica nem privilégio mínimo. Pessoas sem necessidade técnica mantêm acesso administrativo a bancos de produção. |
| R02 | Segurança da nuvem | Segurança | TI + Infraestrutura | Proteção limitada ao firewall nativo da nuvem (bloqueio por IP), sem WAF contra ataques na camada de aplicação. Desenvolvedores sem MFA. |
| R03 | Segurança da rede | Segurança | TI + Infraestrutura | Trabalho híbrido sem VPN obrigatória, sem monitoramento centralizado de tráfego e com protocolos de criptografia obsoletos. |
| R04 | Segurança de dados | Segurança | TI + Infraestrutura + Jurídico/Compliance | Sem classificação de dados nem criptografia em repouso. O backup fica isolado em outra nuvem (ponto positivo), mas a restauração nunca é testada. |
| R05 | Segurança do servidor | Segurança | TI + Infraestrutura | A infraestrutura física é da AWS, mas não há política de patches nem monitoramento contínuo de logs nos servidores. |
| R06 | Recuperação de desastres | Operacional / Disponibilidade | TI + Infraestrutura + Board | Não existe DRP, papéis definidos para momentos de crise, métricas de RTO/RPO ou alinhamento documentado com o suporte da AWS. |
| R07 | Cultura de segurança | Segurança (fator humano) | TI + RH | A conscientização é superficial. Não há treinamento recorrente nem simulação de phishing. |
| R08 | Riscos de terceiros | Terceiros / Compliance | TI + Jurídico/Compliance | Fornecedores e integrações nunca foram avaliados, e não existe homologação de segurança antes de contratar. |

⚠️ **Impactos mapeados.** Os impactos dos 8 riscos convergem em três frentes:
- 💸 Perda do contrato com o maior cliente por descumprimento de governança.
- ⚖️ Sanções da ANPD por violação da LGPD.
- 📉 Indisponibilidade da plataforma, com quebra de SLA e dano à reputação.

---

## 🧭 2. Matriz de Risco (Probabilidade × Impacto)

| Probabilidade ↓ · Impacto → | 1 | 2 | 3 | 4 | 5 |
|:---:|:---:|:---:|:---:|:---:|:---:|
| **5** | | | | 🔴 R01 | |
| **4** | | | | 🔴 R02 · R03 | 🔴 R04 |
| **3** | | | 🟠 R05 | 🟠 R07 | 🔴 R06 |
| **2** | | | | | |
| **1** | | | | 🟡 R08 | |

🔴 **Significativo:** ação imediata &nbsp;·&nbsp; 🟠 **Moderado:** prioridade alta &nbsp;·&nbsp; 🟡 **Tolerável:** monitoramento &nbsp;·&nbsp; ⚪ **Baixo:** aceitável

### 🔍 Leitura da matriz

- **R01 e R04, zona crítica.** Com 180 pessoas e permissões sem controle, um acesso indevido é quase certo. Isso se agrava pela falta de criptografia e pelo backup que nunca foi testado. O impacto seria perder os maiores clientes e sofrer multas da ANPD.
- **R02 e R03, perímetro externo.** O produto é 100% online e o time trabalha de casa sem VPN, então ataques automatizados e interceptação de tráfego são prováveis. Os dois riscos serão **tratados juntos** para economizar custo e tempo.
- **R06, impacto máximo mesmo com probabilidade média.** A estabilidade da AWS não cobre falhas que nascem dentro da operação, como erro humano, falha de configuração, ransomware ou bug em produção. Sem plano, o time não teria como reagir.
- **R07, porta sempre aberta.** Um único clique em phishing entrega credenciais legítimas, e o fator humano é o elo mais frágil da empresa.
- **R05, impacto amortecido.** A estrutura da AWS permite subir servidores reserva rapidamente, o que reduz o dano a um esforço de recuperação emergencial.
- **R08, governança preventiva.** Um ataque via fornecedor é raro no curto prazo, mas integrações por API sem nenhuma auditoria precisam de critério à medida que a empresa cresce.

---

## 🛠️ 3. Plano de Tratamento

| Risco | P × I | Estratégia | Ação imediata | Ação estrutural |
|---|:---:|:---:|---|---|
| R01 · Gestão de acesso | 5×4 | Mitigar | Levantamento emergencial com o RH para revogar contas de colaboradores desligados | Política formal de controle de acesso baseada em **RBAC**, com privilégio mínimo |
| R04 · Segurança de dados | 4×5 | Mitigar | Diagnóstico dos bancos para planejar criptografia em repouso e revisar a rotina de backup | Política de classificação de dados e teste automatizado de restauração de backup |
| R05 · Segurança do servidor ⚡ | 3×3 | Mitigar | Aplicar os patches pendentes pelo painel da AWS e ativar as automações de segurança nativas | Política de manutenção contínua, com cronograma fixo de atualização e auditoria |
| R03 · Segurança da rede | 4×4 | Mitigar | Mapear vulnerabilidades nas conexões remotas e aplicar contenção temporária de tráfego | Arquitetura padrão de rede e **VPN obrigatória** no modelo híbrido |
| R02 · Segurança da nuvem | 4×4 | Mitigar | Fechar portas vulneráveis no firewall atual e cotar um WAF | Monitoramento contínuo da camada de aplicação e requisitos mínimos de perímetro (**WAF**) |
| R06 · Recuperação de desastres | 3×5 | Transferir | Cotação emergencial com consultorias especializadas | Consultoria entrega o **DRP** oficial, com simulação anual |
| R07 · Cultura de segurança | 3×4 | Mitigar | Bloco de 15 minutos no All-Hands e comunicado nos canais internos | Programa contínuo de treinamento, com simulação de phishing de 2 a 3 vezes por ano |
| R08 · Riscos de terceiros | 1×4 | Aceitar / Monitorar | Mapear os fornecedores integrados e classificá-los por criticidade | Política de terceiros com questionário de homologação e cláusulas de privacidade em contrato |

⚡ *Quick win:* R05 tem criticidade menor que R02 e R03, mas foi antecipado porque pode ser resolvido direto pelo painel da AWS. Isso garante proteção imediata enquanto as soluções de perímetro, mais complexas, são avaliadas.

### 💬 Justificativa

- **Mitigar como regra.** A empresa precisa criar controles internos que reduzam a exposição atual sem travar a operação dos 180 colaboradores.
- **Transferir o R06.** A CloudFlux não tem hoje capacidade interna para desenhar um DRP do zero no curto prazo, então essa construção vai para uma consultoria especializada.
- **Guia consultivo, não imposição.** O plano foi montado junto com a liderança e o time de TI, para que a diretoria decida de acordo com o orçamento e o apetite a risco da empresa.

---

## ✅ O que este projeto demonstra

- Aplicação do ciclo da ISO 31000: **identificar → analisar → avaliar → tratar**.
- Priorização por **probabilidade × impacto**, e não pelo impacto isolado.
- Tradução de risco técnico em **impacto de negócio**: contrato, SLA e LGPD.
- Escolha de tratamento considerando **custo e capacidade interna** (quick win e transferência).

## 🔜 Próximo nível

Ficaram identificados como evolução natural deste projeto quatro pontos:
- Quantificação financeira dos riscos.
- Risco residual.
- Um **risk owner** individual para cada risco (hoje a responsabilidade está atribuída a áreas).
- Data de reavaliação.

A reavaliação virou o gatilho do projeto seguinte: **[02 · Auditoria de Controles](../02-auditoria-de-controles/)**, seis meses depois, verificando se esses controles saíram do papel.

# 🛠️ 03 · Change Management do WAF via ServiceNow

> A auditoria encontrou o WAF desligado em produção. Ligar ele de volta não podia ser mais uma mudança feita sem processo.

| | |
|---|---|
| 🗂️ **Tipo** | Projeto simulado, com registro real na ferramenta |
| 🧰 **Ferramenta** | ServiceNow (instância de desenvolvedor, PDI), aprendida do zero |
| 📚 **Referência** | ITIL 4 · Change Enablement |
| 📦 **Entregável** | Change Request **CHG0030001**, do registro ao fechamento |

---

## 🏢 Cenário

Na [Auditoria (Projeto 02)](../02-auditoria-de-controles/), a **RNC 02** mostrou que o WAF da CloudFlux funcionava apenas em staging: em produção, nenhuma das 42 regras estava aplicada. A ação imediata recomendada era ativar o pacote básico **OWASP Top 10** em produção.

Como é uma mudança com potencial de derrubar tráfego legítimo de clientes, ela passou pelo fluxo formal de change management antes de ir para o ar.

---

## 📋 Classificação da mudança

| Campo | Valor |
|---|---|
| Tipo | **Normal**, com CAB obrigatório |
| Categoria | Network |
| Item de configuração | CAROL3-GATEWAY |
| Risco | High |
| Impacto | 1 · High |
| Prioridade | 2 · High |
| Grupo responsável | Network |

**Descrição:** aplicação de segurança na camada 7 para mitigar vulnerabilidades OWASP Top 10 e filtrar tráfego malicioso na aplicação da CloudFlux.

**Justificativa:** conformidade com os padrões de segurança e proteção dos serviços em nuvem contra ataques externos.

⚠️ **Risco da própria mudança:** bloquear tráfego legítimo de clientes (falso positivo) ou deixar passar ataque (falso negativo), com impacto direto na disponibilidade do serviço web.

---

## 🧭 Plano da mudança

### 🪜 Implementação em fases

Staging → **Log-only** (só registra, não bloqueia) → análise de falsos positivos por 24h → **Active blocking**

A fase de log-only é o que protege o cliente: as regras rodam observando o tráfego real antes de terem poder de bloquear qualquer coisa.

### 🧪 Plano de teste

| Teste | Resultado esperado |
|---|---|
| Requisições HTTP/HTTPS legítimas | `200 OK`, o tráfego normal passa |
| SQL Injection e XSS simulados, em ambiente controlado | `403 Forbidden`, o ataque é bloqueado |

Testar os dois lados garante que o WAF bloqueia o que deve **e** não atrapalha o que não deve.

### ↩️ Plano de rollback

1. Reverter a política do WAF para Bypass/Disabled pelo console.
2. Limpar o cache de sessões bloqueadas.
3. Validar que o tráfego da aplicação voltou ao normal.

---

## ✅ Aprovação e fechamento

- 🗓️ **CAB em 07/09/2026:** aprovado **com a condição** de ter o time de redes de plantão durante a janela.
- 🕘 **Janela planejada:** 12/09, 21h → 13/09, 00h (fora do horário comercial).
- 👥 **Aprovações:** 7 aprovadas (6 do CAB e 1 do grupo Network).
- 🧩 **Change Tasks:** implementação (CTASK0010002) e teste pós-implementação (CTASK0010001), ambas fechadas.
- 🏁 **Close code:** Successful. Regras aplicadas e testes validados.

📄 **Registro completo exportado do ServiceNow:** [CHG0030001.pdf](CHG0030001.pdf)

---

## 🔧 O que a ferramenta ensinou

Primeiro contato com ServiceNow, sem curso formal. Os ajustes feitos no caminho foram parte do aprendizado:

- 🎯 **Coerência de classificação.** O impacto estava como Low enquanto risco e prioridade eram High. Foi corrigido para High, alinhando os três campos, porque esse tipo de inconsistência é o primeiro a saltar aos olhos de quem audita o registro depois.
- 🧩 **Change Task não fecha sozinha.** Quando o change avançou de fase, o sistema cancelou automaticamente as tasks que não tinham sido fechadas. Elas foram reabertas e fechadas uma a uma, com close code e notas próprias.
- 🗓️ **Ruído do ambiente de demonstração.** O conflito de agenda apontado vinha de uma janela de manutenção pré-cadastrada na PDI, sem relação com o cenário. Foi documentado em work notes, em vez de forçado a resolver.
- 👥 **Regra de quórum.** Quatro aprovadores do grupo Network ficaram como "No Longer Required" depois que as aprovações do CAB bastaram para avançar. Foi identificado como comportamento legítimo do workflow, e não como aprovação pulada, e também documentado.

> 📝 **Nota de transparência:** no registro exportado, a data de execução real ficou como 14/04 → 15/04, e não dentro da janela planejada. É uma inconsistência de preenchimento, identificada na revisão deste portfólio, quando a instância de teste já havia expirado. No cenário, a execução acontece na janela de 12/09 → 13/09 aprovada pelo CAB.

---

## ✅ O que este projeto demonstra

- Aplicar **ITIL 4 Change Enablement**: classificar, avaliar risco, planejar, aprovar via CAB, testar e fechar.
- Desenhar uma implementação **faseada** que reduz o risco da própria mudança.
- Planejar teste positivo e negativo (`200` e `403`) e um rollback concreto.
- Diferenciar **erro de preenchimento** de **comportamento legítimo da ferramenta**, documentando cada um.

## 🔜 Desdobramentos na série CloudFlux

- 🔙 Esta mudança responde à **RNC 02** da [02 · Auditoria de Controles](../02-auditoria-de-controles/).
- 🚨 Próximo capítulo: **[04 · Gestão de Incidente via Jira](../04-gestao-de-incidente-jira/)**, uma tentativa de acesso indevido que coloca à prova outro controle declarado como pronto na auditoria, a VPN.

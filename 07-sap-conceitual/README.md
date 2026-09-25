# 🏢 07 · SAP e ERP · Estudo Conceitual

> 📘 **Estudo conceitual, sem hands-on.** Mesmo em versão de teste, o SAP exige infraestrutura muito além de um ambiente doméstico de estudo. Este módulo cobre os conceitos que GRC precisa entender sobre ERP, sem simulação na ferramenta.

| | |
|---|---|
| 🗂️ **Tipo** | Rodada de estudos conceituais |
| 🎯 **Foco** | Como um ERP funciona, quem trabalha nele e por que GRC se importa com ele |

---

## 🧱 O que é ERP e onde o SAP entra

Um **ERP** (Enterprise Resource Planning) é o sistema que integra os processos centrais de uma empresa (financeiro, compras, estoque, RH, vendas) numa base única. O **SAP** virou sinônimo de ERP de grande porte, embora hoje também existam versões menores, como o SAP Business One.

### 🕰️ ECC x S/4HANA

| | SAP ECC | SAP S/4HANA |
|---|---|---|
| Geração | Versão antiga | Versão atual |
| Onde aparece | Ainda comum em empresas com sistema legado | Novas implementações e migrações |
| Interface | Clássica | Fiori, mais moderna |

---

## 👥 As três camadas de quem trabalha com SAP

| Camada | O que faz | Exemplo |
|---|---|---|
| ⚙️ **Basis** | Infraestrutura: servidor, banco de dados, segurança técnica | Manter o ambiente no ar e controlar acessos técnicos |
| 🧩 **Consultor Funcional** | Configura regras de negócio pela tela, sem código | Definir uma regra de aprovação de compras |
| 💻 **Desenvolvedor ABAP** | Programa o que a configuração pronta não cobre | Criar uma funcionalidade customizada |

---

## 🗺️ SAP, TOTVS e Coupa

- 🌍 **SAP:** ERP completo, de alcance global.
- 🇧🇷 **TOTVS:** ERP brasileiro, forte nas obrigações fiscais e trabalhistas nacionais.
- 🛒 **Coupa:** não é um ERP completo, e sim um sistema específico de gestão de gastos e compras.

---

## ⚖️ Por que GRC se importa: Segregação de Funções

O conceito central deste estudo é a **Segregação de Funções** (SoD, Segregation of Duties): a mesma pessoa não deve conseguir completar sozinha, do início ao fim, um processo sensível.

🔎 **Exemplo clássico:** quem cadastra um fornecedor não pode ser a mesma pessoa que aprova o pagamento para ele. Se puder, abre-se espaço para fraude sem que ninguém perceba.

Num ERP, onde todos os processos da empresa estão integrados, o controle de acesso deixa de ser só "quem entra no sistema" e passa a ser **"quais combinações de permissão uma mesma pessoa pode ter"**. É por isso que auditoria de acessos em ERP é um tema tão presente em GRC.

🔗 A mesma lógica aparece no [Projeto 05 · Keycloak](../05-gestao-de-acesso-keycloak/), no role `auditoria`: leitura ampla, sem poder de alteração, porque quem audita não deveria poder mudar o que audita.

---

## 🛍️ Conexão com experiência prática

Antes deste estudo, já houve contato com ERPs de menor escala no varejo, incluindo um sistema mais antigo, voltado a lançamento de notas fiscais, e outro mais moderno, na web, com controle de estoque e caixa. A lógica de integrar processos numa base única é a mesma, e a principal diferença para o SAP é a escala.

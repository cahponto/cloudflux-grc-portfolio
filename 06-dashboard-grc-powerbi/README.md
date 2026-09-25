# 📊 06 · Dashboard de Indicadores de GRC via Power BI

> 🚧 **Em evolução.** Este projeto está numa fase de exploração inicial da ferramenta e ainda não tem a versão final do dashboard publicada aqui.

| | |
|---|---|
| 🗂️ **Tipo** | Projeto simulado · exploração de ferramenta |
| 🧰 **Ferramenta** | Power BI Service (versão web) |
| 📦 **Objetivo** | Dashboard de maturidade de GRC para o Board da CloudFlux |

---

## 🎯 A ideia

Depois de meses de matriz de risco, auditoria, change management, incidente e formalização de identidade, o Board da CloudFlux pediu que GRC passasse a reportar a maturidade da empresa **de forma contínua e visual**, em vez de aparecer só quando algo dá errado ou quando chega época de auditoria.

A proposta é reunir numa base simples os dados que já existem nos projetos anteriores, principalmente os resultados da [Auditoria (Projeto 02)](../02-auditoria-de-controles/), e transformá-los em **KPIs** para a liderança: status dos 8 controles, não conformidades por área responsável e controles totalmente conformes.

O critério de escolha é de **storytelling com dado**: mostrar o que a liderança precisa ver para entender a maturidade da empresa em poucos segundos, e não todo dado que existe.

Um dos indicadores já conta uma história importante: as RNCs **não são só um problema de TI**. Elas passam por Financeiro, RH, Compras e Infraestrutura.

---

## 🔍 O que foi explorado até aqui

- 📈 Visuais básicos: gráfico de pizza com o status dos controles, gráfico de barras com as RNCs por área e cartão numérico de controles conformes.
- 🐧 **Power BI Desktop não roda em Linux**, então o trabalho foi feito no Power BI Service, pelo navegador.
- 📥 O upload de CSV e XLSX travava no Service. A saída foi criar as tabelas manualmente pelo recurso **Entrar Dados**.
- 🧱 Tabelas criadas pelo Entrar Dados no navegador não abrem o Power Query Editor completo, então não é possível editar os dados depois. É uma limitação da versão web em relação ao Desktop.

---

## 🔜 Próximos passos

- Montar a versão final do dashboard com os indicadores da Auditoria.
- Publicar prints do resultado nesta pasta.

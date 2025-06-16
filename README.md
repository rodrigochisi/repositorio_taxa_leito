# repositorio_taxa_leito

🧠 Valor Entregue
💡 Forneceu visão diária da ocupação hospitalar por unidade e especialidade

🚨 Lógica visual destacando risco de operação acima do limite

✅ Totalmente validado com o cliente e o sistema

📈 Base para decisões operacionais, redirecionamento de pacientes e alocação de recursos


# 🏥 Painel de Gerenciamento de Leitos – Oracle + QuickSight

> Projeto real de monitoramento da taxa de ocupação de leitos hospitalares, desenvolvido com Oracle SQL e Amazon QuickSight.

---

## 📌 Objetivo

Fornecer uma visão em tempo real da **taxa de ocupação hospitalar** por unidade, com destaque para leitos ativos, ocupados, vagos e pacientes com alta médica. Também foram incluídos limites operacionais por unidade e um histórico mensal de ocupação.

---

## 🧩 Ferramentas Utilizadas

- Oracle SQL (criação de view com regras específicas por unidade)
- Amazon QuickSight (dashboard com KPIs, cálculos e gráficos)
- Validação cruzada com sistema MV e cliente final

---
## 
- % Ocupados: sum(OCUPADO) / sum(LEITOS)
- Leitos Limitados  ( Somente para uma unidade ) ifelse(
  max(CODIGOMULTIEMPRESA) = 9 AND {LIMITAR_LEITOS} <= 87, 
  1,
  ifelse( max(CODIGOMULTIEMPRESA) <> 9 , 1, 0)
)

## 📊 Visual no Painel
Indicadores incluídos:

% de Ocupação por Unidade

Leitos disponíveis x bloqueados x ocupados

Gráficos de histórico de ocupação

Destaque em vermelho para empresas com limiar abaixo do mínimo



## 🧱 Lógica Técnica

### View no Oracle

Criei uma view nomeada `Taxa_Leitos.sql`, que realiza:

- Agrupamento por unidade (`CODIGOMULTIEMPRESA`)
- Soma de leitos ativos, ocupados, livres e bloqueados
- Cálculo da porcentagem de ocupação
- Campo especial para controlar unidades com limitação de leitos:


```sql
CASE 
  WHEN codigomultiempresa = 9 AND total_leitos <= 87 THEN 1
  WHEN codigomultiempresa <> 9 THEN 1
  ELSE 0
END AS limitar_leitos 

# 📊 Projeto Prático: Análise de Campanhas de Marketing com SQL

Este repositório documenta uma sessão intensiva de desenvolvimento prático em **SQL**, utilizando uma base de dados de marketing com mais de **64 mil linhas**. 

O projeto simula um ambiente real de análise de dados, cobrindo desde consultas básicas de filtragem e ordenação até relatórios gerenciais avançados utilizando **PostgreSQL** e **DBeaver**.

---

## 🎯 Contexto e Metodologia

O fluxo de trabalho deste portfólio abrange:
1. **Fundamentos de Consulta:** Extração cirúrgica de dados com `SELECT`, filtros condicionais (`WHERE`, `BETWEEN`, `IN`) e ordenação (`ORDER BY`).
2. **Agregações e Métricas:** Uso de funções estatísticas (`SUM`, `AVG`, `COUNT`, `MAX`) combinadas com o agrupamento de dados (`GROUP BY`).
3. **Filtros Pós-Agrupamento:** Aplicação de restrições em métricas consolidadas utilizando o `HAVING`.
4. **Validação Técnica:** Boas práticas de sintaxe, arredondamento de valores e conversão explícita de tipos de dados.

---

## 💻 Consultas e Casos de Negócio Resolvidos

### 1. Visualizando as primeiras linhas com limite
* **Conceito aplicado:** Projeção de colunas específicas e controle de exibição com `LIMIT`.

```sql
SELECT channel, offer 
FROM marketing_data md 
LIMIT 10;
```
2. Filtrando dados textuais específicos
Conceito aplicado: Filtragem exata de strings usando WHERE.
```sql
SELECT *
FROM marketing_data md
WHERE md.zip_code = 'Urban';
```
3. Filtrando valores numéricos e ordenação ascendente
Conceito aplicado: Operadores de comparação (>) combinados com ORDER BY ASC.
```sql
SELECT history 
FROM marketing_data md
WHERE history > 500
ORDER BY history ASC;
```
4. Múltiplas condições lógicas (E / AND)
Conceito aplicado: Cruzamento de condições textuais e numéricas com AND.
```sql
SELECT channel, conversion 
FROM marketing_data md
WHERE channel = 'Web' AND conversion = 1;
```
5. Condições alternativas (OU / OR)
Conceito aplicado: Uso do operador lógico OR para múltiplas opções de texto.
```sql
SELECT channel, offer, history 
FROM marketing_data md
WHERE offer = 'Discount' OR offer = 'Buy One Get One'
ORDER BY offer;
```
6. Filtrando faixas numéricas com intervalo (BETWEEN)
Conceito aplicado: Seleção de registros dentro de um intervalo com BETWEEN e ordenação decrescente.
```sql
SELECT history 
FROM marketing_data md
WHERE md.history BETWEEN 100 AND 200
ORDER BY md.history DESC;
```
7. Encontrando o teto de uma métrica (MAX)
Conceito aplicado: Função de agregação para encontrar o valor máximo da base.
```sql
SELECT MAX(md.history) AS maior_historico 
FROM marketing_data md;
```
8. Canal de marketing com maior volume total de conversões
Conceito aplicado: Agrupamento básico (GROUP BY) e soma (SUM).
```sql
SELECT 
    md.channel,
    SUM(conversion) AS total_conversoes
FROM marketing_data md
GROUP BY md.channel
ORDER BY total_conversoes DESC;
```
9. Desempenho por canal considerando campanhas BOGO (used_bogo)
Conceito aplicado: Filtro de linha pré-agrupamento.
```sql
SELECT 
    md.channel,
    SUM(conversion) AS total_conversoes
FROM marketing_data md 
WHERE used_bogo = 1
GROUP BY channel
ORDER BY total_conversoes DESC;
```
10. Campanhas com estratégias combinadas (BOGO + Desconto)
Conceito aplicado: Múltiplas restrições lógicas com AND e agregações.
```sql
SELECT 
    md.channel,
    SUM(conversion) AS total_conversoes
FROM marketing_data md 
WHERE used_bogo = 1 AND used_discount = 1
GROUP BY channel 
ORDER BY total_conversoes DESC;
```
11. Histórico médio de compras por canal (com arredondamento)
Conceito aplicado: Cálculo de média (AVG), arredondamento (ROUND) e conversão de tipo (::numeric).
```sql
SELECT 
    md.channel,
    ROUND(AVG(history)::numeric, 2) AS avg_historico
FROM marketing_data md
GROUP BY md.channel
ORDER BY avg_historico DESC;
```
12. Filtragem avançada com múltiplos valores (IN)
Conceito aplicado: Seleção de múltiplos subconjuntos textuais com o operador IN.
```sql
SELECT 
    md.channel,
    SUM(conversion) AS total_conversion
FROM marketing_data md
WHERE channel IN ('Web', 'Phone')
GROUP BY md.channel 
ORDER BY total_conversion DESC;
```
13. Relatório Gerencial Completo por Canal (Múltiplas Agregações)
Conceito aplicado: Uso simultâneo de COUNT(*), SUM, AVG e ROUND por grupo.
```sql
SELECT 
    md.channel,
    COUNT(*) AS total_linhas,
    SUM(conversion) AS total_conversoes,
    ROUND(AVG(history)::numeric, 2) AS media_historico
FROM marketing_data md
GROUP BY md.channel
ORDER BY total_conversoes DESC;
```
14. Filtrando dados pós-agrupamento (HAVING)
Conceito aplicado: Uso do HAVING para aplicar regras em cima de valores agregados.
```sql
SELECT 
    md.offer,
    ROUND(AVG(history)::numeric, 2) AS media_historico
FROM marketing_data md
GROUP BY md.offer
HAVING ROUND(AVG(history)::numeric, 2) > 1
ORDER BY media_historico;
```
15. Contagem de clientes convertidos por zona postal (zip_code)
Conceito aplicado: Filtro WHERE combinado com contagem agrupada.
```sql
SELECT 
    md.zip_code,
    COUNT(*) AS total_clientes
FROM marketing_data md
WHERE md.conversion = 1
GROUP BY md.zip_code
ORDER BY total_clientes DESC;
```
16. Agrupamento Duplo (Cruzamento de Canal e Oferta)
Conceito aplicado: Agrupamento por duas colunas simultaneamente no GROUP BY.
```sql
SELECT 
    channel, 
    offer,
    SUM(conversion) AS total_conversoes
FROM marketing_data md
GROUP BY md.channel, md.offer
ORDER BY total_conversoes DESC;
```
17. Filtrando indicações e canais específicos com limite
Conceito aplicado: Condições restritivas e paginação de dados.
```sql
SELECT 
    channel, 
    history, 
    is_referral 
FROM marketing_data md
WHERE md.is_referral = 1 AND md.channel = 'Phone'
LIMIT 15;
```
🛠️ Tecnologias e Ferramentas Utilizadas
Banco de Dados: PostgreSQL

Ferramenta de Gestão: DBeaver

Linguagem: SQL

Visão geral do ambiente de desenvolvimento e consultas estruturadas:




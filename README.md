# 📊 Projeto Prático: Análise de Campanhas de Marketing com SQL

Este repositório documenta uma sessão de prática guiada e desenvolvimento prático em **SQL**, utilizando uma base de dados de marketing com mais de **64 mil linhas**. 

O projeto simula um ambiente real de análise de dados, onde desafios de negócio foram propostos passo a passo, estruturados, codificados e validados utilizando **PostgreSQL** e **DBeaver**.

---

## 🎯 Contexto e Metodologia
Nesta jornada prática, o fluxo de trabalho consistiu em:
1. **Definição do Desafio:** Apresentação de perguntas de negócios voltadas para otimização de campanhas comerciais.
2. **Resolução Autônoma:** Construção e refinamento das consultas SQL para responder a cada métrica.
3. **Validação Técnica:** Aplicação de boas práticas de sintaxe, ordenação, funções de agregação, filtros condicionais e manipulação de tipos de dados.

---

## 💻 Consultas e Casos de Negócio Resolvidos

-- 1. Visualizando as primeiras linhas com limite
SELECT channel, offer 
FROM marketing_data md 
LIMIT 10;

-- 2. Filtrando dados textuais específicos
SELECT *
FROM marketing_data md
WHERE md.zip_code = 'Urban';

-- 3. Filtrando valores numéricos e ordenação ascendente
SELECT history 
FROM marketing_data md
WHERE history > 500
ORDER BY history ASC;

-- 4. Múltiplas condições lógicas (E / AND)
SELECT channel, conversion 
FROM marketing_data md
WHERE channel = 'Web' AND conversion = 1;

-- 5. Condições alternativas (OU / OR)
SELECT channel, offer, history 
FROM marketing_data md
WHERE offer = 'Discount' OR offer = 'Buy One Get One'
ORDER BY offer;

-- 6. Filtrando faixas numéricas com intervalo (BETWEEN)
SELECT history 
FROM marketing_data md
WHERE md.history BETWEEN 100 AND 200
ORDER BY md.history DESC;

-- 7. Encontrando o teto de uma métrica (MAX)
SELECT MAX(md.history) AS maior_historico 
FROM marketing_data md;

-- 8. Canal de marketing com maior volume total de conversões
SELECT 
    md.channel,
    SUM(conversion) AS total_conversoes
FROM marketing_data md
GROUP BY md.channel
ORDER BY total_conversoes DESC;

-- 9. Desempenho por canal considerando campanhas BOGO (used_bogo)
SELECT 
    md.channel,
    SUM(conversion) AS total_conversoes
FROM marketing_data md 
WHERE used_bogo = 1
GROUP BY channel
ORDER BY total_conversoes DESC;

-- 10. Campanhas com estratégias combinadas (BOGO + Desconto)
SELECT 
    md.channel,
    SUM(conversion) AS total_conversoes
FROM marketing_data md 
WHERE used_bogo = 1 AND used_discount = 1
GROUP BY channel 
ORDER BY total_conversoes DESC;

-- 11. Histórico médio de compras por canal (com arredondamento)
SELECT 
    md.channel,
    ROUND(AVG(history)::numeric, 2) AS avg_historico
FROM marketing_data md
GROUP BY md.channel
ORDER BY avg_historico DESC;

-- 12. Filtragem avançada com múltiplos valores (IN)
SELECT 
    md.channel,
    SUM(conversion) AS total_conversion
FROM marketing_data md
WHERE channel IN ('Web', 'Phone')
GROUP BY md.channel 
ORDER BY total_conversion DESC;

-- 13. Relatório Gerencial Completo por Canal (Múltiplas Agregações)
SELECT 
    md.channel,
    COUNT(*) AS total_linhas,
    SUM(conversion) AS total_conversoes,
    ROUND(AVG(history)::numeric, 2) AS media_historico
FROM marketing_data md
GROUP BY md.channel
ORDER BY total_conversoes DESC;

-- 14. Filtrando dados pós-agrupamento (HAVING)
SELECT 
    md.offer,
    ROUND(AVG(history)::numeric, 2) AS media_historico
FROM marketing_data md
GROUP BY md.offer
HAVING ROUND(AVG(history)::numeric, 2) > 1
ORDER BY media_historico;

-- 15. Contagem de clientes convertidos por zona postal (zip_code)
SELECT 
    md.zip_code,
    COUNT(*) AS total_clientes
FROM marketing_data md
WHERE md.conversion = 1
GROUP BY md.zip_code
ORDER BY total_clientes DESC;

-- 16. Agrupamento Duplo (Cruzamento de Canal e Oferta)
SELECT 
    channel, 
    offer,
    SUM(conversion) AS total_conversoes
FROM marketing_data md
GROUP BY md.channel, md.offer
ORDER BY total_conversoes DESC;

-- 17. Filtrando indicações e canais específicos com limite
SELECT 
    channel, 
    history, 
    is_referral 
FROM marketing_data md
WHERE md.is_referral = 1 AND md.channel = 'Phone'
LIMIT 15;
---

## 🛠️ Tecnologias e Ferramentas Utilizadas
* **Banco de Dados:** PostgreSQL
* **Ferramenta de Gestão:** DBeaver
* **Linguagem:** SQL

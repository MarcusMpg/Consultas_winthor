```sql
SELECT 
    M.CODPROD,  
    SUM(M.QT) AS QT_VENDIDA, 
    P.DESCRICAO 
FROM 
    PCMOV M, 
    PCPRODUT P
WHERE 
    M.CODPROD = P.CODPROD -- Faltava relacionar as duas tabelas!
    AND M.DTMOV BETWEEN TO_DATE('01/05/2026', 'DD/MM/YYYY') AND TO_DATE('18/05/2026', 'DD/MM/YYYY')
    AND M.CODOPER = 'S'
   
GROUP BY 
    M.CODPROD, 
    P.DESCRICAO -- Sempre inclua colunas de texto não agregadas no GROUP BY
ORDER BY 
    M.CODPROD;
```

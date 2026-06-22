SELECT 
    I.NUMPED,
    C.DATA,
    C.CODUSUR AS cod_rca,
    U.NOME AS nome_rca, 
    C.CODPLPAG,
    I.CODPROD,
    P.DESCRICAO AS produto,
    I.PTABELA AS preco_tabela,
    I.PVENDA AS preco_venda_praticado,
    -- ABS transforma qualquer resultado negativo em positivo
    ABS(ROUND(
        CASE 
            WHEN NVL(I.PTABELA, 0) > 0 
            THEN ((NVL(I.PTABELA, 0) - NVL(I.PVENDA, 0)) / I.PTABELA) * 100 
            ELSE 0 
        END, 2
    )) AS perc_variacao
FROM 
    PCPEDI I
    INNER JOIN PCPRODUT P ON (I.CODPROD = P.CODPROD)
    INNER JOIN PCPEDC C ON (I.NUMPED = C.NUMPED)
    INNER JOIN PCUSUARI U ON (C.CODUSUR = U.CODUSUR) 
WHERE 
    C.DATA >= TO_DATE('18/05/2026', 'DD/MM/YYYY')         
    AND I.PVENDA > I.PTABELA                  
    AND C.CODPLPAG = 33    
    AND C.CODUSUR IN (349, 377, 71, 379, 383, 330)
ORDER BY 
    I.NUMPED DESC

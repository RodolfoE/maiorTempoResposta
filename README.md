# maiorTempoResposta
Função de SQL Server que obtem o maior tempo de tempos da tabela de logs

DECLARE @mTabela table(url nvarchar(max), tempo float)
declare @mUrl nvarchar(max)
DECLARE mLogs cursor for 
select distinct url from logs 
OPEN mLogs
FETCH NEXT FROM mLogs INTO @mUrl;
WHILE @@FETCH_STATUS = 0  
    BEGIN
        declare @media FLOAT
        select @media = AVG(convert(float,Tempo_Resposta)) from logs where url = @mUrl
        insert into @mTabela values (@mUrl, @media)        
        FETCH NEXT FROM mLogs INTO @mUrl;
    END;

select * from @mTabela order by tempo desc
CLOSE mLogs;
DEALLOCATE mLogs;

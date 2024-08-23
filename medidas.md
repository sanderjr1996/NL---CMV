
## Medidas e Cálculos
<div class="table-responsive">
<table border="1" class="dataframe styled-table">
  <thead>
    <tr style="text-align: right;">
      <th>Tabela</th>
      <th>Medida</th>
      <th>Expressao</th>
      <th>Formato</th>
      <th>EstaOculto</th>
      <th>Descricao</th>
      <th>Tipo</th>
      <th>Pasta</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Medidas</td>
      <td>% CMV</td>
      <td>-- Esta medida calcula o % do CMV sobre o faturamento líquido.  DIVIDE([Vlr. Custo], [Vlr. Faturamento Liq.], 0)</td>
      <td>#,0.00%;-#,0.00%;#,0.00%</td>
      <td>False</td>
      <td>Percentual do CMV sobre o faturamento líquido.</td>
      <td>Double</td>
      <td>1 - Principais\1.3 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>% CMV Período Anterior</td>
      <td>-- Esta medida calcula o % CMV no período anterior.  VAR _PeriodoSelecionado = // Armazena o intervalo selecionado     [Txt. Intervalo]  VAR _AnoContexto = // Armazena o ano do contexto.     SELECTEDVALUE(Dim_Calendario[AnoNum])  VAR _SemestreContexto = // Armazena o semestre do contexto.     SELECTEDVALUE(Dim_Calendario[SemestreDoAnoNum])  VAR _Semestre1 =  // Calcula o % do período anterior caso o semestre do contexto for 1.     CALCULATE(         [% CMV],         ALL(Dim_Calendario),         Dim_Calendario[AnoNum] = _AnoContexto - 1,         Dim_Calendario[SemestreDoAnoNum] = 2     )  VAR _Semestre2 = // Calcula o % do período anterior caso o semestre do contexto for 2.     CALCULATE(         [% CMV],         ALL(Dim_Calendario),         Dim_Calendario[AnoNum] = _AnoContexto,         Dim_Calendario[SemestreDoAnoNum] = 1     )  VAR _Trimestre = // Calcula o % do trimestre anterior.     CALCULATE(         [% CMV],          PREVIOUSQUARTER(Dim_Calendario[Data])     )  VAR _Mes =  // Calcula o % do mês anterior.     CALCULATE(         [% CMV],          PREVIOUSMONTH(Dim_Calendario[Data])     )  VAR _Ano =  // Calcula o mesmo periodo do ano anterior.     CALCULATE(         [% CMV],          SAMEPERIODLASTYEAR(Dim_Calendario[Data])     )  VAR _PeriodoAnterior =  // Avalia o contexto de intervalo para trazer o calculo adequado.     SWITCH(         TRUE(),         _PeriodoSelecionado = "Mês", _Mes,         _PeriodoSelecionado = "Trimestre", _Trimestre,         _PeriodoSelecionado = "Semestre" &amp;&amp; SELECTEDVALUE(Dim_Calendario[SemestreDoAnoNum]) = 1, _Semestre1,         _PeriodoSelecionado = "Semestre" &amp;&amp; SELECTEDVALUE(Dim_Calendario[SemestreDoAnoNum]) = 2, _Semestre2,         _Ano     )  RETURN      _PeriodoAnterior</td>
      <td>0.00%;-0.00%;0.00%</td>
      <td>False</td>
      <td>Calcula o % CMV do período anterior ao contexto atual.</td>
      <td>Double</td>
      <td>5 - Insights\5.2 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>% CMV Reserva</td>
      <td>-- Esta medida calcula o % do CMV sobre a reserva líquida.  VAR _ReservaLiq = // Calcula o valor total das notas fiscais de bonificações para clientes, faturamento direto e reserva de estoque após a dedução das devoluções de venda.     CALCULATE(         [Vlr. Nota],         Dim_Top[CODTIPOPER] IN {3200, 3205, 3208, 3107, 3111, 3112},         USERELATIONSHIP(Dim_Calendario[Data], Fat_Notas[DTFATUR])     ) - [Vlr. Dev. Venda]  VAR _CMVReserva = // Calcula o % do CMV sobre a reserva líquida.     DIVIDE([Vlr. Custo], _ReservaLiq, 0)  RETURN     _CMVReserva</td>
      <td>#,0.00%;-#,0.00%;#,0.00%</td>
      <td>False</td>
      <td>Percentual do CMV sobre a reserva de estoque.</td>
      <td>Double</td>
      <td>1 - Principais\1.3 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>% Diferença CMV</td>
      <td>-- Calcula a diferença do % CMV atual em relação ao custo do período anterior.  VAR _Ano =  // Armazena o ano minimo da tabela calendário     CALCULATE(         MIN(Dim_Calendario[AnoNum]),         ALL(Dim_Calendario)     )  VAR _Diferenca = // Calcula a diferença de % CMV, exceto se o ano anterior for o mesmo da variavel _Ano.     IF(         SELECTEDVALUE(Dim_Calendario[AnoNum]) = _Ano,         0,         [% CMV] - [% CMV Período Anterior]     )  RETURN     _Diferenca</td>
      <td>0.00%;-0.00%;0.00%</td>
      <td>False</td>
      <td>Calcula a diferença do % CMV do período anterior em relação ao contexto atual.</td>
      <td>Double</td>
      <td>5 - Insights\5.2 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>% Diferença Custo</td>
      <td>-- Calcula o % de diferença do custo do periodo analisado em relação ao periodo anterior.   DIVIDE([Vlr. Diferença Custo], [Vlr. Custo Período Ant.], BLANK())</td>
      <td>0.00%;-0.00%;0.00%</td>
      <td>False</td>
      <td>% da diferença do custo atual em relação ao período anterior.</td>
      <td>Double</td>
      <td>5 - Insights\5.2 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>% Diferença Fat. Líquido</td>
      <td>-- Calcula o % de diferença do faturamento líquido do periodo analisado em relação ao periodo anterior.   DIVIDE([Vlr. Dif. Faturamento Liq.], [Vlr. Faturamento Liq. Periodo Ant.], BLANK())</td>
      <td>0.00%;-0.00%;0.00%</td>
      <td>False</td>
      <td>% da diferença do faturamento líquido atual em relação ao período anterior.</td>
      <td>Double</td>
      <td>5 - Insights\5.2 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>% Margem</td>
      <td>-- Caclula o % de margem bruta por produto.  DIVIDE([Vlr. Margem], [Vlr. Preço Médio], BLANK())</td>
      <td>0.00%;-0.00%;0.00%</td>
      <td>False</td>
      <td>% de margem bruta por produto.</td>
      <td>Double</td>
      <td>1 - Principais\1.3 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>% MarkUp</td>
      <td>-- Esta medida calcula o % de MarkUp.  IF(     DIVIDE([Vlr. Faturamento Liq.], [Vlr. Custo],  0) = 0,      BLANK(),     DIVIDE([Vlr. Faturamento Liq.], [Vlr. Custo],  0) - 1 )</td>
      <td>#,0.00%;-#,0.00%;#,0.00%</td>
      <td>False</td>
      <td>Percentual de rentabilidade.</td>
      <td>Double</td>
      <td>1 - Principais\1.3 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>% Participação Compra</td>
      <td>-- Esta medida calcula o % de participação sobre a compra total.  DIVIDE([Vlr. Compra], [Vlr. Compra Tot.], 0)</td>
      <td>0.00%;-0.00%;0.00%</td>
      <td>False</td>
      <td>% de participação sobre a compra total.</td>
      <td>Double</td>
      <td>1 - Principais\1.3 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>% Participação Faturamento Liq.</td>
      <td>-- Esta medida calcula o % de participação sobre o faturamento líquido total.  DIVIDE([Vlr. Faturamento Liq.], [Vlr. Faturamento Liq. Tot.], 0)</td>
      <td>#,0.00%;-#,0.00%;#,0.00%</td>
      <td>False</td>
      <td>Percentual de participação em relação ao faturamento líquido total.</td>
      <td>Double</td>
      <td>1 - Principais\1.3 - Percentuais</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Classif. ABC</td>
      <td>-- Realiza a classificação de curva ABC dos produtos de acordo com sua participação no faturamento acumulado.   VAR _RealizadoContexto = // Armazena o faturamento líquido do contexto.     [Vlr. Faturamento Liq.]  VAR _RealizadoAcumulado = // Acumula o valor de faturamento líquido linha a linha até o contexto atual.     CALCULATE(         [Vlr. Faturamento Liq.],         FILTER(             ALLSELECTED(Dim_Produto[GRUPO]),             [Vlr. Faturamento Liq.] &gt;= _RealizadoContexto         )     )  VAR _Pareto = // Calcula o % do faturamento líquido acumulado em relação ao faturamento líquido total.     ROUND(DIVIDE(_RealizadoAcumulado, [Vlr. Faturamento Liq. Tot.], 0),0)  VAR _ClassificacaoABC = // Realiza a classificação ABC de acordo com a faixa de % que se encaixa a varável _Pareto.     SWITCH(         TRUE(),         NOT(HASONEVALUE(Dim_Produto[GRUPO])), BLANK(),         NOT(_RealizadoContexto = BLANK()) &amp;&amp; _Pareto &lt;= 0.80, "A",         NOT(_RealizadoContexto = BLANK()) &amp;&amp; _Pareto &lt;= 0.95, "B",         NOT(_RealizadoContexto = BLANK()) &amp;&amp; _Pareto &lt;= 1.00, "C",         BLANK()     )  RETURN     _ClassificacaoABC</td>
      <td></td>
      <td>False</td>
      <td>Classificação de curva ABC.</td>
      <td>String</td>
      <td>4 - Classificações</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Cor Rotulo CMV</td>
      <td>-- Medida utilizada para formatação de campo, se % CMV &lt; 0.78, retorna 1, se não, 0.  IF([% CMV] &lt;= 0.78, 1, 0)</td>
      <td>#,0</td>
      <td>False</td>
      <td>Formatação condicional para a medida [% CMV].</td>
      <td>Integer</td>
      <td>2 - UX</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Cor Rótulo CMV Reserva</td>
      <td>-- Medida utilizada para formatação de campo, se % CMV Reserva &lt; 0.78, retorna 1, se não, 0.  IF([% CMV Reserva] &lt;= 0.78, 1, 0)</td>
      <td>#,0</td>
      <td>False</td>
      <td>Formatação condicional para a medida [% CMV Reserva].</td>
      <td>Integer</td>
      <td>2 - UX</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Cor Rótulo MarkUp</td>
      <td>-- Medida utilizada para formatação de campo, se % MarkUp &gt;= 0.28, retorna 1, se não, 0.  IF([% MarkUp] &gt;= 0.28, 1, 0)</td>
      <td>#,0</td>
      <td>False</td>
      <td>Formatação condicional para a medida [% Rentabilidade].</td>
      <td>Integer</td>
      <td>2 - UX</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Dt. Estoque Fin.</td>
      <td>-- Calcula a data final do estoque com base no contexto do mês selecionado/filtrado.   CALCULATE(      MAX(Fat_ContEstoque[DTCONTAGEM]),     ALL(Dim_Produto),     ALL(Dim_Calendario),     Fat_ContEstoque[DTCONTAGEM] &lt;= MAX((Dim_Calendario[Data])) )</td>
      <td>Short Date</td>
      <td>False</td>
      <td>Data do estoque final.</td>
      <td>DateTime</td>
      <td>3 - Datas</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Dt. Estoque Ini.</td>
      <td>-- Calcula a data inicial do estoque com base no contexto do mês selecionado/filtrado.   CALCULATE(      MAX(Fat_ContEstoque[DTCONTAGEM]),     ALL(Dim_Calendario),      ALL(Dim_Produto),     Fat_ContEstoque[DTCONTAGEM] &lt; MIN (Dim_Calendario[Data]) )</td>
      <td>Short Date</td>
      <td>False</td>
      <td>Data do estoque inicial.</td>
      <td>DateTime</td>
      <td>3 - Datas</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Dt. Ultima Atualização</td>
      <td>-- Traz a Data e Hora da última atualização dos dados.  SELECTEDVALUE('Aux_Ultima_Atualização'[ULTIMA_ATUALIZAÇÃO])</td>
      <td></td>
      <td>False</td>
      <td>Data da ultima atualização do modelo semântico.</td>
      <td>String</td>
      <td>3 - Datas</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Qtd. Compra</td>
      <td>-- Esta medida calcula o valor total de notas fiscais de compras.  CALCULATE(     [Qtd. Nota],     Dim_Top[CODTIPOPER] IN {2100, 2105, 2112, 2115, 2117, 2121, 2136, 2141, 2142},     ALL(Dim_MotivoCanc) )</td>
      <td>#,0</td>
      <td>False</td>
      <td>Quantidade de compras.</td>
      <td>Double</td>
      <td>1 - Principais\1.2 - Quantidades</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Qtd. Faturamento</td>
      <td>-- Esta medida calcula o valor total de notas fiscais de faturamento.  CALCULATE(     [Qtd. Nota],     Dim_Top[CODTIPOPER] IN {3200, 3205, 3208, 3211, 3232},     ALL(Dim_MotivoCanc) )</td>
      <td>#,0</td>
      <td>False</td>
      <td>Quantidade de faturamento.</td>
      <td>Double</td>
      <td>1 - Principais\1.2 - Quantidades</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Qtd. Nota</td>
      <td>-- Esta medida calcula a quantidade total das notas fiscais.  SUM(Fat_Notas[QTDNEG])</td>
      <td>#,0</td>
      <td>False</td>
      <td>Quantidades das notas fiscais.</td>
      <td>Double</td>
      <td>1 - Principais\1.2 - Quantidades</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Rótulo Vlr. Compra</td>
      <td>-- Faz a formatação do Vlr. Compra para que seja exibida com unidade de milhar personalizada. (Código na aba Formato, ao lado.)  [Vlr. Compra]</td>
      <td></td>
      <td>False</td>
      <td>Rótulo personalizado de Compra.</td>
      <td>Double</td>
      <td>2 - UX</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Rótulo Vlr. Compra | % Participação</td>
      <td>-- Faz a formatação do Vlr. Compra para que seja exibida junto ao %. (Código na aba Formato, ao lado.)  [Vlr. Compra]</td>
      <td></td>
      <td>False</td>
      <td>Rótulo personalizado de Compra. Valor e % de participação.</td>
      <td>Double</td>
      <td>2 - UX</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Diminuição/Aumento CMV</td>
      <td>-- Retorna um texto de acordo com a condicional.  IF(     [% Diferença CMV] &lt; 0,     "Diminuição",     "Aumento" )</td>
      <td></td>
      <td>False</td>
      <td>Condicional que retorna texto de acordo com o aumento ou diminuição do CMV.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Diminuição/Aumento Custo</td>
      <td>-- Retorna um texto de acordo com a condicional.  IF(     [Vlr. Diferença Custo] &lt; 0,     "Diminuição",     "Aumento" )</td>
      <td></td>
      <td>False</td>
      <td>Condicional que retorna texto de acordo com o aumento ou diminuição do custo.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Diminuição/Aumento Fat. Liquido</td>
      <td>-- Retorna um texto de acordo com a condicional.  IF(     [Vlr. Dif. Faturamento Liq.] &lt; 0,     "Diminuição",     "Aumento" )</td>
      <td></td>
      <td>False</td>
      <td>Condicional que retorna texto de acordo com o aumento ou diminuição do faturamento líquido.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Insights HTML</td>
      <td>-- Cadeia de texto em HTML para ser exibida no visual "Descobertas" da aba CMV.  VAR _AnaliseTemporal = // Texto de analise temporal.     "&lt;div style='font-family:Calibri; font-size:16px; color: #e6e6e6; text-align: justify; margin: 10px;'&gt;         &lt;b&gt;💡 Em relação ao "&amp;[Txt. Intervalo]&amp;" anterior, tivemos:&lt;/b&gt;     &lt;/div&gt;" &amp;     "&lt;div style='font-family:Calibri; font-size:14px; color: #e6e6e6'&gt;         &lt;p style='margin:0; padding-left :30px;'&gt;📌 "&amp;[Txt. Diminuição/Aumento CMV]&amp;" de "&amp;FORMAT([% Diferença CMV],"Percent")&amp;" no nosso CMV;&lt;/p&gt;         &lt;p style='margin:0; padding-left :30px;'&gt;📌 "&amp;[Txt. Diminuição/Aumento Custo]&amp;" no custo de "&amp;FORMAT([Vlr. Diferença Custo],"R$ #,0")&amp;" ("&amp;FORMAT([% Diferença Custo],"#,0.00%")&amp;")"&amp;";&lt;/p&gt;         &lt;p style='margin:0; padding-left :30px;'&gt;📌 "&amp;[Txt. Diminuição/Aumento Fat. Liquido]&amp;" no fat. liquido de "&amp;FORMAT([Vlr. Dif. Faturamento Liq.],"R$ #,0")&amp;" ("&amp;FORMAT([% Diferença Fat. Líquido],"#,0.00%")&amp;")"&amp;".&lt;/p&gt;     &lt;/div&gt;"      VAR _AnaliseOfensores = // Texto de analise de ofensores.       "&lt;div style='font-family:Calibri; font-size:16px; color: #e6e6e6; text-align: justify; margin: 10px;'&gt;         &lt;b&gt;💡 Em relação aos produtos de Curva A, que representam 80% de nosso faturamento, os principais ofensores no "&amp;[Txt. Intervalo]&amp;" analisado foram:&lt;/b&gt;     &lt;/div&gt;" &amp;     "&lt;div style='font-family:Calibri; font-size:14px; color: #e6e6e6'&gt;         &lt;p style='margin:0; padding-left :30px;'&gt;📌 "&amp;[Txt. Top 1 Ofensor Curva A]&amp;";&lt;/p&gt;         &lt;p style='margin:0; padding-left :30px;'&gt;📌 "&amp;[Txt. Top 2 Ofensor Curva A]&amp;";&lt;/p&gt;         &lt;p style='margin:0; padding-left :30px;'&gt;📌 "&amp;[Txt. Top 3 Ofensor Curva A]&amp;".&lt;/p&gt;     &lt;/div&gt;" &amp;     "&lt;div style='font-family:Calibri; font-size:16px; color: #e6e6e6; text-align: justify; margin: 10px;'&gt;         &lt;b&gt;💡 Em relação aos produtos de Curva B e C, que representam 20% de nosso faturamento, os principais ofensores no "&amp;[Txt. Intervalo]&amp;" analisado foram:&lt;/b&gt;     &lt;/div&gt;" &amp;     "&lt;div style='font-family:Calibri; font-size:14px; color: #e6e6e6'&gt;         &lt;p style='margin:0; padding-left :25px;'&gt;📌 "&amp;[Txt. Top 1 Ofensor Curva B e C]&amp;";&lt;/p&gt;         &lt;p style='margin:0; padding-left :25px;'&gt;📌 "&amp;[Txt. Top 2 Ofensor Curva B e C]&amp;";&lt;/p&gt;         &lt;p style='margin:0; padding-left :25px;'&gt;📌 "&amp;[Txt. Top 3 Ofensor Curva B e C]&amp;".&lt;/p&gt;     &lt;/div&gt;"  VAR _Html = // Montador de texto de acordo com o contexto atual.     IF(         ISFILTERED(Dim_Produto[GRUPO]),         _AnaliseTemporal,         _AnaliseTemporal &amp; _AnaliseOfensores     )  RETURN     _Html</td>
      <td></td>
      <td>False</td>
      <td>Texto em formato HTML para o visual "Descobertas da aba CMV"</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Intervalo</td>
      <td>-- Retorna um texto de acordo com a condicional.  IF(          HASONEFILTER(Dim_Calendario[MesNomeAbrev]) ||         HASONEFILTER(Dim_Calendario[TrimestreAnoNome]) ||          HASONEFILTER(Dim_Calendario[SemestreAnoNome])     ,     MAX('Aux_Parâmetro'[Parâmetro]),     "Ano" )</td>
      <td></td>
      <td>False</td>
      <td>Condicional que retorna texto com o tipo de intervalo de acordo com o filtro aplicado.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Top 1 Ofensor Curva A</td>
      <td>-- Retorna um texto contendo o top 1 Ofensor da Cuva A.  VAR _Tabela = // Retorna uma tarefa sumarizada por Grupo de produto, Curva ABV e % CMV.     SUMMARIZE(         Dim_Produto,         Dim_Produto[GRUPO],         "ABC",         [Classif. ABC],         "CMV",         [% CMV]     )  VAR _TabelaFiltrada = // Aplica filtros na tabela.     FILTER(         _Tabela,         [Classif. ABC] = "A" &amp;&amp; [CMV] &gt;= 0.78     )  VAR _TabelaOrdenada = // Aplica ranqueamento para ordenação da tabela.     ADDCOLUMNS(         _TabelaFiltrada,         "Rank",         RANKX(             _TabelaFiltrada,             [CMV],             ,             DESC,             DENSE         )     )  VAR _PrimeiroColocadoNome = // Retorna o nome do primeiro colocado.     SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 1         ),         "GRUPO", [GRUPO]     )  VAR _PrimeiroColocadoPercent = // Retorna o % CMV do primeiro colocado.     SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 1         ),         "CMV", [CMV]     )  VAR _Texto = // Retorna um texto formatado.     _PrimeiroColocadoNome &amp; " (" &amp; FORMAT(_PrimeiroColocadoPercent, "#,0.00%") &amp; ")"  RETURN     _Texto</td>
      <td></td>
      <td>False</td>
      <td>Retorna o Nome e Percentual do top 1 ofensor da curva A.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Top 1 Ofensor Curva B e C</td>
      <td>-- Retorna um texto contendo o top 1 Ofensor da Cuva B e C.  VAR _Tabela = // Retorna uma tarefa sumarizada por Grupo de produto, Curva ABV e % CMV.     SUMMARIZE(         Dim_Produto,         Dim_Produto[GRUPO],         "ABC",         [Classif. ABC],         "CMV",         [% CMV]     )  VAR _TabelaFiltrada = // Aplica filtros na tabela.     FILTER(         _Tabela,         [Classif. ABC] IN{"B", "C"} &amp;&amp; [CMV] &gt;= 0.78     )  VAR _TabelaOrdenada = // Aplica ranqueamento para ordenação da tabela.     ADDCOLUMNS(         _TabelaFiltrada,         "Rank",         RANKX(             _TabelaFiltrada,             [CMV],             ,             DESC,             DENSE         )     )  VAR _PrimeiroColocadoNome = // Retorna o nome do primeiro colocado.      SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 1         ),         "GRUPO", [GRUPO]     )  VAR _PrimeiroColocadoPercent = // Retorna o % CMV do primeiro colocado.      SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 1         ),         "CMV", [CMV]     )  VAR _Texto = // Retorna um texto formatado.     _PrimeiroColocadoNome &amp; " (" &amp; FORMAT(_PrimeiroColocadoPercent, "#,0.00%") &amp; ")"  RETURN     _Texto</td>
      <td></td>
      <td>False</td>
      <td>Retorna o Nome e Percentual do top 1 ofensor da curva B e C.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Top 2 Ofensor Curva A</td>
      <td>-- Retorna um texto contendo o top 2 Ofensor da Cuva A.  VAR _Tabela = // Retorna uma tarefa sumarizada por Grupo de produto, Curva ABV e % CMV.     SUMMARIZE(         Dim_Produto,         Dim_Produto[GRUPO],         "ABC",         [Classif. ABC],         "CMV",         [% CMV]     )  VAR _TabelaFiltrada = // Aplica filtros na tabela.     FILTER(         _Tabela,         [Classif. ABC] = "A" &amp;&amp; [CMV] &gt;= 0.78     )  VAR _TabelaOrdenada = // Aplica ranqueamento para ordenação da tabela.     ADDCOLUMNS(         _TabelaFiltrada,         "Rank",         RANKX(             _TabelaFiltrada,             [CMV],             ,             DESC,             DENSE         )     )  VAR _SegundoColocadoNome = // Retorna o nome do segundo colocado.     SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 2         ),         "GRUPO", [GRUPO]     )  VAR _SegundoColocadoPercent = // Retorna o % CMV do segundo colocado.     SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 2         ),         "CMV", [CMV]     )  VAR _Texto = // Retorna um texto formatado.     _SegundoColocadoNome &amp; " (" &amp; FORMAT(_SegundoColocadoPercent, "#,0.00%") &amp; ")"  RETURN     _Texto</td>
      <td></td>
      <td>False</td>
      <td>Retorna o Nome e Percentual do top 2 ofensor da curva A.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Top 2 Ofensor Curva B e C</td>
      <td>-- Retorna um texto contendo o top 2 Ofensor da Cuva B e C.  VAR _Tabela = // Retorna uma tarefa sumarizada por Grupo de produto, Curva ABV e % CMV.     SUMMARIZE(         Dim_Produto,         Dim_Produto[GRUPO],         "ABC",         [Classif. ABC],         "CMV",         [% CMV]     )  VAR _TabelaFiltrada = // Aplica filtros na tabela.      FILTER(         _Tabela,         [Classif. ABC] IN{"B", "C"} &amp;&amp; [CMV] &gt;= 0.78     )  VAR _TabelaOrdenada = // Aplica ranqueamento para ordenação da tabela.     ADDCOLUMNS(         _TabelaFiltrada,         "Rank",         RANKX(             _TabelaFiltrada,             [CMV],             ,             DESC,             DENSE         )     )  VAR _SegundoColocadoNome = // Retorna o nome do segundo colocado.      SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 2         ),         "GRUPO", [GRUPO]     )  VAR _SegundoColocadoPercent = // Retorna o % CMV do segundo colocado.      SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 2         ),         "CMV", [CMV]     )  VAR _Texto = // Retorna um texto formatado.     _SegundoColocadoNome &amp; " (" &amp; FORMAT(_SegundoColocadoPercent, "#,0.00%") &amp; ")"  RETURN     _Texto</td>
      <td></td>
      <td>False</td>
      <td>Retorna o Nome e Percentual do top 2 ofensor da curva B e C.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Top 3 Ofensor Curva A</td>
      <td>-- Retorna um texto contendo o top 3 Ofensor da Cuva A.  VAR _Tabela = // Retorna uma tarefa sumarizada por Grupo de produto, Curva ABV e % CMV.     SUMMARIZE(         Dim_Produto,         Dim_Produto[GRUPO],         "ABC",         [Classif. ABC],         "CMV",         [% CMV]     )  VAR _TabelaFiltrada = // Aplica filtros na tabela.      FILTER(         _Tabela,         [Classif. ABC] = "A" &amp;&amp; [CMV] &gt;= 0.78     )  VAR _TabelaOrdenada = // Aplica ranqueamento para ordenação da tabela.     ADDCOLUMNS(         _TabelaFiltrada,         "Rank",         RANKX(             _TabelaFiltrada,             [CMV],             ,             DESC,             DENSE         )     )  VAR _TerceiroColocadoNome = // Retorna o nome do tereiro colocado.      SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 3         ),         "GRUPO", [GRUPO]     )  VAR _TerceiroColocadoPercent = // Retorna o % CMV do terceiro colocado.       SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 3         ),         "CMV", [CMV]     )  VAR _Texto = // Retorna um texto formatado.     _TerceiroColocadoNome &amp; " (" &amp; FORMAT(_TerceiroColocadoPercent, "#,0.00%") &amp; ")"  RETURN     _Texto</td>
      <td></td>
      <td>False</td>
      <td>Retorna o Nome e Percentual do top 3 ofensor da curva A.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Txt. Top 3 Ofensor Curva B e C</td>
      <td>-- Retorna um texto contendo o top 3 Ofensor da Cuva B e C.  VAR _Tabela = // Retorna uma tarefa sumarizada por Grupo de produto, Curva ABV e % CMV.     SUMMARIZE(         Dim_Produto,         Dim_Produto[GRUPO],         "ABC",         [Classif. ABC],         "CMV",         [% CMV]     )  VAR _TabelaFiltrada = // Aplica filtros na tabela.     FILTER(         _Tabela,         [Classif. ABC] IN{"B", "C"} &amp;&amp; [CMV] &gt;= 0.78     )  VAR _TabelaOrdenada = // Aplica ranqueamento para ordenação da tabela.     ADDCOLUMNS(         _TabelaFiltrada,         "Rank",         RANKX(             _TabelaFiltrada,             [CMV],             ,             DESC,             DENSE         )     )  VAR _TerceiroColocadoNome = // Retorna o nome do tereiro colocado.      SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 3         ),         "GRUPO", [GRUPO]     )  VAR _TerceiroColocadoPercent = // Retorna o % CMV do terceiro colocado.      SELECTCOLUMNS(         FILTER(             _TabelaOrdenada,             [Rank] = 3         ),         "CMV", [CMV]     )  VAR _Texto = // Retorna um texto formatado.     _TerceiroColocadoNome &amp; " (" &amp; FORMAT(_TerceiroColocadoPercent, "#,0.00%") &amp; ")"  RETURN     _Texto</td>
      <td></td>
      <td>False</td>
      <td>Retorna o Nome e Percentual do top 3 ofensor da curva B e C.</td>
      <td>String</td>
      <td>5 - Insights\5.3 - Texto</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Bon. Compra</td>
      <td>-- Esta medida calcula o valor total das notas fiscais de bonificação de compras.  CALCULATE(     [Vlr. Nota],     Dim_Top[CODTIPOPER] IN {2106, 2107} )</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor de bonificações recebidas dos fornecedores.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Compra</td>
      <td>-- Esta medida calcula o valor total de notas fiscais de compras.  CALCULATE(     [Vlr. Nota],     Dim_Top[CODTIPOPER] IN {2100, 2105, 2112, 2115, 2117, 2121, 2136, 2141, 2142},     ALL(Dim_MotivoCanc) )</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor de compras.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Compra Tot.</td>
      <td>-- Esta medida calcula o valor total de compra sem considerar o contexto de produtos e fornecedor.  CALCULATE(      [Vlr. Compra],      ALLSELECTED(Dim_Produto),     ALL(Dim_Parceiro) )</td>
      <td></td>
      <td>False</td>
      <td>Valor total de compra.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Consumo Interno</td>
      <td>-- Esta medida calcula o valor total das notas fiscais para o refeitório (Consumo interno).  CALCULATE(     [Vlr. Nota],     Dim_Top[CODTIPOPER] = 2157,     USERELATIONSHIP(Dim_Calendario[Data], Fat_Notas[DTFATUR]) )</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor de consumo interno do refeitório.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Custo</td>
      <td>-- Esta medida calcula o valor total de custos das mercadorias vendidas.  [Vlr. Estoque Ini.] + [Vlr. Compra] + [Vlr. Custos Ad.] - [Vlr. Bon. Compra] - [Vlr. Dev. Compra] - [Vlr. Consumo Interno] - [Vlr. Perda Liq.] - [Vlr. Estoque Fin.]</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor do custo da mercadoria vendida.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Custo Médio</td>
      <td>-- Calcula o custo médio ponderado dos produtos.  DIVIDE([Vlr. Compra], [Qtd. Compra], BLANK())</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor do custo médio ponderado dos produtos.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Custo Médio p/ Margem</td>
      <td>-- Calcula o custo médio ponderado dos produtos para fazer o calculo de margem na tela de Margem.  VAR _VlrCompra = // Calcula o valor da compra, removendo filtro de parceiro e nota fiscal.     CALCULATE(         [Vlr. Compra],         ALL(Dim_Parceiro),         ALL(Fat_Notas[NUNOTA])     )  VAR _QtdCompra = // Calcula a quantidade da compra, removendo filtro de parceiro e nota fiscal.     CALCULATE(         [Qtd. Compra],         ALL(Dim_Parceiro),         ALL(Fat_Notas[NUNOTA])     )  VAR _CustoMedio = // Calcula o custo médio e retorna vazio caso o preço medio seja vazio.     IF(         NOT([Vlr. Preço Médio] = BLANK()),         DIVIDE(_VlrCompra, _QtdCompra, BLANK()),         BLANK()     )  RETURN     _CustoMedio</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor do custo médio ponderado dos produtos desconsiderando filtros de parceiro e nota fiscal.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Custo Período Ant.</td>
      <td>-- Esta medida calcula o valor do custo no período anterior.  VAR _IntervaloSelecionado = // Armazena o intervalo selecionado     [Txt. Intervalo]  VAR _AnoContexto = // Armazena o ano do contexto.     SELECTEDVALUE(Dim_Calendario[AnoNum])  VAR _SemestreContexto = // Armazena o semestre do contexto.     SELECTEDVALUE(Dim_Calendario[SemestreDoAnoNum])  VAR _Semestre1 = // Calcula o valor do período anterior caso o semestre do contexto for 1.     CALCULATE(         [Vlr. Custo],         ALL(Dim_Calendario),         Dim_Calendario[AnoNum] = _AnoContexto - 1,         Dim_Calendario[SemestreDoAnoNum] = 2     )  VAR _Semestre2 = // Calcula o valor do período anterior caso o semestre do contexto for 2.     CALCULATE(         [Vlr. Custo],         ALL(Dim_Calendario),         Dim_Calendario[AnoNum] = _AnoContexto,         Dim_Calendario[SemestreDoAnoNum] = 1     )  VAR _Trimestre = // Calcula o valor do trimestre anterior.     CALCULATE(         [Vlr. Custo],          PREVIOUSQUARTER(Dim_Calendario[Data])     )  VAR _Mes = // Calcula o valor do mês anterior.     CALCULATE(         [Vlr. Custo],          PREVIOUSMONTH(Dim_Calendario[Data])     )  VAR _Ano = // Calcula o mesmo periodo do ano anterior.     CALCULATE(         [Vlr. Custo],          SAMEPERIODLASTYEAR(Dim_Calendario[Data])     )  VAR _PeriodoAnterior = // Avalia o contexto de intervalo para trazer o calculo adequado.     SWITCH(         TRUE(),         _IntervaloSelecionado = "Mês", _Mes,         _IntervaloSelecionado = "Trimestre", _Trimestre,         _IntervaloSelecionado = "Semestre" &amp;&amp; SELECTEDVALUE(Dim_Calendario[SemestreDoAnoNum]) = 1, _Semestre1,         _IntervaloSelecionado = "Semestre" &amp;&amp; SELECTEDVALUE(Dim_Calendario[SemestreDoAnoNum]) = 2, _Semestre2,         _Ano     )  RETURN      _PeriodoAnterior</td>
      <td>"R$"\ #,0.0;-"R$"\ #,0.0;"R$"\ #,0.0</td>
      <td>False</td>
      <td>Calcula o valor do período anterior.</td>
      <td>Double</td>
      <td>5 - Insights\5.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Custos Ad.</td>
      <td>-- Esta medida calcula o valor total de custos adicionais rateado por produto.  VAR _ProdutosDistintos = // Calcula a contagem de diferentes produtos comprados.     CALCULATE(         DISTINCTCOUNT(Fat_Notas[CODPROD]),         Dim_Top[CODTIPOPER] IN {2100, 2105, 2112, 2115, 2117, 2121, 2136, 2141, 2142},         ALL(Dim_Produto)     )  VAR _CustosAd = // Calcula a quantidade de custos adionais.     CALCULATE(         SUM(Fat_Despesas[VLRRATEIO]),         Dim_Top[CODTIPOPER] IN {2101, 2104, 2108, 2111, 2114, 2118, 2120, 2125, 2150, 2153, 2160, 2102, 2113, 2169, 2165, 2175, 2176},         Dim_Natureza[CODNAT] IN {201220000, 201210000, 201240000, 201240000, 201080000, 201270000, 201230000},         ALL(Dim_Produto)     )  VAR _Rateio = // Realiza o rateio dos custos adicionais entre os diferentes produtos comprados.     SUMX(         VALUES(Dim_Produto[CODPROD]),         IF([Vlr. Compra] &gt; 0, DIVIDE(_CustosAd, _ProdutosDistintos, BLANK()), BLANK())     )  RETURN      _Rateio</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor de custos adicionais ( fretes, operadores logísticos, chapas, e outros ).</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Dev. Compra</td>
      <td>-- Esta medida calcula o valor total de devoluções de compras.  CALCULATE(     [Vlr. Nota],     Dim_Top[CODTIPOPER] = 3000,     USERELATIONSHIP(Dim_Calendario[Data], Fat_Notas[DTFATUR]) )</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor de devolução para os fornecedores.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Dev. Venda</td>
      <td>-- Esta medida calcula o valor total de devoluções de vendas.  CALCULATE(     [Vlr. Nota],     Dim_Top[CODTIPOPER] IN {2200, 2201} )</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor de devoluções dos clientes.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Dif. Faturamento Liq.</td>
      <td>-- Calcula a diferença do faturamento líquido atual em relação ao custo do período anterior.  VAR _Ano = // Armazena o ano minimo da tabela calendário     CALCULATE(         MIN(Dim_Calendario[AnoNum]),         ALL(Dim_Calendario)     )  VAR _Diferenca = // Calcula a diferença de faturamento líquido, exceto se o ano anterior for o mesmo da variavel _Ano.     IF(         SELECTEDVALUE(Dim_Calendario[AnoNum]) = _Ano,         0,         [Vlr. Faturamento Liq.] - [Vlr. Faturamento Liq. Periodo Ant.]     )  RETURN     _Diferenca</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Calcula o valor do faturamento líquido em relação ao período anterior.</td>
      <td>Double</td>
      <td>5 - Insights\5.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Diferença Custo</td>
      <td>-- Calcula a diferença do custo atual em relação ao custo do período anterior.  VAR _Ano = // Armazena o ano minimo da tabela calendário     CALCULATE(         MIN(Dim_Calendario[AnoNum]),         ALL(Dim_Calendario)     )  VAR _Diferenca = // Calcula a diferença de custo, exceto se o ano anterior for o mesmo da variavel _Ano.     IF(         SELECTEDVALUE(Dim_Calendario[AnoNum]) = _Ano,         0,         [Vlr. Custo] - [Vlr. Custo Período Ant.]     )  RETURN     _Diferenca</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Calcula o valor de custo em relação ao período anterior.</td>
      <td>Double</td>
      <td>5 - Insights\5.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Estoque Fin.</td>
      <td>-- Esta medida calcula o valor total do estoque final.  VAR _Data = // Armazena a data do estoque final.     [Dt. Estoque Fin.]   VAR _VlrEstoque = // Calcula o valor do estoque referente a data da variável _Data.     CALCULATE(         SUM(Fat_ContEstoque[CUSTOT]),         Dim_Calendario[Data] = _Data     )  RETURN     _VlrEstoque</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor do estoque final.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Estoque Ini.</td>
      <td>-- Esta medida calcula o valor total do estoque inicial.  VAR _Data = // Armazena a data do estoque inicial.     [Dt. Estoque Ini.]  VAR _VlrEstoque = // Calcula o valor do estoque referente a data da variável _Data.     CALCULATE(         SUM(Fat_ContEstoque[CUSTOT]),         Dim_Calendario[Data] = _Data     )  RETURN     _VlrEstoque</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor do estoque inicial.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Faturamento</td>
      <td>-- Esta medida calcula o valor total das notas fiscais de bonificações para clientes, faturamento direto e PDV.  CALCULATE(     [Vlr. Nota],     Dim_Top[CODTIPOPER] IN {3200, 3205, 3208, 3211, 3232},     USERELATIONSHIP(Dim_Calendario[Data], Fat_Notas[DTFATUR]) )</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor do faturamento ( vendas e bonificações ).</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Faturamento Liq.</td>
      <td>-- Esta medida calcula o valor de faturamento líquido após a dedução das devoluções de venda.  [Vlr. Faturamento] - [Vlr. Dev. Venda]</td>
      <td>"R$"\ #,0.00;-"R$"\ #,0.00;"R$"\ #,0.00</td>
      <td>False</td>
      <td>Valor do faturamento após dedução das devoluções de clientes.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Faturamento Liq. Periodo Ant.</td>
      <td>-- Esta medida calcula o valor do faturamento líquido no período anterior.  VAR _PeriodoSelecionado = // Armazena o intervalo selecionado     [Txt. Intervalo]  VAR _AnoContexto = // Armazena o ano do contexto.     SELECTEDVALUE(Dim_Calendario[AnoNum])  VAR _SemestreContexto = // Armazena o semestre do contexto.     SELECTEDVALUE(Dim_Calendario[SemestreDoAnoNum])  VAR _Semestre1 = // Calcula o valor do período anterior caso o semestre do contexto for 1.     CALCULATE(         [Vlr. Faturamento Liq.],         ALL(Dim_Calendario),         Dim_Calendario[AnoNum] = _AnoContexto - 1,         Dim_Calendario[SemestreDoAnoNum] = 2     )  VAR _Semestre2 = // Calcula o valor do período anterior caso o semestre do contexto for 2.     CALCULATE(         [Vlr. Faturamento Liq.],         ALL(Dim_Calendario),         Dim_Calendario[AnoNum] = _AnoContexto,         Dim_Calendario[SemestreDoAnoNum] = 1     )  VAR _Trimestre = // Calcula o valor do trimestre anterior.      CALCULATE(         [Vlr. Faturamento Liq.],          PREVIOUSQUARTER(Dim_Calendario[Data])     )  VAR _Mes = // Calcula o valor do mês anterior.     CALCULATE(         [Vlr. Faturamento Liq.],          PREVIOUSMONTH(Dim_Calendario[Data])     )  VAR _Ano = // Calcula o mesmo periodo do ano anterior.     CALCULATE(         [Vlr. Faturamento Liq.],          SAMEPERIODLASTYEAR(Dim_Calendario[Data])     )  VAR _PeriodoAnterior = // Avalia o contexto de intervalo para trazer o calculo adequado.     SWITCH(         TRUE(),         _PeriodoSelecionado = "Mês", _Mes,         _PeriodoSelecionado = "Trimestre", _Trimestre,         _PeriodoSelecionado = "Semestre" &amp;&amp; SELECTEDVALUE(Dim_Calendario[SemestreDoAnoNum]) = 1, _Semestre1,         _PeriodoSelecionado = "Semestre" &amp;&amp; SELECTEDVALUE(Dim_Calendario[SemestreDoAnoNum]) = 2, _Semestre2,         _Ano     )  RETURN      _PeriodoAnterior</td>
      <td></td>
      <td>False</td>
      <td>Calcula o valor do faturamento líquido do período anterior</td>
      <td>Double</td>
      <td>5 - Insights\5.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Faturamento Liq. Tot.</td>
      <td>-- Esta medida calcula o valor total de faturamento líquido sem considerar o contexto específico dos produtos.  CALCULATE(      [Vlr. Faturamento Liq.],      ALLSELECTED(Dim_Produto)  )</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor total de faturamento líquido.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Margem</td>
      <td>-- Caclula a margem bruta da venda por produto caso o tenha custo médio, se não, retorna vazio.  IF(     ISBLANK([Vlr. Custo Médio p/ Margem]),     BLANK(),     [Vlr. Preço Médio] - [Vlr. Custo Médio p/ Margem] )</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Faz o calculo do valor da margem bruta da venda por produto.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Nota</td>
      <td>-- Esta medida calcula o valor total das notas fiscais.  SUM(Fat_Notas[VLRTOT])</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor das notas fiscais.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Perda Liq.</td>
      <td>-- Esta medida calcula o valor total de perdas após todas as deduções e tratativas realizadas.  VAR _Perda = // Calcula o valor total perdas.     CALCULATE(         [Vlr. Nota],         Dim_Top[CODTIPOPER] IN {2155, 2156},         USERELATIONSHIP(Dim_Calendario[Data], Fat_Notas[DTFATUR])     )  VAR _DescFin = // Calcula o valor total de descontos financeiros.     CALCULATE(         [Vlr. Nota],         Dim_Top[CODTIPOPER] = 2167     )  VAR _AcordoComercial = // Calcula o valor total de devoluções para fornecedores referente a acordos comerciais.     CALCULATE(         [Vlr. Nota],         Dim_Top[CODTIPOPER] = 3000,         Dim_MotivoCanc[MOTIVOCANC] IN {4, 5},         USERELATIONSHIP(Dim_Calendario[Data], Fat_Notas[DTFATUR])     )  VAR _PerdaLiq = // Calcula o valor da quebra após as deduções das tratativas.     _Perda - [Vlr. Bon. Compra] - _DescFin - _AcordoComercial  RETURN     IF(_PerdaLiq &lt; 0, 0, _PerdaLiq) // Se o valor de quebra no contexto avaliado for negativo, o valor se torna 0.</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor da perda após deduções das tratativas ( desconto financeiro, acordo comercial e bonificações )</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
    <tr>
      <td>Medidas</td>
      <td>Vlr. Preço Médio</td>
      <td>-- Calcula o preço médio ponderado dos produtos.  DIVIDE([Vlr. Faturamento], [Qtd. Faturamento], BLANK())</td>
      <td>"R$"\ #,0;-"R$"\ #,0;"R$"\ #,0</td>
      <td>False</td>
      <td>Valor do preço médio ponderado dos produtos.</td>
      <td>Double</td>
      <td>1 - Principais\1.1 - Valores</td>
    </tr>
  </tbody>
</table>
</div>
        
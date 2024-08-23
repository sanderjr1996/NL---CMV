
## Modelagem e Arquitetura de Dados
<div class="table-responsive">
<table border="1" class="dataframe styled-table">
  <thead>
    <tr style="text-align: right;">
      <th>DeTabela</th>
      <th>DeColuna</th>
      <th>DeCardinalidade</th>
      <th>De</th>
      <th>ParaTabela</th>
      <th>ParaColuna</th>
      <th>ParaCardinalidade</th>
      <th>Para</th>
      <th>EstaAtivo</th>
      <th>Sentido</th>
      <th>DePara</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Fat_ContEstoque</td>
      <td>CODPROD</td>
      <td>*</td>
      <td>'Fat_ContEstoque'[CODPROD]</td>
      <td>Dim_Produto</td>
      <td>CODPROD</td>
      <td>1</td>
      <td>'Dim_Produto'[CODPROD]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_ContEstoque'[CODPROD] * &lt;- 1 'Dim_Produto'[CODPROD]</td>
    </tr>
    <tr>
      <td>Fat_ContEstoque</td>
      <td>DTCONTAGEM</td>
      <td>*</td>
      <td>'Fat_ContEstoque'[DTCONTAGEM]</td>
      <td>Dim_Calendario</td>
      <td>Data</td>
      <td>1</td>
      <td>'Dim_Calendario'[Data]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_ContEstoque'[DTCONTAGEM] * &lt;- 1 'Dim_Calendario'[Data]</td>
    </tr>
    <tr>
      <td>Fat_Despesas</td>
      <td>CODNAT</td>
      <td>*</td>
      <td>'Fat_Despesas'[CODNAT]</td>
      <td>Dim_Natureza</td>
      <td>CODNAT</td>
      <td>1</td>
      <td>'Dim_Natureza'[CODNAT]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_Despesas'[CODNAT] * &lt;- 1 'Dim_Natureza'[CODNAT]</td>
    </tr>
    <tr>
      <td>Fat_Despesas</td>
      <td>CODTIPOPER</td>
      <td>*</td>
      <td>'Fat_Despesas'[CODTIPOPER]</td>
      <td>Dim_Top</td>
      <td>CODTIPOPER</td>
      <td>1</td>
      <td>'Dim_Top'[CODTIPOPER]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_Despesas'[CODTIPOPER] * &lt;- 1 'Dim_Top'[CODTIPOPER]</td>
    </tr>
    <tr>
      <td>Fat_Despesas</td>
      <td>DTENTSAI</td>
      <td>*</td>
      <td>'Fat_Despesas'[DTENTSAI]</td>
      <td>Dim_Calendario</td>
      <td>Data</td>
      <td>1</td>
      <td>'Dim_Calendario'[Data]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_Despesas'[DTENTSAI] * &lt;- 1 'Dim_Calendario'[Data]</td>
    </tr>
    <tr>
      <td>Fat_Notas</td>
      <td>CODPARC</td>
      <td>*</td>
      <td>'Fat_Notas'[CODPARC]</td>
      <td>Dim_Parceiro</td>
      <td>CODPARC</td>
      <td>1</td>
      <td>'Dim_Parceiro'[CODPARC]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_Notas'[CODPARC] * &lt;- 1 'Dim_Parceiro'[CODPARC]</td>
    </tr>
    <tr>
      <td>Fat_Notas</td>
      <td>CODPROD</td>
      <td>*</td>
      <td>'Fat_Notas'[CODPROD]</td>
      <td>Dim_Produto</td>
      <td>CODPROD</td>
      <td>1</td>
      <td>'Dim_Produto'[CODPROD]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_Notas'[CODPROD] * &lt;- 1 'Dim_Produto'[CODPROD]</td>
    </tr>
    <tr>
      <td>Fat_Notas</td>
      <td>CODTIPOPER</td>
      <td>*</td>
      <td>'Fat_Notas'[CODTIPOPER]</td>
      <td>Dim_Top</td>
      <td>CODTIPOPER</td>
      <td>1</td>
      <td>'Dim_Top'[CODTIPOPER]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_Notas'[CODTIPOPER] * &lt;- 1 'Dim_Top'[CODTIPOPER]</td>
    </tr>
    <tr>
      <td>Fat_Notas</td>
      <td>DTENTSAI</td>
      <td>*</td>
      <td>'Fat_Notas'[DTENTSAI]</td>
      <td>Dim_Calendario</td>
      <td>Data</td>
      <td>1</td>
      <td>'Dim_Calendario'[Data]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_Notas'[DTENTSAI] * &lt;- 1 'Dim_Calendario'[Data]</td>
    </tr>
    <tr>
      <td>Fat_Notas</td>
      <td>DTFATUR</td>
      <td>*</td>
      <td>'Fat_Notas'[DTFATUR]</td>
      <td>Dim_Calendario</td>
      <td>Data</td>
      <td>1</td>
      <td>'Dim_Calendario'[Data]</td>
      <td>False</td>
      <td>Único</td>
      <td>'Fat_Notas'[DTFATUR] * &lt;- 1 'Dim_Calendario'[Data]</td>
    </tr>
    <tr>
      <td>Fat_Notas</td>
      <td>MOTIVOCANC</td>
      <td>*</td>
      <td>'Fat_Notas'[MOTIVOCANC]</td>
      <td>Dim_MotivoCanc</td>
      <td>MOTIVOCANC</td>
      <td>1</td>
      <td>'Dim_MotivoCanc'[MOTIVOCANC]</td>
      <td>True</td>
      <td>Único</td>
      <td>'Fat_Notas'[MOTIVOCANC] * &lt;- 1 'Dim_MotivoCanc'[MOTIVOCANC]</td>
    </tr>
  </tbody>
</table>
</div>
        
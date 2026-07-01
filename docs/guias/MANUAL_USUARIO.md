### 📄 `docs/guias/MANUAL_USUARIO.md`
```markdown
# Manual do Usuário (Operação do Sistema)

Guia de orientação operacional direcionado aos servidores da Secretaria Municipal de Finanças (SEFIN) e auditores fiscais para as rotinas diárias no SIGT-Mun[cite: 4].

## 1. Módulo: Atendimento ao Contribuinte e Cadastro

### 1.1 Inclusão de Novo Contribuinte (Pessoa Física ou Jurídica)
1. No menu lateral esquerdo do painel interno, clique em **Cadastros** -> **Contribuintes**[cite: 4].
2. Clique no botão superior direito **Novo Contribuinte**[cite: 4].
3. Selecione o tipo de cadastro (`CPF` ou `CNPJ`)[cite: 4].
4. Insira o número do documento[cite: 4]. O sistema realizará automaticamente uma consulta na base sincronizada da Receita Federal para validar a regularidade do documento[cite: 4].
5. Preencha os dados de endereço e e-mail[cite: 4]. Ative a caixa **Domicílio Tributário Eletrônico (DTE)** para permitir notificações legais automatizadas via e-mail[cite: 4].
6. Clique em **Salvar Registro**[cite: 4].

---

## 2. Módulo: Lançamento de IPTU e Emissão de DAM

### 2.1 Emissão de Segunda Via de Guia para Pagamento (DAM)
O sistema unifica os lançamentos para facilitar o recebimento de cotas vencidas[cite: 4].
1. No menu principal, acesse **Arrecadação** -> **Consultar Débitos**[cite: 4].
2. Digite o CPF/CNPJ do contribuinte ou o número da **Inscricao Imobiliaria** do imóvel alvo[cite: 4].
3. O painel exibirá a lista de exercícios (anos fiscais) com seus respectivos status (`Aberto`, `Vencido`, `Pago`)[cite: 4].
4. Selecione as parcelas que deseja emitir e clique em **Gerar DAM Unificado**[cite: 4].
5. O sistema gerará instantaneamente um arquivo PDF contendo a linha digitável convencional com código de barras e o **QRCode Pix dinâmico com vencimento para o dia atual**[cite: 4].

---

## 3. Módulo: Fiscalização e Emissão de Notas Fiscais (NFS-e)

### 3.1 Consulta e Auditoria de Notas Fiscais Emitidas
1. Acesse o menu **Fiscalizacao** -> **Notas Fiscais (NFS-e)**[cite: 4].
2. Utilize os filtros de busca para refinar os resultados por intervalo de datas, CNPJ do Prestador ou CPF do Tomador dos Serviços[cite: 4].
3. Para auditar os dados brutos trafegados em formato XML padrão ABRASF, clique no ícone **Ver XML** localizado na coluna de ações do registro desejado[cite: 4].
4. Caso identifique irregularidades fiscais ou a pedido fundamentado da empresa emissora, clique em **Cancelar Nota**, preencha o campo obrigatório **Justificativa Legal de Cancelamento** e confirme com sua senha institucional[cite: 4].
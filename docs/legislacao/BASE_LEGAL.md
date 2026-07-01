# Base Legal do Sistema Tributário Municipal

Este documento mapeia o fundamento jurídico e as regras normativas aplicadas diretamente nas rotinas automatizadas, algoritmos de cálculo de juros, multas e regras de privacidade do SIGT-Mun[cite: 3].

## 1. Código Tributário Nacional (Lei nº 5.172/1966)
O sistema observa estritamente os preceitos gerais do CTN relativos à constituição do crédito tributário, lançamento, garantias e privilégios do crédito municipal[cite: 3].

### Aplicação Prática no Software
* **Artigo 198 (Sigilo Fiscal)**: Os dados de faturamento de empresas (ISSQN) e valores venais declarados de transações imobiliárias são criptografados em repouso[cite: 3]. O acesso aos dados por servidores públicos é auditado via logs e protegido por restrições baseadas em perfis funcionais (RBAC)[cite: 3].
* **Artigo 174 (Prescrição)**: O sistema roda uma rotina em lote (`cron job`) mensal que identifica débitos em aberto sem movimentação judicial há mais de 5 anos, notificando a Procuradoria Jurídica do Município para análise de prescrição tributária[cite: 3].

---

## 2. Lei Geral de Proteção de Dados - LGPD (Lei nº 13.709/2018)
O tratamento de informações cadastrais de pessoas físicas atende aos requisitos de finalidade, adequação e necessidade da LGPD[cite: 3].

### Aplicação Prática no Software
* **Eliminação de Dados**: Conforme o artigo 16 da LGPD, a exclusão de dados pessoais a pedido do titular não se aplica a registros necessários para o cumprimento de obrigações legais e fiscalização tributária municipal[cite: 3].
* **Trilha de Auditoria para Acesso a Dados Pessoais (PII)**: Qualquer consulta na tela de atendimento que exponha dados pessoais protegidos como CPF, endereço ou telefone dispara um log de nível `SELECT_PII` associado à matrícula do servidor municipal que efetuou o acesso[cite: 3].

---

## 3. Lei Complementar nº 116/2003 (Regras Gerais do ISSQN)
O módulo de Nota Fiscal de Serviços Eletrônica (NFS-e) e o cadastro de atividades econômicas estão alinhados com a lista nacional de serviços instituída por esta lei complementar federal[cite: 3].

### Limites de Alíquotas Implementados no Sistema
* **Alíquota Mínima**: 2,00% (Dois por cento) - O sistema bloqueia a gravação de tabelas de parametrização municipal ou emissão de notas com alíquota inferior a este patamar, prevenindo renúncia de receita ilegal[cite: 3].
* **Alíquota Máxima**: 5,00% (Cinco por cento) - Qualquer digitação ou cálculo que ultrapasse este limite é rejeitado pela camada de validação da API[cite: 3].
### 📄 `docs/modelagem/DICIONARIO_DADOS.md`
```markdown
# Dicionário de Dados Completo

Este documento descreve detalhadamente a estrutura física das tabelas do banco de dados relacional PostgreSQL (com extensão PostGIS) para o SIGT-Mun[cite: 7].

## 1. Tabela: `contribuinte`
Armazena as informações cadastrais unificadas de pessoas físicas e jurídicas[cite: 7].

| Campo | Tipo | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id` | UUID | PK | Identificador único universal do contribuinte[cite: 7]. |
| `cpf_cnpj` | VARCHAR(14) | UK, NOT NULL | CPF (11 caracteres) ou CNPJ (14 caracteres) sem máscara[cite: 7]. |
| `nome_razao` | VARCHAR(255) | NOT NULL | Nome completo (PF) ou Razão Social (PJ)[cite: 7]. |
| `tipo` | VARCHAR(2) | CHECK (tipo IN ('PF', 'PJ')) | Indicador de tipo de pessoa[cite: 7]. |
| `email` | VARCHAR(150) | - | E-mail principal para notificações e domicílio tributário eletrônico[cite: 7]. |
| `telefone` | VARCHAR(20) | - | Telefone de contato com DDD[cite: 7]. |
| `data_nascimento` | DATE | - | Data de nascimento (PF) ou data de abertura da empresa (PJ)[cite: 7]. |
| `regime_tributario` | VARCHAR(50) | - | Regime (ex: Simples Nacional, Lucro Presumido, Lucro Real)[cite: 7]. |
| `optante_simples` | BOOLEAN | DEFAULT FALSE | Flag de controle para integração com a Receita Federal[cite: 7]. |
| `ativo` | BOOLEAN | DEFAULT TRUE | Define se o cadastro está ativo para operações ordinárias[cite: 7]. |
| `criado_em` | TIMESTAMP | DEFAULT NOW() | Carimbo de data/hora de criação do registro[cite: 7]. |
| `atualizado_em` | TIMESTAMP | DEFAULT NOW() | Carimbo de data/hora da última alteração[cite: 7]. |

## 2. Tabela: `imovel`
Cadastro imobiliário urbano para cálculo e lançamento do IPTU[cite: 7].

| Campo | Tipo | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id` | UUID | PK | Identificador único do imóvel[cite: 7]. |
| `municipio_id` | UUID | FK -> `municipio(id)` | Município de circunscrição do imóvel[cite: 7]. |
| `contribuinte_id` | UUID | FK -> `contribuinte(id)` | Proprietário ou possuidor principal do imóvel[cite: 7]. |
| `inscricao_imobiliaria` | VARCHAR(30) | UK, NOT NULL | Código único de inscrição imobiliária municipal[cite: 7]. |
| `logradouro` | VARCHAR(255) | NOT NULL | Rua, Avenida, Praça, etc[cite: 7]. |
| `numero` | VARCHAR(10) | NOT NULL | Número do lote/edificação[cite: 7]. |
| `complemento` | VARCHAR(100) | - | Apartamento, Bloco, Sala, etc[cite: 7]. |
| `bairro` | VARCHAR(100) | NOT NULL | Bairro de localização do imóvel[cite: 7]. |
| `cep` | VARCHAR(8) | NOT NULL | CEP sem hífen[cite: 7]. |
| `lote` | VARCHAR(10) | - | Identificação do lote na quadra fiscal[cite: 7]. |
| `quadra` | VARCHAR(10) | - | Identificação da quadra fiscal[cite: 7]. |
| `zona_fiscal` | VARCHAR(20) | NOT NULL | Zona fiscal para fins de enquadramento de alíquota progressiva[cite: 7]. |
| `area_terreno` | NUMERIC(12,2) | NOT NULL | Área total da gleba ou terreno em metros quadrados[cite: 7]. |
| `area_construida` | NUMERIC(12,2) | DEFAULT 0.00 | Área construída da edificação em metros quadrados[cite: 7]. |
| `valor_venal_terreno` | NUMERIC(15,2) | NOT NULL | Valor venal calculado do terreno baseado na Planta Genérica[cite: 7]. |
| `valor_venal_construcao` | NUMERIC(15,2) | DEFAULT 0.00 | Valor venal da área construída baseado no padrão construtivo[cite: 7]. |
| `valor_venal_total` | NUMERIC(15,2) | NOT NULL | Soma do valor venal do terreno e da construção[cite: 7]. |
| `coordenadas` | GEOMETRY(Point, 4326) | - | Localização espacial por coordenadas geográficas via PostGIS[cite: 7]. |

## 3. Tabela: `empresa`
Cadastro mobiliário de empresas prestadoras de serviço para apuração do ISSQN[cite: 7].

| Campo | Tipo | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id` | UUID | PK | Identificador único da empresa[cite: 7]. |
| `contribuinte_id` | UUID | FK -> `contribuinte(id)` | Contribuinte PJ vinculado à empresa[cite: 7]. |
| `inscricao_municipal` | VARCHAR(20) | UK, NOT NULL | Número de Inscrição Municipal Mobiliária[cite: 7]. |
| `cnae_principal` | VARCHAR(7) | NOT NULL | Código CNAE principal da atividade econômica[cite: 7]. |
| `alvara_numero` | VARCHAR(20) | - | Número do alvará de funcionamento e localização vigente[cite: 7]. |
| `alvara_validade` | DATE | - | Data de expiração do alvara de funcionamento[cite: 7]. |
| `regime_issqn` | VARCHAR(30) | NOT NULL | Regime de apuração do imposto (Fixo, Variável, Estimado)[cite: 7]. |
| `aliquota_issqn` | NUMERIC(4,2) | CHECK (>= 2.00 AND <= 5.00) | Alíquota municipal padrão aplicável à empresa[cite: 7]. |

## 4. Tabela: `debito`
Lançamentos financeiros de obrigações tributárias principais e acessórias[cite: 7].

| Campo | Tipo | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id` | UUID | PK | Identificador único da obrigação financeira[cite: 7]. |
| `contribuinte_id` | UUID | FK -> `contribuinte(id)` | Devedor principal do débito tributário[cite: 7]. |
| `imovel_id` | UUID | FK, NULL | Vínculo caso o débito seja IPTU ou ITBI[cite: 7]. |
| `empresa_id` | UUID | FK, NULL | Vínculo caso o débito seja ISSQN ou Taxas Mobiliárias[cite: 7]. |
| `tipo` | VARCHAR(20) | NOT NULL | Tipo do tributo: 'IPTU', 'ISSQN', 'ITBI', 'TAXA'[cite: 7]. |
| `exercicio` | INTEGER | NOT NULL | Ano de competência do lançamento tributário (ex: 2026)[cite: 7]. |
| `parcela` | INTEGER | NOT NULL | Número da parcela (0 para cota única, >= 1 para parcelados)[cite: 7]. |
| `valor_principal` | NUMERIC(15,2) | NOT NULL | Valor histórico/nominal lançado do imposto[cite: 7]. |
| `valor_multa` | NUMERIC(15,2) | DEFAULT 0.00 | Multa moratória calculada com base no atraso[cite: 7]. |
| `valor_juros` | NUMERIC(15,2) | DEFAULT 0.00 | Juros de mora acumulados (conforme taxa Selic ou Código Municipal)[cite: 7]. |
| `valor_total` | NUMERIC(15,2) | NOT NULL | Soma de principal + multa + juros[cite: 7]. |
| `data_vencimento` | DATE | NOT NULL | Data limite para pagamento sem acréscimos legais[cite: 7]. |
| `status` | VARCHAR(20) | DEFAULT 'Aberto' | Estado atual: 'Aberto', 'Vencido', 'Pago', 'Cancelado'[cite: 7]. |
| `dam_numero` | VARCHAR(30) | UK, NOT NULL | Documento de Arrecadação Municipal único gerado para pagamento[cite: 7]. |
| `codigo_pix` | TEXT | - | Payload String do Pix Copia e Cola estático/dinâmico[cite: 7]. |
| `txid_pix` | VARCHAR(50) | UK, NULL | Identificador único da transação Pix junto ao Banco Central/PSP[cite: 7]. |

## 5. Tabela: `nota_fiscal_servico`
Registro de Notas Fiscais de Serviços Eletrônicas (NFS-e) emitidas na plataforma[cite: 7].

| Campo | Tipo | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id` | UUID | PK | Identificador único da nota fiscal emitida[cite: 7]. |
| `empresa_id` | UUID | FK -> `empresa(id)` | Empresa prestadora/emissora do serviço[cite: 7]. |
| `numero` | BIGINT | NOT NULL | Número sequencial crescente da NFS-e por município[cite: 7]. |
| `codigo_verificacao`| VARCHAR(10) | NOT NULL | Código alfanumérico para validação de autenticidade pública[cite: 7]. |
| `data_emissao` | TIMESTAMP | NOT NULL | Data e hora exata da consolidação da nota fiscal[cite: 7]. |
| `prestador_cnpj` | VARCHAR(14) | NOT NULL | CNPJ do prestador para fins de checagem direta[cite: 7]. |
| `tomador_cpf_cnpj` | VARCHAR(14) | NOT NULL | CPF ou CNPJ da pessoa que contratou o serviço[cite: 7]. |
| `tomador_nome` | VARCHAR(255) | NOT NULL | Nome ou razão social do tomador do serviço[cite: 7]. |
| `discriminacao` | TEXT | NOT NULL | Descrição detalhada dos serviços executados[cite: 7]. |
| `codigo_servico` | VARCHAR(10) | NOT NULL | Subitem da lista de serviços da Lei Complementar nº 116/2003[cite: 7]. |
| `valor_servicos` | NUMERIC(15,2) | NOT NULL | Valor total bruto cobrado pelos serviços prestados[cite: 7]. |
| `base_calculo` | NUMERIC(15,2) | NOT NULL | Valor base para aplicação da alíquota (serviços - deduções)[cite: 7]. |
| `aliquota_issqn` | NUMERIC(4,2) | NOT NULL | Alíquota aplicada ao serviço específico (2% a 5%)[cite: 7]. |
| `valor_issqn` | NUMERIC(15,2) | NOT NULL | Montante de imposto devido calculado (base_calculo * aliquota)[cite: 7]. |
| `status` | VARCHAR(20) | DEFAULT 'Emitida' | Estado da nota fiscal: 'Emitida', 'Cancelada'[cite: 7]. |
| `xml_abrasf` | TEXT | NOT NULL | Conteúdo estruturado XML no padrão oficial ABRASF v2.03[cite: 7]. |

## 6. Tabela: `usuario_sistema`
Controle de acessos de servidores e administradores municipais[cite: 7].

| Campo | Tipo | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id` | UUID | PK | Identificador único do servidor[cite: 7]. |
| `cpf` | VARCHAR(11) | UK, NOT NULL | CPF do funcionário sem máscara[cite: 7]. |
| `email` | VARCHAR(150) | UK, NOT NULL | E-mail corporativo/institucional[cite: 7]. |
| `senha_hash` | VARCHAR(255) | NOT NULL | Hash criptográfico seguro (Argon2id ou BCrypt)[cite: 7]. |
| `perfil` | VARCHAR(30) | NOT NULL | Perfil RBAC: 'Admin', 'Fiscal', 'Atendente', 'Auditor'[cite: 7]. |
| `ativo` | BOOLEAN | DEFAULT TRUE | Bloqueio ou liberação imediata de acesso ao sistema[cite: 7]. |

## 7. Tabela: `auditoria_log`
Trilha imutável de auditoria em conformidade com as normas de controle público e LGPD[cite: 7].

| Campo | Tipo | Restrições | Descrição |
| :--- | :--- | :--- | :--- |
| `id` | BIGSERIAL | PK | Sequencial numérico imutável auto-incrementável[cite: 7]. |
| `usuario_id` | UUID | FK -> `usuario_sistema(id)`| Usuário interno que executou a ação fiscal/tributária[cite: 7]. |
| `data_hora` | TIMESTAMP | DEFAULT NOW() | Data e hora milisegundada do evento[cite: 7]. |
| `ip_address` | VARCHAR(45) | NOT NULL | Endereço IP de origem da requisição (suporta IPv4 e IPv6)[cite: 7]. |
| `operacao` | VARCHAR(10) | NOT NULL | Ação executada: 'INSERT', 'UPDATE', 'DELETE', 'SELECT_PII'[cite: 7]. |
| `tabela_afetada` | VARCHAR(50) | NOT NULL | Nome técnico da tabela que sofreu a intervenção[cite: 7]. |
| `registro_id` | UUID | NOT NULL | Identificador do registro modificado ou consultado[cite: 7]. |
| `valor_anterior` | JSONB | - | Estado completo do registro em formato JSON antes da operação[cite: 7]. |
| `valor_novo` | JSONB | - | Estado completo do registro em formato JSON após a operação[cite: 7]. |
| `hash_integridade` | VARCHAR(64) | NOT NULL | Hash SHA-256 encadeado do registro para evitar adulterações físicas[cite: 7]. |
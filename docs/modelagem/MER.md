# Modelo Entidade-Relacionamento (MER)

Este documento apresenta a especificação detalhada do Modelo Entidade-Relacionamento do Sistema Web de Gestão Tributária Municipal (SIGT-Mun), mapeando os fluxos e dependências estruturais entre as entidades centrais do sistema.

## 1. Diagrama Conceitual Geral

```mermaid
erDiagram
    MUNICIPIO ||--o{ IMOVEL : "contém"
    CONTRIBUINTE ||--o{ IMOVEL : "possui"
    CONTRIBUINTE ||--o{ EMPRESA : "representa"
    CONTRIBUINTE ||--o{ DEBITO : "vincula-se a"
    CONTRIBUINTE ||--o{ PARCELAMENTO : "solicita"
    CONTRIBUINTE ||--o{ CDA : "inscrito em"
    CONTRIBUINTE ||--o{ CERTIDAO_NEGATIVA : "emite"
    
    IMOVEL ||--o{ DEBITO : "origina"
    IMOVEL ||--o{ TRANSACAO_ITBI : "objeto de"
    
    EMPRESA ||--o{ DEBITO : "origina"
    EMPRESA ||--o{ NOTA_FISCAL_SERVICO : "emite"
    
    DEBITO ||--o| PARCELAMENTO : "pode compor"
    PARCELAMENTO ||--|{ PARCELAMENTO_PARCELA : "divide-se em"
    
    USUARIO_SISTEMA ||--o{ AUDITORIA_LOG : "gera"
    TRANSACAO_ITBI }o--|| CONTRIBUINTE : "transmitente"
    TRANSACAO_ITBI }o--|| CONTRIBUINTE : "adquirente"
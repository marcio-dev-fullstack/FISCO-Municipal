### 📄 `docs/guias/DESENVOLVEDOR.md`
```markdown
# Guia do Desenvolvedor

Manual de onboarding técnico para engenheiros de software do projeto SIGT-Mun. Aqui estão descritas as convenções, padrões de arquitetura e o fluxo de trabalho de desenvolvimento.

## 1. Padrões de Projeto e Arquitetura de Software

O ecossistema backend está estruturado sob os princípios da **Arquitetura Limpa (Clean Architecture)** e **Domain-Driven Design (DDD)**. O código-fonte na pasta `src/backend/` adota a seguinte segmentação interna rígida:

```plain
src/backend/app/
├── core/               # Configurações globais do sistema (segurança, envs, logs)
├── domain/             # Entidades puras de negócio, regras de validação e interfaces
├── infra/              # Implementações de banco de dados (ORM), clientes HTTP e Gateways
├── use_cases/          # Orquestração dos fluxos de trabalho funcionais do sistema
└── v1/                 # Controladores HTTP (API Endpoints), schemas de entrada e saída
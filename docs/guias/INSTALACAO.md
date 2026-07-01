# Guia de Instalação e Implantação

Este manual contém as instruções técnicas passo a passo para configurar, buildar e rodar o Sistema de Gestão Tributária Municipal (SIGT-Mun) em ambientes de desenvolvimento e produção utilizando contêineres Docker.

## 1. Pré-requisitos do Sistema
Antes de prosseguir, garanta que os componentes abaixo estejam devidamente instalados no servidor de aplicação:
* **Git** >= 2.34
* **Docker Engine** >= 24.0.0
* **Docker Compose V2** >= 2.20.0

### Portas Mapeadas por Padrão
Certifique-se de que as seguintes portas estão livres no host:
* `8000`: Backend FastAPI (API Gateway / Core)
* `3000`: Frontend React (Portal do Contribuinte / Backoffice)
* `5432`: Banco de Dados PostgreSQL + PostGIS

---

## 2. Passo a Passo da Instalação

### Passo 1: Clonar o Repositório Oficial
Execute o clone das fontes do projeto utilizando acesso HTTPS ou SSH:
```bash
git clone [https://github.com/seu-org/sigt-mun.git](https://github.com/seu-org/sigt-mun.git)
cd sigt-mun
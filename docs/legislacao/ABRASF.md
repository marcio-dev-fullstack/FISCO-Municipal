# Manual de Integração e Padrão ABRASF (NFS-e)

O SIGT-Mun adota o modelo conceitual de Nota Fiscal de Serviços Eletrônica (NFS-e) instituído pela **ABRASF (Associação Brasileira das Secretarias de Finanças das Capitais)**, na versão estável **2.03**.

## 1. Estrutura de Comunicação via Web Services
A troca de mensagens entre os sistemas das empresas contribuintes (ERP) e a API do município ocorre por meio de Web Services estruturados em protocolo **SOAP 1.2** com segurança em nível de transporte e autenticação por certificado digital ICP-Brasil.

### Endpoints Disponibilizados pela API Municipal
* `EnviarLoteRps`: Recebimento de Lotes de Recibo Provisório de Serviços (RPS) para processamento assíncrono.
* `ConsultarLoteRps`: Consulta do resultado do processamento do lote e retorno das notas fiscais geradas ou erros de validação.
* `CancelarNfse`: Solicitação de cancelamento de nota fiscal emitida[cite: 2].

---

## 2. Esquema de Validação XML (XSD)
Todas as requisições enviadas ao sistema devem ser validadas previamente contra os esquemas XSD oficiais da ABRASF[cite: 2]. Segue a estrutura básica obrigatória do nó de dados brutos para o Envio de RPS[cite: 2]:

```xml
<EnviarLoteRpsEnvio xmlns="[http://www.abrasf.org.br/nfse.xsd](http://www.abrasf.org.br/nfse.xsd)">
    <LoteRps Id="lote_001" versao="2.03">
        <NumeroLote>1</NumeroLote>
        <Cnpj>00000000000000</Cnpj>
        <InscricaoMunicipal>12345</InscricaoMunicipal>
        <QuantidadeRps>1</QuantidadeRps>
        <ListaRps>
            <Rps>
                <InfDeclaracaoPrestadorServico Id="rps_001">
                    <Rps>
                        <IdentificacaoRps>
                            <Numero>101</Numero>
                            <Serie>A</Serie>
                            <Tipo>1</Tipo>
                        </IdentificacaoRps>
                        <DataEmissao>2026-07-01T10:00:00</DataEmissao>
                        <Status>1</Status>
                    </Rps>
                    <Competencia>2026-07-01</Competencia>
                    <Servico>
                        <Valores>
                            <ValorServicos>1000.00</ValorServicos>
                            <BaseCalculo>1000.00</BaseCalculo>
                            <Aliquota>0.05</Aliquota>
                            <ValorIss>50.00</ValorIss>
                        </Valores>
                        <ItemListaServico>01.01</ItemListaServico>
                        <CodigoMunicipio>1502707</CodigoMunicipio>
                    </Servico>
                </InfDeclaracaoPrestadorServico>
            </Rps>
        </ListaRps>
    </LoteRps>
</EnviarLoteRpsEnvio>
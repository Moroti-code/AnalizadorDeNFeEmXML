# Analisador de NF-e

Ferramenta web, 100% local, para extração e organização de dados de NF-e (XML), com foco em **cadastro de produtos e fornecedores** e **geração de pedido de compra (PDC)**.

**Autor:** Denis Moroti de Souza Campos (denis.campos)
**Versão atual:** v14.0.0
**Data de consolidação:** 22 de novembro de 2026
**Licença:** Uso por licença pessoal — ver seção *Licença de Uso*

---

## 📌 Sobre este README

Este documento é mantido **junto com o código** e serve como:

1. **Registro de autoria e titularidade** da ferramenta
2. **Histórico cronológico** de versões e modificações (ver `CHANGELOG.md`)
3. **Documentação dos termos legais** aplicáveis ao uso
4. **Prova documental** do desenvolvimento independente do autor

O registro cronológico oficial de cada versão é o **histórico de commits do GitHub**.

---

## 🎯 O que a ferramenta faz

- Lê um ou mais arquivos **XML de NF-e** (layout 4.00) diretamente no navegador
- Extrai dados para **cadastro de produto**: código, descrição, NCM, CEST, origem/CST, unidade, valor (opcional)
- Extrai **ficha completa do emitente** (distribuidor) para cadastro de novos fornecedores
- Apresenta **resumo financeiro agregado** em 2 linhas: Acréscimos e Descontos
- Exibe **formas de pagamento** e **duplicatas/boletos** quando presentes
- Detecta **situação da NF-e** (autorizada, cancelada, evento pendente)
- Oferece **3 modos independentes por NF-e**:
  - **Normal** — cadastro de produto (sem valores)
  - **PDC** — pedido de compra (Qtd, Vlr Unit, Vlr Total + CNPJ destacado)
  - **Tributário** — conferência fiscal (CFOP, ICMS, IPI, PIS, COFINS, FCI, totais)
- Permite **ocultar código de barras** por NF-e (padrão OFF)
- Compara **código de barras × código do produto** e evita duplicação quando iguais
- Marca produtos como **cadastrado** (esmaece a linha) — com controle por NF-e e global
- Copia **campos com 1 clique** (código, descrição, NCM, CEST, valores)
- Pesquisa imagem do produto via **Google Images / Google / Bing / Mercado Livre** (configurável)
- Processa tudo **localmente no navegador** — nenhum XML é enviado a servidor

---

## 🧭 Modos de operação (por NF-e)

Cada NF-e carregada tem **seus próprios toggles** (não afetam outras notas):

### Modo Normal (padrão)
Foco: cadastro de produto.

**Colunas exibidas:**
`#` · Código · Descrição · O/CST · NCM · CEST · Unidade · Marca* · FCI* · Barcode*

*\* condicional — só aparece se algum produto da nota tiver o campo (e, no caso do Barcode, se o toggle estiver ativo)*

### Modo PDC (Pedido de Compra)
Foco: lançar produtos no sistema de PDC.

**Colunas exibidas:**
`#` · Código · Descrição · Qtd · Vlr Unit · Vlr Total

**Destaque:** CNPJ do distribuidor em caixa verde no topo do card (copiável)

### Modo Tributário
Foco: conferência fiscal.

**Colunas exibidas:**
`#` · Código · Descrição · O/CST · NCM · CEST · Unidade · Qtd · Vlr Unit · Vlr Total · Marca* · FCI* · Barcode* · CFOP · ICMS · IPI · PIS · COFINS

**Rodapé adicional:** ICMS, IPI, ICMS-ST, FCP

### Toggles independentes por NF-e
- **🎯 Tributário** — ativa/desativa modo tributário
- **📦 PDC** — ativa/desativa modo pedido de compra (mutuamente exclusivo com Tributário)
- **📋 FCI** — mostra/esconde coluna FCI (padrão: ativo)
- **📷 Barcode** — mostra/esconde coluna EAN (padrão: inativo)
- **☐ Desmarcar NF-e** — remove marcações apenas desta nota

### Toggle global
- **☐ Desmarcar todos** — remove marcações de **todas** as notas

---

## ⚖️ Escopo e não-escopo

**É escopo desta ferramenta:**
- Apoiar cadastro de produto (SKU, NCM, CEST, CFOP, O/CST, unidade)
- Apoiar cadastro de fornecedor (ficha completa do emitente)
- Apoiar geração de pedido de compra (Qtd, valor, CNPJ do distribuidor)
- Apoiar conferência fiscal quando solicitado (modo Tributário)

**Não é escopo:**
- Validação fiscal completa da NF-e contra a SEFAZ
- Cálculos fiscais de qualquer natureza
- Substituição do DANFE (PDF) para fins fiscais
- Exportação CSV (temporariamente fora de escopo — usar "Copiar tabela")

---

## 🛠️ Histórico de versões

Ver [`CHANGELOG.md`](CHANGELOG.md) para o histórico completo.

### Versões recentes

- **v14.0.0** (22/11/2026) — Modos por NF-e, rodapé simplificado (Acréscimos/Descontos), FCI toggle, destaque de linha inteira
- **v13.0.0** (18/11/2026) — Correção de bugs (numeração, marcação), reorganização de colunas, modo PDC com CNPJ em destaque, FCI no Normal, sem quebra de linha
- **v12.0.0** (17/11/2026) — Cards compactos, tema responsivo, marcar como cadastrado, modo tributário, pesquisa de imagem
- **v11.0.0** (15/11/2026) — Ficha do emitente, consulta por chave, download XML/PDF, campos copiáveis
- **v10.0.0** (14/11/2026) — Disclaimers legais (banner bloqueante, modal de termos, rodapé)

---

## ⚖️ Termos legais

### 1. Titularidade e autoria

Esta ferramenta é de **propriedade intelectual de Denis Moroti de Souza Campos (denis.campos)**, nos termos da **Lei 9.609/98 (Lei do Software)** e da **Lei 9.610/98 (Lei de Direitos Autorais)**.

Foi desenvolvida de forma **independente**, em ambiente e equipamento **pessoais**, **fora do horário de trabalho** e **sem utilização de dados, sistemas ou recursos de qualquer empresa**.

A utilização desta ferramenta por qualquer pessoa ou empresa configura **licença de uso**, de caráter **não exclusivo, não transferível, revogável e gratuito**, limitada a **uso interno**, e **não implica cessão, total ou parcial, dos direitos autorais ou de propriedade intelectual**.

### 2. Limitação de responsabilidade

A ferramenta é fornecida **"no estado em que se encontra" ("as is")**, **sem garantia de qualquer espécie**, expressa ou implícita, incluindo, mas não se limitando a, garantias de comerciabilidade, adequação a um propósito específico, ausência de erros ou de vulnerabilidades.

Na máxima extensão permitida pela legislação aplicável, **o autor não será responsável** por quaisquer danos diretos, indiretos, incidentais, especiais, consequenciais ou punitivos — incluindo lucros cessantes, perda de dados, interrupção de atividade, erros de cadastro, pagamentos indevidos, autuações fiscais ou prejuízos de qualquer natureza — decorrentes do uso, do mau uso ou da impossibilidade de uso da ferramenta, **ainda que o autor tenha sido avisado da possibilidade de tais danos**.

**A responsabilidade final pela conferência dos dados e pelas decisões tomadas com base neles é integralmente do usuário e da empresa utilizadora.**

### 3. Uso de inteligência artificial

Esta ferramenta foi **desenvolvida com auxílio de inteligência artificial** e **supervisionada, revisada e validada por responsável humano identificado** (Denis Moroti de Souza Campos — denis.campos).

O código foi revisado e testado antes da disponibilização, mas **pode conter erros, imprecisões ou vulnerabilidades**.

A inteligência artificial **não detém responsabilidade legal** sobre a ferramenta. A responsabilidade pela revisão, aprovação e uso é **humana**, e recai sobre o usuário e a empresa utilizadora.

### 4. Dados e privacidade

Esta ferramenta processa os arquivos **exclusivamente no navegador do usuário**. Nenhum dado é enviado a servidores, serviços externos ou terceiros. Os arquivos XML **não são armazenados, transmitidos ou compartilhados** pela ferramenta.

Ainda assim, **a responsabilidade pela guarda, pelo tratamento e pela proteção dos dados pessoais e fiscais contidos nos XMLs é integralmente do usuário e da empresa utilizadora**, nos termos da **LGPD (Lei 13.709/18)** e da legislação aplicável.

### 5. Licença de uso e restrições

**É vedado**, sem autorização prévia e por escrito do autor:

- Copiar, modificar, distribuir, sublicenciar, vender, alugar ou disponibilizar a ferramenta a terceiros
- Utilizar a ferramenta em **outras empresas do grupo empresarial**, em filiais, coligadas ou controladas, sem autorização formal
- Utilizar a ferramenta para finalidades ilícitas, fraudulentas ou que violem direitos de terceiros
- Remover, ocultar ou alterar avisos de autoria, disclaimers ou marcas

**O uso em outras empresas do grupo depende de autorização prévia e por escrito do autor (Denis Moroti de Souza Campos).**

---

## 📬 Canal de reporte de falhas

Encontrou erro, imprecisão, vulnerabilidade ou comportamento inesperado?

**Comunique imediatamente ao autor antes de aprovar, distribuir ou ampliar o uso da ferramenta.**

- Autor: Denis Moroti de Souza Campos (denis.campos)
- Preferencialmente por **issue no repositório** ou **contato direto** com o autor

### Política de segurança

Vulnerabilidades de segurança são tratadas com **prioridade máxima**.
Se você identificar uma falha que possa comprometer dados, integridade
ou disponibilidade, **não abra issue pública** — entre em contato
diretamente com o autor para tratamento reservado antes da divulgação.

---

## 🔒 Nota sobre o desenvolvimento

- Desenvolvido em **2026**, em ambiente pessoal, **fora do horário de trabalho**
- **Sem utilização de dados, sistemas, equipamentos ou recursos** de qualquer empresa
- Dados de teste originais: XML de compra pessoal do autor no Mercado Livre
- **Sem vínculo com projeto interno** de qualquer empregador
- Código mantido neste repositório como **registro público de autoria e histórico**

---

## 🧪 Como usar

1. Baixe o arquivo `analisador-nfe.html` da release mais recente
2. Abra no navegador (duplo clique)
3. Aceite o aviso legal no primeiro acesso
4. Arraste um ou mais XMLs de NF-e
5. Use os toggles por NF-e para alternar entre os modos Normal / PDC / Tributário
6. Use **Copiar tabela** para exportar (TSV, colável em planilha)

**Requisitos:** navegador moderno (Chrome, Edge, Firefox, Safari). Não requer instalação, servidor, internet ou conta.

---

## 🚫 Limitações conhecidas

- Não valida assinatura digital da NF-e
- Não verifica a nota em tempo real na SEFAZ
- Não substitui o DANFE (PDF) para fins fiscais
- Não detecta todos os eventos possíveis (só cancelamento via `110111`)
- Não lê PDFs — apenas XMLs
- Exportação CSV temporariamente desabilitada (apenas "Copiar tabela" disponível)
- A "marca" só é detectada se vier em tag custom (`<marca>`, `<xMarca>`) ou `obsCont` com `xCampo="marca"`

---

## 📄 Licença

**Todos os direitos reservados ao autor.** Uso por licença, não cessão. Consulte a seção *5. Licença de Uso e Restrições* acima para os termos completos.

© 2026 Denis Moroti de Souza Campos (denis.campos)

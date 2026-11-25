# Changelog

Todas as alterações notáveis deste projeto estão documentadas neste arquivo.

O formato segue [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/).
O versionamento segue [Semantic Versioning](https://semver.org/lang/pt-BR/).

---
v17.0.0 — Correções do modo Asana e botões globais

Corrigido:
- Botões de cópia do modo Asana (NF, Distribuidor, Ambos) não funcionavam
  porque tinham classe .asana-copy-btn sem .copy-clickable nem .row-action.
  Adicionado seletor no handler global.

Adicionado:
- Separador decimal próprio do modo Asana (padrão: ponto ".")
  - Toggle inline no card Asana (botão "🔢 Ponto/Vírgula")
  - Também disponível nas Configurações
  - Aplicado ao valor da NF, lista de boletos e pill do título
  - Separador global continua aplicando a Normal/PDC/Tributário
- Botão "📅 Boletos (todas)" na toolbar — aplica modo Boletos em todas as NF-e
- Botão "📋 Asana (todas)" na toolbar — aplica modo Asana em todas as NF-e
  (ambos têm comportamento toggle: se todas já estão no modo, volta para Normal)

---
  
v16.0.0 — Modo Asana, modo Boletos, vencimento no título e correções

Adicionado:
- Modo "Asana" (📋): card minimal com NF+distribuidor (3 botões de cópia),
  valor da NF, data de emissão, lista de boletos e cópia em lote dos nomes
- Modo "Boletos" (📅): lista focada em vencimentos com dias até o vencimento
  calculados a partir da data do computador; cores por urgência
- Pill de vencimento no título (todos os modos): menor data de vencimento
  com cor por urgência (verde/amarelo/vermelho)

Corrigido:
- Fechar NF-e → re-upload marcava como duplicada (agora splice de nfes)
- `\n` literal ou real em nomes de produtos ao copiar (safeCopyLine em todos
  os data-copy; parser sanitiza também cProd, ncm, cest, cfop)
- Toast "1 produtos" → "1 produto"
- Minimized set persiste após close (limpo no close)
- `data-copy` com caracteres de controle

Interno:
- `safeCopyLine` remove `\\n`, `\\r`, `\\t` literais e `\n`, `\r`, `\t` reais
- Análise de bugs registrada em comentário (v17 se necessário):
  * Estado de <details> reseta a cada render (requer state per-NF-e)
  * `chave` vazia causa colisão (improvável em NF-e real)

---

v15.0.0 — Cópia em lote e correções

Adicionado:
- Botão por NF-e: "Copiar todos os códigos" (1 por linha)
- Botão por NF-e: "Copiar código + nome" (1 por linha)
- Botão por produto: "Copiar nome + (código)"
- Botão global na toolbar: alternar separador decimal (vírgula ↔ ponto)

Corrigido:
- Upload múltiplo: só a primeira NF-e era carregada (FileList ficava stale após limpar input)
- Highlight de linha: elemento copiado perdia destaque após re-render, mas a linha continuava destacada
- Cópia em lote força remoção de quebras de linha nos nomes

Interno:
- Tracking de destaque agora persiste via data-field após re-renderizações

---

## [14.0.0] — 22/11/2026

### Adicionado
- **Botão FCI por NF-e** — ativa/desativa a coluna FCI apenas na NF-e correspondente, com detecção automática se o campo existir
- **FCI ativado por padrão** em todas as NF-e que contenham o campo
- **Botão "Desmarcar NF-e"** dentro de cada card — remove apenas as marcações daquela nota (complementa o "Desmarcar todos" global)
- **Destaque da linha inteira** ao copiar — a linha inteira fica com fundo azul suave até nova cópia, facilitando saber em qual linha se está trabalhando em notas com 10+ produtos

### Modificado
- **Modos PDC / Tributário / Barcode agora são por NF-e** (antes eram globais). Cada nota pode ter seu próprio modo ativo, sem afetar as outras
- **Rodapé financeiro simplificado** para apenas duas linhas agregadas:
  - **Acréscimos** — soma de tudo que aumenta o valor (frete + seguro + outras despesas + IPI + ST + FCP)
  - **Descontos** — soma de tudo que diminui o valor
  - Elimina as linhas separadas de "Frete", "Seguro" e "Outras Despesas"
- **Cor do desconto alterada de vermelho para verde** — desconto reduz o valor da nota, é algo positivo; o vermelho transmitia a ideia errada

### Removido
- **CNPJ do título da NF-e** — redundante, já aparece em destaque no bloco verde do modo PDC
- **Botão "Exportar CSV"** — temporariamente fora de escopo; permanece apenas "Copiar tabela" (TSV para colar em planilha)

### Mantido
- "Desmarcar todos" global na toolbar
- "Copiar tabela" (TSV)

---

## [13.0.0] — 18/11/2026

### Corrigido
- **Bug crítico: numeração das linhas aparecendo vazia** — o parser usava o campo `nItem` do XML, que em alguns fornecedores vem vazio. Agora a numeração é gerada internamente pela ordem do produto no XML (sempre `1.`, `2.`, `3.`...)
- **Bug crítico: marcação "cadastrado" afetava todos os produtos de uma vez** — a chave interna de marcação usava `nItem`; se este vinha vazio, todos os produtos compartilhavam a mesma chave. Agora a chave usa o índice do produto, tornando cada linha independente
- **Botão "Desmarcar todos"** agora reflete corretamente o estado real das marcações

### Adicionado
- **Coluna Qtd** de volta no modo PDC e Tributário (necessária para gerar o pedido de compra)
- **Coluna FCI** no modo Normal (quando o XML trouxer o campo)
- **Toggle PDC** (modo pedido de compra — exibe Qtd, Vlr Unit, Vlr Total + CNPJ do distribuidor em destaque)
- **Toggle Barcode** (ativa/desativa coluna EAN — padrão OFF)
- **Checkbox como primeira coluna** à esquerda da linha (facilita uso em monitor duplo)
- **CNPJ do distribuidor em destaque** no topo do card, no modo PDC (copiável)
- **Frete e Desconto explícitos** no rodapé

### Modificado
- **O/CST movido para antes do NCM** na tabela
- **Sem quebra de linha em todas as colunas** exceto descrição (CEST estava quebrando)
- **Colunas vazias escondidas automaticamente** — se nenhum produto da nota tem o campo, a coluna inteira desaparece
- **Cadastro sem valores por item** — Vlr Unit e Vlr Total agora aparecem apenas nos modos PDC e Tributário (não são úteis no cadastro)

### Removido
- **Botão Imprimir** (individual e geral)
- **Seção "Consultar NF-e por chave de acesso"**
- **Botões ⬇ XML e 🖨 PDF** por NF-e

---

## [12.0.0] — 17/11/2026

### Adicionado
- **Modo retrato compacto** — cards de 3 linhas em telas estreitas
- **Ciclo de 3 cores** nos cards (claro → médio → escuro → reinicia)
- **Detecção automática de modo** (auto / monitor / celular)
- **Zoom 75% compatível**
- **Botão "Marcar como cadastrado"** (esmaece a linha)
- **Último copiado destacado** (fundo amarelo persistente)
- **Botão Barcode** (toggle global)
- **Modo tributário** (CFOP, ICMS, IPI, PIS, COFINS, FCI)
- **Ação 🔍 por produto** com site de pesquisa configurável
- **CNPJ copiável** no título da NF-e
- **Minimizar NF-e inteira** (▼) e **fechar individual** (✕)

### Modificado
- **Valores sem R$** na cópia (apenas `291,93`)
- **Separador decimal configurável** — vírgula por padrão, ponto opcional
- **NCM e CEST formatados na tela** (`8467.22.00`), crus na cópia (`84672200`)
- **Descrições longas não truncam** — whitespace normalizado
- **Unidade não quebra linha** quando ≤3 caracteres
- **Numeração visível** `#1, #2...`

### Removido
- Ação "copiar código + nome" (redundante com 🔍)

---

## [11.0.0] — 15/11/2026

### Adicionado
- **Ficha do emitente minimizável** (recolhida por padrão)
- **Consulta por chave de acesso** com decodificação (remoção futura na v13)
- **Download individual de XML e PDF**
- **Campos copiáveis com 1 clique** (sem ícone)
- **Ação "copiar código + nome"** para pesquisa (remoção futura na v12)
- **Pesquisa configurável** (Google Images, Google, Bing, Mercado Livre, custom)
- **Detecção de campo "marca"** (tag custom ou `obsCont`)
- **Modo de visualização manual** (monitor / celular / automático)
- **Remoção de zeros à esquerda** em códigos numéricos
- **O/CST com cor apenas na origem**

---

## [10.0.0] — 14/11/2026

### Adicionado
- Banner bloqueante de aviso no primeiro acesso
- Modal de termos com 5 seções (Autor, Responsabilidade, IA, Dados, Licença)
- Rodapé permanente com texto legal
- Marcação de versão na interface
- **Sem alterações funcionais** em relação à v9

---

## [9.0.0] — 14/11/2026

### Corrigido
- Classificação de evento de cancelamento em **confirmado** (retorno cStat 135/155) ou **pendente** (sem retorno)
- Evento de cancelamento pendente exibido com banner amarelo, sem marcar como cancelada
- Sanitização de TSV contra injeção de fórmulas e tab/newline
- Pill de valor neutro quando não há protocolo

---

## [8.0.0] — 14/11/2026

### Corrigido
- Detecção de evento de cancelamento via `<procEventoNFe>`
- Sanitização de fórmula também no TSV
- `csvSafe` checa primeiro caractere significativo
- Coluna `Acréscimos NF` renomeada para `Ajuste NF`

---

## [7.0.0] — 14/11/2026

### Corrigido
- Preservação da precisão decimal do valor unitário (até 10 casas)
- Suporte a múltiplos pagamentos
- Aviso visual de `cStat` da NF-e
- Exportação de Origem, CST e CSOSN em colunas separadas
- Detecção de upload duplicado pela chave de acesso
- Acréscimos negativos exibidos como "Outras Deduções"

---

## [6.0.0] — 14/11/2026

### Adicionado
- Rodapé com Valor dos Produtos, Descontos, Acréscimos e Total da Nota
- Fórmula garantida: `Total = vProd − vDesc + ajuste`
- Colunas extras no CSV

---

## [5.0.0] — 14/11/2026

### Adicionado
- Detecção de `cEAN` igual ao `cProd`
- Célula mostra `= código` em vez de repetir
- Coluna oculta quando todos os produtos têm barcode = código
- Célula vazia no CSV quando barcode = código

---

## [4.0.0] — 14/11/2026

### Adicionado
- Card completo do emitente (CNPJ, IE, regime, endereço, CEP, telefone)
- Cópia individual de cada campo
- Botão "Copiar ficha" (bloco pronto para cadastro)
- Extração de `xPed` e `cEAN`
- Colunas condicionais de EAN e unidade tributável

---

## [3.0.0] — 14/11/2026

### Adicionado
- Layout adaptável a monitores retrato e paisagem
- Tabela vira cards empilhados em telas estreitas
- Tema claro e escuro com persistência
- Ajustes de impressão

---

## [2.0.0] — 14/11/2026

### Modificado
- Remoção do painel de configuração de cores
- O/CST com cores semânticas fixas (azul = origem, verde = CST)
- Tooltip com descrição de cada código

---

## [1.0.0] — 14/11/2026

### Adicionado
- Analisador HTML autônomo
- Parser de XML de NF-e com DOMParser
- Extração de dados de produto
- Interface com painel de configuração de cores por dígito

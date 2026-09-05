# data-published

Repositorio publico, so de dados. Aqui ficam snapshots de dados gerados por
projectos meus (privados) que corram automaticamente - sem expor o codigo
fonte desses projectos.

## Estrutura

Uma pasta por projecto de origem, com os ficheiros de dados desse projecto
tal como ele os produz - normalmente em dois formatos, lado a lado:

- JSON (`latest.json` snapshot, `history.jsonl` historico com uma linha
  por dia) - para consumo programatico.
- CSV (`latest.csv` snapshot, `history.csv` historico em formato longo) -
  para ligar directamente a graficos no Excel ou Google Sheets.

```
<nome-do-projecto>/
  latest.json
  history.jsonl
  latest.csv
  history.csv
```

## Projectos publicados aqui

### routine-data-etf

Dados de mercado (preco, retornos, volatilidade, Sharpe ratio, drawdown,
distancia ao ATH) para uma carteira de 11 ETFs/ETPs UCITS europeus,
actualizados nos dias uteis. Codigo fonte num repositorio privado separado.

- `routine-data-etf/latest.json` - snapshot mais recente
- `routine-data-etf/history.jsonl` - uma linha JSON por dia (a execucao
  mais recente desse dia, se tiver corrido mais do que uma vez)
- `routine-data-etf/latest.csv` - o mesmo snapshot em CSV, uma linha por activo
- `routine-data-etf/history.csv` - historico em CSV, formato longo (uma
  linha por activo por dia) - o mais indicado para graficos

Acesso directo (sem autenticacao, repositorio publico):

```
https://raw.githubusercontent.com/dblxpt/data-published/main/routine-data-etf/latest.json
https://raw.githubusercontent.com/dblxpt/data-published/main/routine-data-etf/history.jsonl
https://raw.githubusercontent.com/dblxpt/data-published/main/routine-data-etf/latest.csv
https://raw.githubusercontent.com/dblxpt/data-published/main/routine-data-etf/history.csv
```

Para Google Sheets, `=IMPORTDATA(url)` com um dos URLs `.csv` acima
carrega a tabela directamente numa folha, pronta para graficos.

### crawl-sim-cred

Preçário (TAN/TAEG/MTIC/prestação) de simuladores públicos de crédito
pessoal, para várias combinações de montante, prazo e finalidade —
Cofidis e Cetelem por agora. Atualizado nos dias úteis. Código fonte
no repositório privado
[`dblxpt/crawl-sim-cred`](https://github.com/dblxpt/crawl-sim-cred).

- `crawl-sim-cred/cof.csv` / `cof.json` / `cof_historico.csv` - Cofidis
  (snapshot mais recente, recolha em bruto, histórico cumulativo)
- `crawl-sim-cred/cet.csv` / `cet.json` / `cet_historico.csv` - o mesmo,
  para a Cetelem
- Cada `*_historico.csv` é a **única cópia** desse histórico; o
  repositório privado não guarda duplicado
- `crawl-sim-cred/sim-cred-dash.html` - painel visual com os dois
  bancos, um botão COF/CET para alternar entre eles e outro por
  finalidade dentro do banco escolhido. A grelha de montante/prazo é
  **partilhada entre os dois bancos** (a união dos valores de ambos) —
  a mesma posição de célula representa sempre o mesmo montante/prazo,
  mesmo ao trocar de banco, mesmo havendo escalões que só um dos dois
  usa. Por finalidade mostra: preçário atual, alterações face à última
  recolha em que o preçário foi diferente (pode ter sido há dias ou
  meses) e esse preçário anterior. Lê os dois `*_historico.csv`
  diretamente ao carregar a página (não tem dados embutidos - fica
  sempre atualizado sozinho, e continua a funcionar com só um dos
  bancos se o outro falhar a carregar). Só funciona servido por
  http(s) (ex.: GitHub Pages); aberto como ficheiro local (`file://`)
  o browser bloqueia esse pedido e a página mostra um aviso a explicar.
- `crawl-sim-cred/cof-dash.html` - versão anterior, só Cofidis (mantida
  por compatibilidade com quem já tenha este link).

Acesso directo (sem autenticacao, repositorio publico):

```
https://raw.githubusercontent.com/dblxpt/data-published/main/crawl-sim-cred/cof.csv
https://raw.githubusercontent.com/dblxpt/data-published/main/crawl-sim-cred/cof.json
https://raw.githubusercontent.com/dblxpt/data-published/main/crawl-sim-cred/cof_historico.csv
https://raw.githubusercontent.com/dblxpt/data-published/main/crawl-sim-cred/cet.csv
https://raw.githubusercontent.com/dblxpt/data-published/main/crawl-sim-cred/cet.json
https://raw.githubusercontent.com/dblxpt/data-published/main/crawl-sim-cred/cet_historico.csv
```

Para ver `sim-cred-dash.html` como página (não como texto em bruto),
ativar o GitHub Pages neste repositório (Settings → Pages → Deploy
from branch → `main` → `/ (root)`) e abrir:

```
https://dblxpt.github.io/data-published/crawl-sim-cred/sim-cred-dash.html
```

## Nota

Estes dados sao publicados "as-is", tal como calculados pelos scripts de
origem. Ver o `source_notes` e o campo `notes` de cada activo em
`latest.json`/`latest.csv` para limitacoes, fallbacks e proxies aplicados.

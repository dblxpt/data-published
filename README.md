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
pessoal, para várias combinações de montante, prazo e finalidade — um
banco de cada vez, a começar pela Cofidis. Atualizado nos dias úteis.
Código fonte no repositório privado
[`dblxpt/crawl-sim-cred`](https://github.com/dblxpt/crawl-sim-cred).

- `crawl-sim-cred/cof.csv` - snapshot mais recente (Cofidis)
- `crawl-sim-cred/cof.json` - a mesma recolha em bruto (JSON completo)
- `crawl-sim-cred/cof_historico.csv` - histórico cumulativo (uma linha por
  combinação por dia de recolha) - **única cópia deste histórico**; o
  repositório privado não guarda duplicado
- `crawl-sim-cred/cof-dash.html` - painel visual (tabelas de TAN e
  prestação mensal por montante/prazo). Uma finalidade de cada vez,
  escolhida por botão: preçário atual, alterações face à última recolha
  em que o preçário foi diferente (pode ter sido há dias ou meses) e
  esse preçário anterior. Lê `cof_historico.csv` diretamente ao carregar
  a página (não tem dados embutidos - fica sempre atualizado sozinho).
  Só funciona servido por http(s) (ex.: GitHub Pages); aberto como ficheiro
  local (`file://`) o browser bloqueia esse pedido e a página mostra um
  aviso a explicar.

Acesso directo (sem autenticacao, repositorio publico):

```
https://raw.githubusercontent.com/dblxpt/data-published/main/crawl-sim-cred/cof.csv
https://raw.githubusercontent.com/dblxpt/data-published/main/crawl-sim-cred/cof.json
https://raw.githubusercontent.com/dblxpt/data-published/main/crawl-sim-cred/cof_historico.csv
```

Para ver `cof-dash.html` como página (não como texto em bruto), ativar o
GitHub Pages neste repositório (Settings → Pages → Deploy from branch →
`main` → `/ (root)`) e abrir:

```
https://dblxpt.github.io/data-published/crawl-sim-cred/cof-dash.html
```

## Nota

Estes dados sao publicados "as-is", tal como calculados pelos scripts de
origem. Ver o `source_notes` e o campo `notes` de cada activo em
`latest.json`/`latest.csv` para limitacoes, fallbacks e proxies aplicados.

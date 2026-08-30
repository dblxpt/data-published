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

## Nota

Estes dados sao publicados "as-is", tal como calculados pelos scripts de
origem. Ver o `source_notes` e o campo `notes` de cada activo em
`latest.json`/`latest.csv` para limitacoes, fallbacks e proxies aplicados.


```
deriv/
├── main.py                 # Script principal
├── config.py              # Configurações do sistema
├── data_collector.py      # Coleta de dados via API
├── feature_engineering.py # Pipeline de features
├── ml_model.py           # Modelo de machine learning
├── trader.py             # Executor de trading
├── backtester.py         # Sistema de backtest
├── utils.py              # Utilitários e logging
├── dashboard.py          # Dashboard Streamlit
├── requirements.txt      # Dependências
├── .env.example         # Exemplo de variáveis de ambiente
└── README.md           # Documentação
```

##  Instalação

### 1. Clone o repositório
```bash
git clone <repository-url>
cd deriv
```

### 2. Crie um ambiente virtual
```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate     # Windows
```

### 3. Instale as dependências
```bash
pip install -r requirements.txt
```

### 4. Configure as variáveis de ambiente
```bash
cp .env.example .env
# Edite o arquivo .env com suas credenciais da API Deriv
```

## Deploy no Streamlit Community Cloud

### Pré-requisitos
1. Conta no [GitHub](https://github.com)
2. Conta no [Streamlit Community Cloud](https://streamlit.io/cloud)
3. Credenciais da API Deriv

### Passos para Deploy

1. **Faça fork ou clone este repositório no GitHub**

2. **Configure as variáveis de ambiente no Streamlit Cloud:**
   - `DERIV_APP_ID`: Seu App ID da Deriv
   - `DERIV_API_TOKEN`: Seu token da API Deriv
   - `INITIAL_STAKE`: Valor inicial das operações
   - `MAX_DAILY_LOSS`: Perda máxima diária
   - `ENVIRONMENT`: production

3. **Deploy automático:**
   - Acesse [share.streamlit.io](https://share.streamlit.io)
   - Conecte sua conta GitHub
   - Selecione o repositório
   - Defina o arquivo principal: `dashboard_integration.py`
   - Configure as secrets (variáveis de ambiente)

### URL do App
Após o deploy, seu bot estará disponível em:
```
https://[seu-username]-[nome-do-repo]-[branch]-[hash].streamlit.app
```

## 🔧 Configuração Local

### 3. Instale as dependências
```bash
pip install -r requirements.txt
```

### 4. Configure as variáveis de ambiente
```bash
cp .env.example .env
```

Edite o arquivo `.env` com suas credenciais:
```env
DERIV_APP_ID=your_app_id
DERIV_API_TOKEN=your_api_token
INITIAL_STAKE=1.0
MAX_DAILY_LOSS=50.0
ENVIRONMENT=demo
```

##  Como Usar

### Verificar Status do Sistema
```bash
python main.py --mode status
```

### 1. Coletar Dados Históricos
```bash
python main.py --mode collect
```

### 2. Treinar o Modelo de IA
```bash
python main.py --mode train
```

### 3. Executar Backtest
```bash
python main.py --mode backtest
```

### 4. Iniciar Trading Automático
```bash
python main.py --mode trade
```

### 5. Abrir Dashboard
```bash
python main.py --mode dashboard
```

##  Dashboard

O dashboard oferece:

- 

Acesse em: `http://localhost:8501`

##  Modelo de Machine Learning

### Features Utilizadas
- **Preço**: Open, High, Low, Close, variações
- **Indicadores Técnicos**: RSI, MACD, Bollinger Bands, médias móveis
- **Volatilidade**: ATR, desvio padrão
- **Momentum**: ROC, Williams %R
- **Volume**: Quando disponível
- **Padrões**: Candlestick patterns
- **Temporal**: Hora, dia da semana, lags

### Algoritmo
- **LightGBM**: Gradient boosting otimizado
- **Validação Cruzada**: Time series split
- **Métricas**: Acurácia, precisão, recall, F1-score

##  Gerenciamento de Risco

### Controles Implementados
- **Stake Fixo**: Valor fixo por trade
- **Stop Loss Diário**: Limite máximo de perda
- **Limite de Trades**: Máximo de operações por dia
- **Martingale Opcional**: Progressão após perdas
- **Confiança Mínima**: Threshold para execução

### Configurações de Risco
```python
# Exemplo de configuração
INITIAL_STAKE = 1.0          # $1 por trade
MAX_DAILY_LOSS = 50.0        # Máximo $50 de perda
MAX_DAILY_TRADES = 100       # Máximo 100 trades/dia
MIN_CONFIDENCE = 0.6         # 60% de confiança mínima
ENABLE_MARTINGALE = True     # Martingale ativo
MARTINGALE_MULTIPLIER = 2.0  # 2x após perda
```

##  Estratégia de Trading

### Sinais de Entrada
1. **Previsão do Modelo**: Probabilidade > threshold
2. **Confirmação Técnica**: Indicadores alinhados
3. **Gestão de Risco**: Verificação de limites

### Tipos de Contrato
- **CALL**: Previsão de alta
- **PUT**: Previsão de baixa
- **Duração**: Configurável (padrão: 1 tick)

### Execução
1. Coleta de dados em tempo real
2. Geração de features
3. Previsão do modelo
4. Verificação de risco
5. Execução do trade
6. Monitoramento do resultado

##  Backtesting

### Funcionalidades
- **Simulação Histórica**: Teste com dados passados
- **Métricas Detalhadas**: PnL, drawdown, Sharpe ratio
- **Análise Visual**: Gráficos de performance
- **Comparação de Estratégias**: Múltiplos cenários

### Exemplo de Uso
```python
from backtester import run_simple_backtest

results = run_simple_backtest(
    data_file="data/R_50_ticks.csv",
    start_date="2024-01-01",
    end_date="2024-01-31"
)

print(f"Total PnL: ${results.total_pnl:.2f}")
print(f"Win Rate: {results.win_rate:.2%}")
```

## ogging e Monitoramento

### Logs Disponíveis
- **Trading**: Todas as operações executadas
- **Sistema**: Status e erros do sistema
- **Performance**: Métricas de performance
- **Dados**: Coleta e processamento de dados

### Arquivos de Log
```
logs/
├── bot.log              # Log principal
├── trades.csv          # Histórico de trades
├── daily_reports/      # Relatórios diários
└── performance/        # Métricas de performance
```

##  Configurações Avançadas

### Arquivo config.py
```python
# Exemplo de configuração personalizada
config.trading.symbol = "R_100"
config.trading.initial_stake = 2.0
config.trading.min_confidence = 0.65
config.ml.retrain_interval = 12  # horas
```

### Variáveis de Ambiente
```env
# Trading
SYMBOL=R_50
INITIAL_STAKE=1.0
MAX_DAILY_LOSS=50.0
MAX_DAILY_TRADES=100
MIN_CONFIDENCE=0.6

# Martingale
ENABLE_MARTINGALE=true
MARTINGALE_MULTIPLIER=2.0
MAX_MARTINGALE_STEPS=3

# ML
RETRAIN_INTERVAL=24
MIN_DATA_POINTS=1000

# Sistema
LOG_LEVEL=INFO
ENVIRONMENT=demo
```

##  Segurança

### Boas Práticas
- **Nunca** commite credenciais no código
- Use **sempre** conta demo para testes
- Monitore **constantemente** as operações
- Defina **limites rigorosos** de risco
- Mantenha **backups** dos dados

### Credenciais
- Obtenha App ID em: https://app.deriv.com/account/api-token
- Use tokens com **escopo limitado**
- **Revogue** tokens não utilizados

##  Avisos Importantes
 **ATENÇÃO**: Trading automatizado envolve riscos financeiros significativos.

- **Teste sempre** em conta demo primeiro
- **Nunca** invista mais do que pode perder
- **Monitore** constantemente o sistema
- **Entenda** completamente a estratégia
- **Mantenha** controles de risco rigorosos

##  Desenvolvimento

### Estrutura de Classes Principais

#### TradingExecutor
```python
executor = TradingExecutor()
await executor.start_trading()
```

#### TradingMLModel
```python
model = TradingMLModel()
model.train(data)
prediction = model.predict(features)
```

#### DerivDataCollector
```python
collector = DerivDataCollector()
await collector.connect()
await collector.subscribe_ticks("R_50")
```

### Extensões Futuras

#### Features Avançadas
- **Múltiplas Timeframes**: Análise multi-temporal
- **Order Book**: Dados de profundidade (se disponível)
- **Sentiment Analysis**: Análise de notícias
- **Ensemble Models**: Combinação de modelos

#### Modelos Alternativos
- **LSTM/GRU**: Redes neurais recorrentes
- **Transformers**: Attention para séries temporais
- **Reinforcement Learning**: Aprendizado por reforço

#### Melhorias de Sistema
- **Latência**: Otimização de velocidade
- **Escalabilidade**: Múltiplos símbolos
- **Robustez**: Tratamento de falhas
- **Explainability**: SHAP para interpretação

## Suporte

### Documentação Adicional
- [API Deriv](https://developers.deriv.com/)
- [LightGBM](https://lightgbm.readthedocs.io/)
- [Streamlit](https://docs.streamlit.io/)

### Troubleshooting

#### Problemas Comuns
1. **Conexão WebSocket**: Verificar credenciais
2. **Modelo não treina**: Verificar dados suficientes
3. **Dashboard não abre**: Verificar porta 8501
4. **Trades não executam**: Verificar limites de risco

#### Logs de Debug
```bash
# Ativar logs detalhados
export LOG_LEVEL=DEBUG
python main.py --mode trade
```

## 📄 Licença

Este projeto é fornecido "como está" para fins educacionais. Use por sua própria conta e risco.

---

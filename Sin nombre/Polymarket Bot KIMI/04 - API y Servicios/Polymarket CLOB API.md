# Polymarket CLOB API

Central Limit Order Book (CLOB) de Polymarket para trading real.

## Inicialización

```python
ClobClient(
    host="https://clob.polymarket.com",
    key=CONFIG.polymarket_pk,
    chain_id=137,  # Polygon Mainnet
    funder=CONFIG.polymarket_funder,
    signature_type=3,  # EIP-1271
)
```

## Autenticación

### Método 1: Credenciales existentes
```python
client = ClobClient(..., creds=ApiCreds(
    api_key=CONFIG.polymarket_api_key,
    api_secret=CONFIG.polymarket_api_secret,
    api_passphrase=CONFIG.polymarket_api_passphrase,
))
```

### Método 2: Derivar de wallet
```python
client = ClobClient(...)
creds = client.derive_api_key()
client.set_api_creds(creds)
```

## Endpoints usados

| Endpoint | Uso |
|----------|-----|
| `GET /balance-allowance` | Verificar balance pUSD |
| `POST /order` | Colocar orden de mercado |
| `POST /auth/api-key` | Crear/derivar API key |

## Errores comunes

| Error | Causa | Solución |
|-------|-------|----------|
| `400 Could not create api key` | Sin gas para la tx | Usar credenciales existentes o derivar |
| `401 Unauthorized` | API key inválida | Generar nuevas credenciales |
| `INSUFFICIENT_BALANCE` | pUSD insuficiente | Depositar en Polymarket |

## Balance

1 pUSD = 1 USDC en Polygon (6 decimales).

```python
balance = client.get_balance_allowance(
    BalanceAllowanceParams(asset_type=AssetType.COLLATERAL)
)
# balance['balance'] en micro-USDC (1_000_000 = $1.00)
```

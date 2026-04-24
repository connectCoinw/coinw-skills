---
{
  "name": 'Coinw Account Skill',
  "description": 'Coinw Common Account REST API skill: covers account setup docs, deposit/withdraw address and history, withdrawal actions, account transfer, sub-account management, and sub-account transfers.',
  "metadata": {"version": "1.0.0","author": "Coinw","openclaw":{"always": true,"requires":{"env":["COINW_API_KEY","COINW_SECRET_KEY"]}}}
}
---

# Coinw Account Skill

Coinw Common Account REST API skill: covers account setup docs, deposit/withdraw address and history, withdrawal actions, account transfer, sub-account management, and sub-account transfers.

### Setup Credentials
CoinW private endpoints require `api_key` and a request signature (`sign`).

> Signing note: Account skill follows Spot/Common signing (MD5 uppercase). Do not use Contract HMAC-SHA256 signing for these endpoints.

1. Environment variables:
```bash
export COINW_API_KEY="your_api_key"
export COINW_SECRET_KEY="your_secret_key"
```
2. In chat: provide `api_key`/`secret_key` (and an account name). The agent will mask secrets when showing them back and store them securely in OpenClaw's credential storage (not inside skill markdown files).

## Key Features
- Introduction and compliance docs: account/API creation, precautions, and error-code guidance
- Account operations: get deposit/withdraw address and history, initiate/cancel withdrawal, account transfer
- Sub-account operations: list sub-accounts, list API keys, reset API key, query transferable amount, query/execute sub-account transfer

## Quick Reference

### Introduction

| No. | name | Endpoint | Description | Method | Authentication | Input Parameters | Output Parameters |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1.1 | Introduction | `/api-doc/common/introduction` | Basic onboarding guidance for CoinW API usage, auth flow, and code examples. | DOC | Public | — | — |
| 1.2 | Account and API creation | `/api-doc/common/introduction/account-api-creation` | Steps for account registration, security setup, KYC, and API key creation. | DOC | Public | — | — |
| 1.3 | Precautions | `/api-doc/common/precautions` | Important request rules and safety considerations before integrating APIs. | DOC | Public | — | — |
| 1.4 | Error code description | `/api-doc/common/introduction/error-codes` | Common API error-code definitions and troubleshooting guidance. | DOC | Public | — | — |

### Account

| No. | name | Endpoint | Description | Method | Authentication | Input Parameters | Output Parameters |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2.1 | Get deposit and withdrawal address | `/api/v1/private?command=returnDepositAddresses` | Gets deposit/withdraw address for a specified coin and chain. | POST | Private | api_key, sign, symbolId, chain | minRechargeAmount, chainName, address |
| 2.2 | Get deposit and withdrawal history | `/api/v1/private?command=returnDepositsWithdrawals` | Gets deposit/withdraw history summary for a specified asset. | POST | Private | api_key, sign, symbol, depositNumber | amount, chain, side, depositNumber, address, txid, memo, currency, and 14 total fields |
| 2.3 | Initiate withdrawal | `/api/v1/private?command=doWithdraw` | Initiates on-chain or internal withdrawal with amount/address/chain details. | POST | Private | api_key, sign, memo, type, amount, currency, address, chain, innerToType | depositNumber |
| 2.4 | Cancel withdrawal | `/api/v1/private?command=cancelWithdraw` | Cancels a submitted withdrawal by withdrawal request ID. | POST | Private | api_key, sign, id | msg |
| 2.5 | Account transfer | `/api/v1/private?command=spotWealthTransfer` | Transfers assets between account types (for example spot and funding). | POST | Private | api_key, sign, accountType, targetAccountType, bizType, coinCode, amount | data, msg |

### Sub-Account

| No. | name | Endpoint | Description | Method | Authentication | Input Parameters | Output Parameters |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 3.1 | Get sub-account list | `/api/v1/private?command=subAccount_list` | Lists all sub-accounts under the master account with status fields. | POST | Private | api_key, sign | data, spotUid, userCode, nickName, frozenFlag, spotTradeFlag, contractTradeFlag |
| 3.2 | Get sub-account API key list | `/api/v1/private?command=subAccount_apikey` | Gets API key metadata and permissions for sub-accounts. | POST | Private | api_key, sign | data, userCode, nickName, apiRemark, apiKey, createdAt, spotTradeFlag, contractTradeFlag, and 11 total fields |
| 3.3 | Reset sub-account API key | `/api/v1/private?command=reset_subAccount_apikey` | Resets API key permissions for a specified sub-account key. | POST | Private | api_key, sign, subAccount_apiKey | data |
| 3.4 | Get max transferable amount of sub-account | `/api/v1/private?command=subAccount_transferAvailable` | Queries max transferable amount by sub-account UID and coin. | POST | Private | api_key, sign, coinCode, subUid | data |
| 3.5 | Get master/sub transfer history | `/api/v1/private?command=returnTransferHistory` | Gets internal transfer history across master/sub accounts and account types. | POST | Private | api_key, sign, currency, from, to, pageNo, pageSize | data, currency, createTime, amount, from, to, status, ledgerId |
| 3.6 | Transfer assets between master and sub-account | `/api/v1/private?command=subAccount_transfer` | Transfers assets between master account and sub-account in both directions. | POST | Private | api_key, sign, side, subUid, currency, amount | data, bizNo |

## Common Parameters and Enums

### Auth and URL
- Base URL: `https://api.coinw.com`.
- Common docs pages: `https://www.coinw.com/api-doc/common/...`.
- Private REST endpoints require `POST https://api.coinw.com/api/v1/private?command=...` with `api_key` and `sign` (MD5, see Reference).
### `command` values (private endpoints covered in this file)
`cancelWithdraw`, `doWithdraw`, `reset_subAccount_apikey`, `returnDepositAddresses`, `returnDepositsWithdrawals`, `returnTransferHistory`, `spotWealthTransfer`, `subAccount_apikey`, `subAccount_list`, `subAccount_transfer`, `subAccount_transferAvailable`
### Common request fields
- **symbol / symbolId / currency / coinCode**: asset identifier fields (names vary by endpoint).
- **chain**: chain/network name for deposit/withdraw operations.
- **subUid / side / from / to**: master-sub account transfer direction and target account identifiers.
### Standard response wrapper (common in REST)
- Common top-level fields: `code`, `msg` / `message`, `success`, `failed`, `data` (actual response varies by endpoint).
### Common enums and values
- **type** (withdraw): transfer type depends on endpoint spec and account context.
- **side** (sub-account transfer): transfer direction between master and sub-account.
- **status** (history rows): record status returned by transfer/withdraw history APIs.

## Examples
### POST (private endpoint)
```bash
params="api_key=$COINW_API_KEY&symbolId=50&chain=BTC"
sign_string="$params&secret_key=$COINW_SECRET_KEY"
sign=$(echo -n "$sign_string" | openssl md5 | cut -d' ' -f2 | tr '[:lower:]' '[:upper:]')
curl -X POST "https://api.coinw.com/api/v1/private?command=returnDepositAddresses&$params&sign=$sign"
```
### Sub-account transfer (private endpoint)
```bash
params="api_key=$COINW_API_KEY&side=1&subUid=10000001&currency=USDT&amount=10"
sign_string="$params&secret_key=$COINW_SECRET_KEY"
sign=$(echo -n "$sign_string" | openssl md5 | cut -d' ' -f2 | tr '[:lower:]' '[:upper:]')
curl -X POST "https://api.coinw.com/api/v1/private?command=subAccount_transfer&$params&sign=$sign"
```
## Security
When showing credentials to users:
- **API Key:** Show first 4 + last 5 characters: `12&*1...198I`
- **Secret Key:** Always mask, show only last 4: `***...isf1`
- Ask for user confirmation before any withdrawal or transfer action.
- Store user `api_key` and `secret_key` in a secure location.

## Agent Behavior

1. Credentials requested: Mask secrets (show last 5 chars only)
2. Listing accounts: Show names never keys
3. New credentials: Prompt for name, signing mode

## Adding New Accounts

When user provides new credentials:

* Ask for account name, api_key, secret_key
* Store the provided credentials in OpenClaw's secure credential store with masked display confirmation 

## Reference
- Authentication`./references/Authentication.md`
- errorcode: `./references/error-codes.md`
- notes: `./references/notes.md`
- api-key create steps: `./references/api-key-creation-steps.md`

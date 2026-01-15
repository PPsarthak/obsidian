---
status: Started
last-reviewed: 2025-09-04
tags:
  - western-union
  - domain-knowledge
---
#western-union #domain-knowledge
#### Basic Treasury Concepts
1. **Cash and Liquidity**: Ensuring the company has enough cash to meet obligations.
2. **Payments and Collections**: Handling outgoing and incoming payments.
3. **Liquidity Management**: Monitoring inflows/outflows and short-term borrowing/lending to stay solvent.
4. **Bank Account Management**: Managing multiple accounts across geographies.
5. **Forecasting**: Predicting future cash flows.
6. **Risk Management**: Managing currency, interest rate, and credit risks.
7. **Reconciliation**: Matching bank statements with internal records. It typically means matching bank statements (from SWIFT MT940 or CAMT.053 files) with internal accounting records (from ERP systems like SAP or Oracle).
#### SWIFT -  Society for Worldwide Interbank Financial Telecommunication
* **Founded**: 1973, headquartered in Belgium.
* **Purpose**: Provides a secure messaging network for financial institutions to exchange information about financial transactions.
* **Members**: Over 11,000 institutions in more than 200 countries.

> [!note] 
> SWIFT does not move money itself.
> It sends standardized messages between banks and financial institutions to initiate and confirm transactions like:
> * Payments (e.g., wire transfers)
> * Securities trades
> * Foreign exchange
> * Treasury transactions 
> 
> Message Types:
> * MT (Message Type): Legacy format (e.g., MT103 for payments).
> * MX (ISO 20022): Modern XML-based format (e.g., pain.001, camt.053).
#### File Types
##### MT - Message Type (Flat Text Format)
* **MT940**: End-of-day bank statement (used for reconciliation).
* **MT950**: Daily statement message (similar to MT940 but more detailed).
* **MT101**: Request for transfer (used to initiate payments).
* **MT103**: Customer credit transfer (used for international payments).
* **MT 202**: Bank to bank transfer

```
{1:F01BANKBEBBAXXX0000000000}{2:I940BANKDEFFXXXXN}{4:
:20:STATEMENT12345
:25:123456789/123-4567890
:28C:00001/001
:60F:C240902EUR12345,67
:61:2409020902D100,00NTRFNONREF//123456789
:86:Payment to Vendor ABC
:61:2409020902C500,00NTRFNONREF//987654321
:86:Incoming Payment from Customer XYZ
:62F:C240902EUR12745,67
-}
```
###### Breakdown of Fields
- **`:20:`** → Transaction Reference Number (unique ID).
- **`:25:`** → Account Identification (IBAN or account number).
- **`:28C:`** → Statement Number/Sequence.
- **`:60F:`** → Opening Balance (Format: Debit/Credit, Date, Currency, Amount).
	- Example: C240902EUR12345,67 → Credit balance on Sept 2, 2024, EUR 12,345.67.
- **`:61:`** → Transaction Line.
	- `:61:<Value Date><Entry Date><Debit/Credit><Amount><Transaction Type><Reference>//<Bank Reference>` 
	-   2409020902D100,00NTRFNONREF//123456789
	- Breaking Down the Example
		- 240902 → Value Date: 2nd September 2024
		- 0902 → Entry Date: 2nd September (sometimes omitted if same as value date)
		- D → Debit/Credit Indicator
			- D = Debit (money going out)
			- C = Credit (money coming in)
		- 100,00 → Amount: 100.00 (European notation uses , for decimal)
		- NTRF → Transaction Type Code
			- "TRF" = Transfer
			- "NTRF" means a transfer with a narrative (extra info in :86:)
		- NONREF → Customer Reference (could be NONREF if none provided)
		- //123456789 → Bank Reference / Transaction Identifier
- **`:86:`** → Transaction Description (free text narrative) - exists iff there is NTRF mentioned above, if it is TRF then this field does not exists
- **`:62F:`** → Closing Balance.
	- Example: C240902EUR12745,67 → Credit balance after transactions, EUR 12,745.67.
##### MX – ISO 20022 XML Messages
The modern replacement for MT messages. They are:
XML-based, more structured and flexible.
Used in SEPA, SWIFT gpi, and other modern payment systems.

Examples:
* pain.001: Payment initiation.
* pain.002: Payment status report.
* pacs.008 → Equivalent of MT103.
##### CAMT (Cash Management) – ISO 20022 Subset
These are part of MX messages and used for cash reporting:
- camt.052: Real-time/intra-day reports.
- camt.053: End-of-day bank statements.
- camt.054: Transaction-level notifications.
##### BAI / BAI2 – Bank Administration Institute Format (Non-SWIFT ????)
Used mainly in North America for cash reporting:
- BAI2 is the updated version.
- Text-based format with codes for transaction types.
- Used for bank statements and reconciliation.





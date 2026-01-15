#western-union 

```xml
<Document xmlns="urn:iso:std:iso:20022:tech:xsd:camt.053.001.04">
  <BkToCstmrStmt>
    <GrpHdr>
      <MsgId>STATEMENT98765</MsgId>
      <CreDtTm>2024-09-02T23:59:59</CreDtTm>
    </GrpHdr>

    <Stmt>
      <Id>00002/001</Id>
      <ElctrncSeqNb>1</ElctrncSeqNb>
      <CreDtTm>2024-09-02T23:59:59</CreDtTm>
      <Acct>
        <Id>
          <IBAN>DE12345678901234567890</IBAN>
        </Id>
      </Acct>

      <!-- Opening Balance -->
      <Bal>
        <Tp><CdOrPrtry><Cd>OPBD</Cd></CdOrPrtry></Tp>
        <Amt Ccy="EUR">5000.00</Amt>
        <CdtDbtInd>CRDT</CdtDbtInd>
        <Dt><Dt>2024-09-01</Dt></Dt>
      </Bal>

      <!-- Transaction 1: Debit -->
      <Ntry>
        <Amt Ccy="EUR">250.00</Amt>
        <CdtDbtInd>DBIT</CdtDbtInd>
        <Sts>BOOK</Sts>
        <BookgDt><Dt>2024-09-01</Dt></BookgDt>
        <ValDt><Dt>2024-09-01</Dt></ValDt>
        <BkTxCd>
          <Domn><Fmly><SubFmlyCd>TRF</SubFmlyCd></Fmly></Domn>
        </BkTxCd>
        <NtryDtls>
          <TxDtls>
            <RmtInf>
              <Ustrd>Payment to Supplier ABC - Invoice 789</Ustrd>
            </RmtInf>
          </TxDtls>
        </NtryDtls>
      </Ntry>

      <!-- Transaction 2: Credit -->
      <Ntry>
        <Amt Ccy="EUR">1000.00</Amt>
        <CdtDbtInd>CRDT</CdtDbtInd>
        <Sts>BOOK</Sts>
        <BookgDt><Dt>2024-09-01</Dt></BookgDt>
        <ValDt><Dt>2024-09-01</Dt></ValDt>
        <BkTxCd>
          <Domn><Fmly><SubFmlyCd>TRF</SubFmlyCd></Fmly></Domn>
        </BkTxCd>
        <NtryDtls>
          <TxDtls>
            <RmtInf>
              <Ustrd>Incoming Payment from Customer XYZ - Invoice 456</Ustrd>
            </RmtInf>
          </TxDtls>
        </NtryDtls>
      </Ntry>

      <!-- Transaction 3: Debit -->
      <Ntry>
        <Amt Ccy="EUR">150.00</Amt>
        <CdtDbtInd>DBIT</CdtDbtInd>
        <Sts>BOOK</Sts>
        <BookgDt><Dt>2024-09-02</Dt></BookgDt>
        <ValDt><Dt>2024-09-02</Dt></ValDt>
        <BkTxCd>
          <Domn><Fmly><SubFmlyCd>CHRG</SubFmlyCd></Fmly></Domn>
        </BkTxCd>
        <NtryDtls>
          <TxDtls>
            <RmtInf>
              <Ustrd>Cheque Payment - Payroll Deduction</Ustrd>
            </RmtInf>
          </TxDtls>
        </NtryDtls>
      </Ntry>

      <!-- Transaction 4: Credit -->
      <Ntry>
        <Amt Ccy="EUR">500.00</Amt>
        <CdtDbtInd>CRDT</CdtDbtInd>
        <Sts>BOOK</Sts>
        <BookgDt><Dt>2024-09-02</Dt></BookgDt>
        <ValDt><Dt>2024-09-02</Dt></ValDt>
        <BkTxCd>
          <Domn><Fmly><SubFmlyCd>TRF</SubFmlyCd></Fmly></Domn>
        </BkTxCd>
        <NtryDtls>
          <TxDtls>
            <RmtInf>
              <Ustrd>Refund from Vendor LMN</Ustrd>
            </RmtInf>
          </TxDtls>
        </NtryDtls>
      </Ntry>

      <!-- Closing Balance -->
      <Bal>
        <Tp><CdOrPrtry><Cd>CLBD</Cd></CdOrPrtry></Tp>
        <Amt Ccy="EUR">6100.00</Amt>
        <CdtDbtInd>CRDT</CdtDbtInd>
        <Dt><Dt>2024-09-02</Dt></Dt>
      </Bal>
    </Stmt>
  </BkToCstmrStmt>
</Document>
```

*`<Document>`*: 
- Root element of the XML
- Attribute: `xmlns="urn:iso:std:iso:20022:tech:xsd:camt.053.001.04"`
	- camt = Cash Management
	- 053 = Statement message (end-of-day)
	- 001.04 = Version 4 of the message schema
*`<BkToCstmrStmt>`*: 
- Stands for Bank To Customer Statement
- Encloses all content of the statement, including header, account details, balances and transactions
*`<GrpHdr>`*:
- Group Header contains metadata
*`<MsgId>`*:
- Message Identifier (unique ID) assigned by the sending institution (the bank) - STATEMENT98765
*`<CreDtTm>`*:
- Timestamp when the message was created by the bank - 2024-09-02T23:59:59
*`<Stmt>`*:
- Represents one statement within the file. 
- A single camt.053 file may contain one or multiple `<Stmt>` blocks, each for a different account or period.
*`<Id>`*:
- Statement Identifier that identifies other statements among others (assigned by bank)
*`<ElctrncSeqNb>`*:
- Electronic Sequence Number - number for statements sent electronically
- 1 → this is the first electronic statement for that account on that date. If multiple statements are sent the same day (say intra-day cut-offs), you might see 2, 3, etc.
*`<CreDtTm>`*:
- Creation Date and Time for the statement
- 2024-09-02T23:59:59 → this is an end-of-day statement
*`<Acct>`*:
- Identifies the account the statement refers to
*`<Id> → <IBAN>`*:
- Holds the International Bank Account Number of customer's account
*`<Tp> → <CdOrPrtry> → <Cd>`*:
- Balance type code
- OPBD = Opening Booked Balance (balance at start of the reporting period)
- CdOrPrtry = Credit or Proprietary 
*`<Amt Ccy="EUR">5000.00</Amt>`*:
- Balance amount (€5,000)
- Ccy attribute shows the currency
*`<CdtDbtInd>CRDT</CdtDbtInd>`*:
- Credit or Debit Indicator
*`<Dt>`*:
- Date of balance
*`<Ntry>`*:
- Each Entry 
*`<Amt>`* → €250.00
*`<CdtDbtInd>DBIT</CdtDbtInd>`* → Debit entry (money leaving the account).
*`<Sts>BOOK</Sts>`* → Status: Booked (already posted to the account).
*`<BookgDt>`* → Booking date = 1st Sept 2024 (date bank recorded it).
*`<ValDt>`* → Value date = 1st Sept 2024 (date used for interest/cash value).
*`<BkTxCd> → <SubFmlyCd>TRF</SubFmlyCd>`*
- Transaction code: TRF = Transfer.
- Means this debit was a transfer payment.
*`<NtryDtls> → <TxDtls> → <RmtInf> → <Ustrd>`*
- Unstructured remittance information.
- Free-text reference: “Payment to Supplier ABC - Invoice 789”.
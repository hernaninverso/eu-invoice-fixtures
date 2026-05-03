# UBL — expected failures

## negative-quantity.xml

**Format:** ubl
**Expected error:** BR-22, BR-CO-04 / line quantity must be positive
**Description:** A regular Invoice (TypeCode 380) cannot have negative line items. Negative invoicing is only permitted via a CreditNote document (TypeCode 381) where credited amounts are positive but represent reversals.

```
[ERROR] BR-22: Each Invoice line MUST have an Invoiced quantity (BT-129).
[ERROR] BR-CO-04: Each Invoice line (BG-25) MUST have an Invoice line net
                  amount (BT-131) which equals the sum of line amounts
                  (positive).
```

**How to fix:** if you intend to credit, change `InvoiceTypeCode` to `381` and
emit as a `<CreditNote>` document with positive amounts (the credit note
implicitly represents reversal).

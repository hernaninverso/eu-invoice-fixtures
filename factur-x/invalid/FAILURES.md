# Factur-X — expected failures

## missing-currency.xml

**Format:** factur-x
**Expected error:** BR-05
**Description:** `InvoiceCurrencyCode` is mandatory in CII semantic model. Without it, the invoice has no defined currency and cannot represent any monetary value unambiguously.

```
[ERROR] BR-05: An Invoice MUST have an Invoice currency code (BT-5).
```

**How to fix:** add `<ram:InvoiceCurrencyCode>EUR</ram:InvoiceCurrencyCode>` as the first child of `<ram:ApplicableHeaderTradeSettlement>`.

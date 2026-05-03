# Expected validation failures

## missing-vat-id.xml

**Format:** peppol-bis-3
**Expected error:** BR-CO-9 — Supplier VAT identifier is required
**Helger phive output excerpt:**

```
[ERROR] BR-CO-9: An invoice MUST have a Supplier VAT identifier
                 (BT-31) where the Seller is registered for VAT.
```

**Why this happens:** PartyTaxScheme/CompanyID is missing from the supplier's
Party block. EN16931 requires a VAT ID for any invoice issued by a VAT-registered
seller (which is anyone above the small-business threshold in DE/FR/IT/ES).

**How to fix:** add this block under AccountingSupplierParty/Party:

```xml
<cac:PartyTaxScheme>
  <cbc:CompanyID>DE123456789</cbc:CompanyID>
  <cac:TaxScheme><cbc:ID>VAT</cbc:ID></cac:TaxScheme>
</cac:PartyTaxScheme>
```

---

## wrong-currency-iso.xml

**Format:** peppol-bis-3
**Expected error:** BR-DEC-08 / Code list ISO 4217 violation
**Description:** `DocumentCurrencyCode` and `currencyID` attribute must be a 3-letter ISO 4217 code (EUR, USD, GBP, CHF...). `USDX` is not a valid ISO code.

```
[ERROR] BR-DEC-08: The Invoice currency code (BT-5) MUST be coded using ISO 4217.
```

**How to fix:** use a real ISO code (`EUR`, `USD`, `GBP`, `JPY`, etc).

---

## missing-issue-date.xml

**Format:** peppol-bis-3
**Expected error:** BR-03 / cardinality violation
**Description:** Every invoice MUST contain an `IssueDate` (BT-2). Without it the receiver cannot apply payment terms or verify VAT period.

```
[ERROR] BR-03: An Invoice MUST have an Invoice issue date (BT-2).
```

**How to fix:** add `<cbc:IssueDate>YYYY-MM-DD</cbc:IssueDate>` after `<cbc:ID>`.

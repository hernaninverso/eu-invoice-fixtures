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

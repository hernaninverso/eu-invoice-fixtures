# Fixtures index

| File | Format | Valid? | Notes |
|------|--------|--------|-------|
| `peppol-bis-3/valid/01-simple-services.xml` | Peppol BIS 3 | ✅ | DE→FR, single-line consulting hours |
| `peppol-bis-3/valid/02-credit-note.xml` | Peppol BIS 3 | ✅ | CreditNote (TypeCode 381) referencing 01 |
| `peppol-bis-3/valid/03-with-allowance.xml` | Peppol BIS 3 | ✅ | Document-level allowance (volume discount) |
| `peppol-bis-3/valid/04-reverse-charge.xml` | Peppol BIS 3 | ✅ | Intra-EU B2B with VAT reverse charge (AE) |
| `peppol-bis-3/valid/05-multi-line.xml` | Peppol BIS 3 | ✅ | Multi-line, mixed VAT categories (S 21% + AA 10%) |
| `peppol-bis-3/invalid/missing-vat-id.xml` | Peppol BIS 3 | ❌ | Supplier missing PartyTaxScheme/CompanyID — BR-CO-9 |
| `peppol-bis-3/invalid/wrong-currency-iso.xml` | Peppol BIS 3 | ❌ | DocumentCurrencyCode `USDX` not in ISO 4217 — BR-DEC-08 |
| `peppol-bis-3/invalid/missing-issue-date.xml` | Peppol BIS 3 | ❌ | IssueDate (BT-2) missing — BR-03 |
| `xrechnung-2.x/valid/01-services-leitweg-id.xml` | XRechnung 2.3 | ✅ | B2G with Leitweg-ID + Skonto reference |
| `xrechnung-2.x/valid/02-reduced-vat.xml` | XRechnung 2.3 | ✅ | German reduced VAT 7% (category AA) for books |
| `xrechnung-2.x/valid/03-with-payment-terms.xml` | XRechnung 2.3 | ✅ | Skonto early-pay discount in PaymentTerms.Note |
| `xrechnung-2.x/invalid/missing-buyer-reference.xml` | XRechnung 2.3 | ❌ | Missing BuyerReference (Leitweg-ID) — BR-DE-15 |
| `xrechnung-2.x/invalid/wrong-vat-rate.xml` | XRechnung 2.3 | ❌ | VAT 10% for category S (DE has S=19%) — BR-S-08 |
| `factur-x/valid/01-basic-profile.xml` | Factur-X | ✅ | BASIC profile, single line |
| `factur-x/valid/02-en16931-profile.xml` | Factur-X | ✅ | EN16931 profile, full conformance |
| `factur-x/invalid/missing-currency.xml` | Factur-X | ❌ | Missing InvoiceCurrencyCode — BR-05 |
| `ubl/valid/01-simple-product-invoice.xml` | UBL 2.1 | ✅ | ES→GB cross-border product invoice |
| `ubl/valid/02-with-prepayment.xml` | UBL 2.1 | ✅ | PT→NL with PrepaidAmount → reduced PayableAmount |
| `ubl/invalid/negative-quantity.xml` | UBL 2.1 | ❌ | InvoiceTypeCode 380 with negative qty — BR-22 |

**Total:** 19 fixtures (12 valid + 7 invalid).

## How to test against eleata API

```bash
# Validate a known-valid file
curl -X POST 'https://api.eleata.io/v1/validate?format=peppol-bis-3' \
  -H "Authorization: Bearer $ELEATA_KEY" \
  -H "Content-Type: application/xml" \
  --data-binary @peppol-bis-3/valid/01-simple-services.xml

# Should return: {"valid": true, ...}

# Validate a known-invalid file
curl -X POST 'https://api.eleata.io/v1/validate?format=peppol-bis-3' \
  -H "Authorization: Bearer $ELEATA_KEY" \
  -H "Content-Type: application/xml" \
  --data-binary @peppol-bis-3/invalid/missing-vat-id.xml

# Should return: {"valid": false, "errors": [...BR-CO-9...]}
```

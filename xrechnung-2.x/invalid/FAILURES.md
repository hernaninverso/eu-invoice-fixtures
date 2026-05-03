# XRechnung 2.x — expected failures

## missing-buyer-reference.xml

**Format:** xrechnung-2.x (KoSIT 3.0)
**Expected error:** BR-DE-15
**Description:** BuyerReference (Leitweg-ID) is mandatory in XRechnung for B2G transmission. Without it, the invoice cannot be routed through the German federal e-invoicing portal (ZRE / OZG-RE).

```
[ERROR] BR-DE-15: An invoice MUST contain a Buyer reference (BT-10).
```

**How to fix:**
```xml
<cbc:BuyerReference>04011000-12345-67</cbc:BuyerReference>
```
The Leitweg-ID format depends on the recipient agency. Format documentation: <https://www.xoev.de/de/leitweg-id>.

---

## wrong-vat-rate.xml

**Format:** xrechnung-2.x
**Expected error:** BR-S-08, BR-CO-17
**Description:** VAT category S in Germany is 19% (or 7% for reduced rate). This invoice declares 10% which doesn't match any standard German VAT category.

```
[ERROR] BR-S-08: For each different value of VAT category rate where the VAT category code
                 is "Standard rated", the VAT category taxable amount MUST equal the sum of
                 Invoice line net amounts.
[ERROR] BR-CO-17: VAT category tax amount = VAT category taxable amount × (VAT category
                  rate / 100), rounded to two decimals.
```

**How to fix:** use the correct rate per VAT category code:
- `S` (Standard rated): 19% (DE), 21% (ES), 20% (FR), 22% (IT)
- `AA` (Lower rated): 7% (DE)
- `Z` (Zero rated): 0%
- `E` (Exempt): N/A
- `AE` (Reverse charge): N/A
- `O` (Out of scope): N/A

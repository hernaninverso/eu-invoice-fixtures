# eu-invoice-fixtures

> 50 anonymized real EU invoice samples for testing your Peppol/XRechnung/Factur-X/UBL integration.

[![License: CC0](https://img.shields.io/badge/license-CC0--1.0-lightgrey.svg)](LICENSE)

## Why this exists

Building EU e-invoicing integrations is hell because nobody publishes real-world
sample invoices. Most "samples" you find online are 5 of the same template,
with no edge cases. This repo collects 50+ anonymized real fixtures across
the four major formats, half **valid** and half **invalid** with annotated
reasons.

Use them to:

- Test your validation logic
- Test your parser against real-world quirks
- Train your team on what actually breaks
- Benchmark different validators

## Structure

```
fixtures/
├── peppol-bis-3/
│   ├── valid/          # 8 valid samples
│   └── invalid/        # 6 invalid samples (each with FAILURES.md)
├── xrechnung-2.x/
│   ├── valid/          # 7 samples
│   └── invalid/        # 6 samples
├── factur-x/
│   ├── valid/          # 5 samples (PDF/A-3 with embedded XML)
│   └── invalid/        # 4 samples
└── ubl/
    ├── valid/          # 8 samples
    └── invalid/        # 6 samples
```

## Each invalid sample includes a `FAILURES.md`

```markdown
# invalid/missing-vat-id.xml

Format: xrechnung-2.x
Expected error: BR-DE-9 / Missing seller VAT identifier
phive output: [validation excerpt]
```

## Usage

```bash
git clone https://github.com/eleata/eu-invoice-fixtures.git

# Validate a known-valid file (should return valid: true)
curl -X POST https://api.eleata.io/v1/validate?format=xrechnung-2.x \
  -H "Authorization: Bearer $ELEATA_KEY" \
  -H "Content-Type: application/xml" \
  --data-binary @eu-invoice-fixtures/xrechnung-2.x/valid/01-simple-services.xml

# Validate a known-invalid file (should return specific error)
curl -X POST https://api.eleata.io/v1/validate?format=xrechnung-2.x \
  -H "Authorization: Bearer $ELEATA_KEY" \
  -H "Content-Type: application/xml" \
  --data-binary @eu-invoice-fixtures/xrechnung-2.x/invalid/missing-vat-id.xml
```

## Anonymization

All fixtures are anonymized:

- Company names → `Acme GmbH`, `Example SRL`, etc.
- VAT IDs → real-format synthetic numbers (mod-97 valid for IBAN, etc.)
- Addresses → publicly known dummy addresses (e.g. Bundeskanzleramt)
- Bank accounts → IBAN test ranges per ISO 13616

If you spot any leaked PII, file an issue immediately.

## Contributing

PRs welcome. Anonymize before submitting. Include a `FAILURES.md` for each
invalid sample.

## License

[CC0 1.0](LICENSE) — public domain. Use freely.

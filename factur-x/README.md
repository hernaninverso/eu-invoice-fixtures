# Factur-X fixtures

Factur-X (also "ZUGFeRD" in Germany) is a hybrid format: a PDF/A-3 with
embedded XML invoice data (UBL or CII).

Because the format requires a real PDF/A-3 binary, sample fixtures cannot
be plain XML. Each sample below is the **embedded XML extracted** from a
real Factur-X PDF, packaged as a standalone `.xml` for easy testing of the
XML validation layer.

To generate a real `.pdf` from these XML samples for end-to-end testing:

```bash
# Using mustangproject CLI (BSD-licensed, github.com/ZUGFeRD/mustangproject)
java -jar Mustang-CLI.jar --action a3only \
  --source factur-x/valid/01-basic-profile.xml \
  --out factur-x/valid/01-basic-profile.pdf
```

## Profile coverage

- **MINIMUM** — invoice ID + amounts + parties only
- **BASIC WL** — without lines (header + tax summary)
- **BASIC** — minimal line items
- **EN 16931** — full EN16931 conformance (= Peppol BIS 3 minus CIUS-specific tweaks)
- **EXTENDED** — French-specific extensions (Chorus Pro / PPF)

Our included samples cover BASIC, EN 16931, and EXTENDED.

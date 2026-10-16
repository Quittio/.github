<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Quittio/quittio/master/art/banner-dark.svg">
    <img src="https://raw.githubusercontent.com/Quittio/quittio/master/art/banner-light.svg" alt="Quittio: Invoices, settled. E-invoicing for PHP and Laravel" width="1280">
  </picture>
</p>

<p align="center">
  <b>E-invoicing for PHP and Laravel.</b> Create, validate, read and render electronic invoices based on EN 16931:<br>
  ZUGFeRD/Factur-X, XRechnung, UBL and Peppol BIS from one data model.
</p>

<p align="center">
  <a href="https://docs.quittio.dev"><b>Documentation</b></a> ·
  <a href="https://docs.quittio.dev/getting-started.html">Getting started</a> ·
  <a href="https://github.com/Quittio/quittio">Main repository</a> ·
  <a href="https://github.com/Quittio/quittio/issues">Issues</a>
</p>

---

Germany is the first market. Everything country-specific (VAT rates, legal texts, XRechnung rules) lives in country packs, so the core stays country-neutral.

```bash
composer require quittio/quittio
php artisan quittio:install
```

```php
Quittio::invoice()
    ->number('RE-2026-0042')
    ->buyer(Party::make('Kunde AG')->vatId('DE987654321'))
    ->line('Web development', netPrice: '95.00', quantity: 24, unit: UnitCode::Hour)
    ->dueInDays(14)
    ->render()        // ZUGFeRD: PDF/A-3 with embedded XML
    ->save('invoices');
```

### Packages

| Package | Purpose |
|---|---|
| [**quittio**](https://github.com/Quittio/quittio) | Everything in one package: core, Laravel integration and the country pack Germany. Development, issues and pull requests happen here. |
| [core](https://github.com/Quittio/core) | Country-neutral core: EN 16931 data model, calculation, CII/UBL writers and readers, ZUGFeRD/Factur-X PDF/A-3, validation. Usable without Laravel. |
| [laravel](https://github.com/Quittio/laravel) | Laravel integration: facade, configuration, Blade templates, Artisan commands, testing helpers. |
| [country-de](https://github.com/Quittio/country-de) | Country pack for Germany: UStG mandatory fields, tax presets, XRechnung profiles and BR-DE rules, DIN 5008 layouts. |

`core`, `laravel` and `country-de` are read-only splits of the [main repository](https://github.com/Quittio/quittio), updated automatically on every push and tag. Please open issues and pull requests there.

### Formats

| Format | Content |
|---|---|
| ZUGFeRD / Factur-X | PDF/A-3 with embedded CII XML, profiles MINIMUM to EXTENDED and XRECHNUNG |
| XRechnung 3.0 | CII or UBL as plain XML |
| CII, UBL 2.1 | Plain XML in any profile |
| Peppol BIS Billing 3.0 | UBL |
| PDF, HTML, JSON | PDF without XML, HTML preview, normalised data model |

### Status

Version 0.1.0, the first release. APIs may still change until 1.0.0, and the legal wording of the country pack Germany is under review by a tax adviser. A commercial Managed layer (`quittio/managed`: issuer profiles, gapless numbering, archive, cancellations, mail delivery, audit log) is planned and not released yet.

Requires PHP 8.3+ and Laravel 13. The open source packages are MIT-licensed. Security issues: please [report them privately](https://github.com/Quittio/quittio/security/advisories/new).

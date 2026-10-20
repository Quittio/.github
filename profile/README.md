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
  <a href="https://quittio.dev"><b>Website</b></a> ·
  <a href="https://docs.quittio.dev"><b>Documentation</b></a> ·
  <a href="https://docs.quittio.dev/getting-started.html">Getting started</a> ·
  <a href="https://github.com/Quittio/quittio">Main repository</a> ·
  <a href="https://github.com/Quittio/quittio/issues">Issues</a>
</p>

---

- **PHP:** the core runs in any PHP 8.3 application, with Symfony, another framework or none.
- **Laravel:** the integration adds a facade, configuration, Blade templates, queued rendering, mail attachments and Artisan commands.

Everything country-specific (VAT rates, legal texts, national rules) lives in country packs, so the core stays country-neutral. Germany is the first market; packs for Austria, Belgium, Switzerland and France are available as well.

```bash
# PHP, with any framework or none
composer require quittio/core quittio/country-de

# Laravel
composer require quittio/quittio
php artisan quittio:install
```

```php
// PHP
$quittio = Quittio::create([], [new CountryDE]);

$invoice = InvoiceBuilder::make()
    ->number('RE-2026-0042')
    ->deliveryDate('2026-09-30')
    ->seller(Party::make('Muster GmbH')->vatId('DE123456789')->address('Hauptstraße 1', '12345', 'Musterstadt', 'DE'))
    ->buyer(Party::make('Kunde AG')->vatId('DE987654321')->address('Bahnhofstraße 5', '54321', 'Beispielstadt', 'DE'))
    ->line('Web development', netPrice: '95.00', quantity: 24, unit: UnitCode::Hour, vat: VatPreset::Standard)
    ->dueInDays(14)
    ->build();

$quittio->make($invoice)->render()->save(__DIR__.'/invoices/RE-2026-0042.pdf');   // ZUGFeRD: PDF/A-3 with embedded XML

// Laravel: seller and bank account from config/quittio.php
Quittio::invoice()
    ->number('RE-2026-0042')
    ->deliveryDate('2026-09-30')
    ->buyer(Party::make('Kunde AG')->vatId('DE987654321')->address('Bahnhofstraße 5', '54321', 'Beispielstadt', 'DE'))
    ->line('Web development', netPrice: '95.00', quantity: 24, unit: UnitCode::Hour)
    ->dueInDays(14)
    ->render()
    ->save('invoices');
```

The complete examples with imports are in the [main repository](https://github.com/Quittio/quittio#quick-example) and the [documentation](https://docs.quittio.dev/examples.html).

### Packages

| Package | Purpose |
|---|---|
| [**quittio**](https://github.com/Quittio/quittio) | Everything in one package for Laravel applications: core, Laravel integration and the country pack Germany. Development, issues and pull requests happen here. |
| [core](https://github.com/Quittio/core) | Country-neutral core: EN 16931 data model, calculation, CII/UBL writers and readers, ZUGFeRD/Factur-X PDF/A-3, validation. Usable without Laravel. |
| [laravel](https://github.com/Quittio/laravel) | Laravel integration: facade, configuration, Blade templates, Artisan commands, testing helpers. |
| [country-de](https://github.com/Quittio/country-de) | Country pack for Germany: UStG mandatory fields, tax presets, XRechnung profiles and BR-DE rules, DIN 5008 layouts. Usable without Laravel. |
| [country-at](https://github.com/Quittio/country-at) | Country pack for Austria: UStG 1994 mandatory fields, tax presets, CIUS-AT profiles, ebInterface, default interest. Not reviewed by a tax adviser yet. |
| [country-be](https://github.com/Quittio/country-be) | Country pack for Belgium: mandatory fields of Royal Decree No. 1, tax presets, Peppol BIS, language regions, default interest. Not reviewed by a tax adviser yet. |
| [country-ch](https://github.com/Quittio/country-ch) | Country pack for Switzerland: MWSTG requirements, tax presets, QR bill, rounding to 5 Rappen. Not reviewed by a tax adviser yet. |
| [country-fr](https://github.com/Quittio/country-fr) | Country pack for France: mentions of the CGI (CIBS from 2027) and the Code de commerce, the e-invoicing reform, SIREN, late payment penalties. Not reviewed by a tax adviser yet. |

`core`, `laravel` and the `country-*` packs are read-only splits of the [main repository](https://github.com/Quittio/quittio), updated automatically on every push and tag. Please open issues and pull requests there.

### Formats

| Format | Content |
|---|---|
| ZUGFeRD / Factur-X | PDF/A-3 with embedded CII XML, profiles MINIMUM to EXTENDED and XRECHNUNG |
| XRechnung 3.0 | CII or UBL as plain XML, also the XRechnung extension and CVD |
| CII, UBL 2.1 | Plain XML in any profile |
| Peppol BIS Billing 3.0 | UBL |
| PDF, HTML, JSON | PDF without XML, HTML preview, normalised data model |

### Status

Version 0.7.0. APIs may still change until 1.0.0 ([changelog](https://github.com/Quittio/quittio/blob/master/CHANGELOG.md), [roadmap](https://quittio.dev/en/roadmap)). The legal wording, rules and defaults of the country pack Germany were reviewed against the UStG, the UStDV, the UStAE and the BMF letters on e-invoicing and confirmed by a tax adviser in October 2026 ([legal review](https://github.com/Quittio/quittio#legal-review)). The packs for Austria, Belgium, Switzerland and France follow the law and the official sources of October 2026, but have not been reviewed by a tax adviser yet ([documentation](https://docs.quittio.dev/country-packs.html#other-countries)). A commercial Managed layer for Laravel (`quittio/managed`: issuer profiles, gapless numbering, archive, cancellations, mail delivery, audit log) is planned and not released yet ([more](https://quittio.dev/en/managed)).

Requires PHP 8.3+; Laravel 13 only for the Laravel integration. The open source packages are MIT-licensed. Security issues: please [report them privately](https://github.com/Quittio/quittio/security/advisories/new).

<sub>Quittio is software, not tax or legal advice. The review lowers the risk of errors but does not exclude them; whether an invoice meets the legal requirements in the individual case remains the responsibility of the business that issues it. See the [disclaimer](https://github.com/Quittio/quittio#disclaimer).</sub>

#i18n/l10n

---

# Numeronym
- Number based word
- mostly used as a shortend form of a word
- eg.
  - W3C (World Wide Web Concortium)
  - k8s (kubernetes)
  - g11n (globalization)
  - i18n (internationalisation)
  - l10n (localisation)
  - a11y (accessibility)

---
# Why

> Adding i18n early to a project is much easier than afterwards.

---
# Why
- Size difference needs to be taken into account
  - eg. bezahlen -> pay
- RTL vs. LTR
- Adding translate functions afterwards is a big task
  - doing this right away is a no brainer
- easiest translation `const t = (text) => text;`
  - wrap all hardcoded text with t function
  - use IDE to find all references afterwards

----
# i18n definition
> Internationalization is the design and development of a product, application or document content that enables easy localization for target audiences that vary in culture, region, or language. **(W3C)**

----
# i18n
- Process of designing apps which can be localized later
  - use UTF-8 as encoding instead of latin-1
  - handling timestamps accross different timezones

----
# l10n definition
> Localization refers to the adaptation of a product, application or document content to meet the language, cultural and other requirements of a specific target market (a locale). **(W3C)**

----
# l10n
- Localisation might contain:
  - Formating (datetime/numeric/... values)
  - Graphics which might be inappropriate in a given culture
  - Changes due to legal requirements
    - eg. a VAT-ID is required in certain countries on purchase

---
# Things to consider
- Language
- Writing Direction (RTL, LTR)
- Formatting (Date, Time, Currency, ...)
- Relative Time (Days ago, ...)
- Pluralisation
  - some language differenciate between one, few, many

---
# CLDR
- Common locale data repository
- Most extensive repository for locale-specific data
- Developed/Maintained by the Unicode Consortium
- Pretty big 72mb of JSON (xml is bigger)

----
# CLDR
- certain translations
  - language names
  - terretory country names
  - calendric names (weekdays, months,...)
- formatting/parsing numbers, dates
- conversion rules
  - numeral systems (eg. roman -> arabic numbers)
  - calendar systems (eg. julian -> gregorian calendar)
  - spelling numbers as words
- Country infos
  - which day does a week start
  - ...

---
# ICU
- International Components for Unicode
- OpenSource Project for internationalisation
- Uses CLDR internally
- Ported to many environments
  - JS [formatjs](https://formatjs.io)
  - Java
  - C++
  - ...

----
# ICU
- Simple string translations: 'Hello everybody'
- Simple string with placeholder: 'Hello {name}'
- Formatters
  - Number
  - Date
  - Time
  - Select
  - Plural
  - Custom formats eg. currencies

---
# ICU Basic Numbers
- `{ count, number }`
  - en: 21,629,693
  - ca: 21.629.693
  - de: 21629693

----
# ICU Currency Numbers
- `{ count, number, currency }`
  - en: $1,693.10
  - ca: 1.693,10 USD
  - de: 1.693,10 $

- `{ count, number, currency:EUR }`
  - en: €1.693,10
  - ca: 1.693,10 €
  - de: 1.693,10 €

----
# ICU Date

- `{ count, date }`
  - en: May 1, 2019
  - ca: 1 de maig de 2019
  - de: 1. Mai 2019

- `{ count, date, short }`
  - en: 5/1/19
  - ca: 1/5/19
  - de: 1.5.19

----
# ICU Plural
```
{ count, plural,
    =0    {Zero}
    =1    {One}
    other {Many} }
```

----
# ICU Select
```
{ gender, select,
    male {He avoids bugs}
  female {She avoids bugs}
   other {They avoid bugs} }
```

---
# Locale
- Dependent on language AND region
- Language code + country code
  - 'de-AT' === 'deu-AT'
  - 'de-DE' === 'deu-DE'
  - 'en-US'

----
# Why <!-- .element: class="color--white" -->

<!-- .slide: data-background="https://media.giphy.com/media/JSueytO5O29yM/giphy.gif" -->

----
# Why language and region

- Austria vs. Germany
  - Jänner => Januar
  - Erdapfel => Kartoffel
  - Kassa => Kasse
  - https://de.wikipedia.org/wiki/Liste_von_Austriazismen
- England vs. America vs. Canada
  - 01/05/19 => 5/1/19 => 19-05-01

----
# ISO-639 (language codes)
- character code to represent a language
- ISO 639-1
  - two character code, most major languages spoken today
  - more restrictive, not every language is added
- ISO 639-2
  - ISO 639-1 still valid (with exceptions)
  - three character code
  - includes ancient/constructed/historical languages
  - more than 4000 languages

----
# ISO-3166 (country codes)
- character code to represent countries
- ISO-3166-1
  - two character code
  - used for country top level domains
  - this should be used
- ISO 3166-3
  - Used for ancient countries (eg. DDR/UDSSR)

---
# Which locale?
- use system settings
- (use custom user settings)
  - when many people share the same device
- use url parameter (mostly for debugging)

----
# Select system locale
- on the client
  - `navigator.language || navigator.userLanguage`
  - 'de-AT' or 'de-DE'
- on the server
  - via HTTP Accept-Language header

---
# Task
- Add to project:
  - Simple translation
  - Pluralized translation
    - eg. No member in group/many members in group
    - https://angular.io/guide/i18n

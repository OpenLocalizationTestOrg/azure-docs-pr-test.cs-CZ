Odesílatel zásad Framework (SPF) záznamy jsou použité toospecifying serverů e-mailu, které jsou povoleny e-mailu toosend jménem zadaného názvu domény.  Správná konfigurace záznamy SPF je důležité tooprevent příjemce označení e-mailu jako 'nevyžádaná'.

Hello dokumenty RFC DNS původně zavedl nový toosupport typ záznamu, SPF' Tento scénář. toosupport starší názvové servery, budou rovněž povoleno použití hello záznamů toospecify SPF typ záznamu TXT hello.  Tato nejednoznačnost vedla tooconfusion, který se vyřešil tím [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1).  Uvádí, že záznamy SPF musí vytvořit pouze pomocí hello typ záznamu TXT a daného typu záznamu hello SPF je zastaralý.

**Záznamy SPF Azure DNS podporuje a by měl být vytvořen pomocí záznamu typu TXT hello.** Hello zastaralé SPF záznam typ není podporován. Když [import souboru zóny DNS](../articles/dns/dns-import-export.md), se převedou všechny záznamy SPF pomocí záznamu typu hello SPF typ záznamu toohello TXT.

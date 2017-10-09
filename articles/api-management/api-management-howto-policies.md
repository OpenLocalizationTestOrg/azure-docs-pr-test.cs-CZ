---
title: aaaPolicies v Azure API Management | Microsoft Docs
description: "Zjistěte, jak upravit toocreate a ke konfiguraci zásad ve službě API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 9ab0f884a655004cb10c05085034df1795f512e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="policies-in-azure-api-management"></a>Zásady ve službě Azure API Management
Ve službě Azure API Management zásady jsou vynikající funkcí hello systému, který povolí hello vydavatele toochange hello chování hello rozhraní API prostřednictvím konfigurace. Zásady představují kolekci příkazů, které se postupně provádí na hello požadavku nebo odezvy z rozhraní API. Oblíbené příkazy patří převod formátu XML tooJSON a četnosti omezení toorestrict hello množství příchozích volání od vývojáře. Mnoho další zásady jsou k dispozici předinstalované hello.

V tématu hello [referenční informace o zásadách] [ Policy Reference] pro úplný seznam příkazy zásad a jejich nastavení.

Jsou zásady použity uvnitř hello bránu, která je umístěna mezi hello příjemce rozhraní API a hello spravované rozhraní API. Hello gateway přijímá všechny požadavky a obvykle předává je v nezměněném stavu toohello základní rozhraní API. Zásady můžete ale použít změny tooboth hello příchozí žádosti a odchozí odpovědi.

Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných hello zásad služby API Management, pokud hello zásady neurčí jinak. Některé zásady, jako je například hello [řízení toku] [ Control flow] a [nastavená proměnná] [ Set variable] jsou založené na výrazech zásad. Další informace najdete v tématu [pokročilé zásady] [ Advanced policies] a [výrazy zásad][Policy expressions].

## <a name="scopes"></a>Jak tooconfigure zásady
Zásady je možné nakonfigurovat globálně nebo na hello oboru [produktu][Product], [rozhraní API] [ API] nebo [operaci] [Operation]. tooconfigure zásadu, přejděte editor zásad toohello portálu vydavatele hello.

![Zásady nabídky][policies-menu]

editor zásad Hello se skládá z tři hlavní části: hello zásady oboru (nahoře), hello definice zásady kde zásady jsou upravovat (vlevo) a příkazy hello seznamu (vpravo):

![Editor zásad][policies-editor]

toobegin konfigurace zásad, musíte nejdřív vybrat hello oboru, na které hello mají zásady platit. Na snímku obrazovky hello níže hello **Starter** produktu je vybrána. Všimněte si, že hello odmocnina symbol další toohello název zásady vyplývá, že je zásada na této úrovni již použita.

![Rozsah][policies-scope]

Vzhledem k tomu, že již byla použita zásada, konfigurace hello je zobrazena v zobrazení definice hello.

![Konfigurace][policies-configure]

Hello zásada se zobrazí, jen pro čtení na první. V pořadí tooedit hello definice, klikněte na tlačítko hello **konfigurovat zásadu** akce.

![Upravit][policies-edit]

Definice zásad Hello je jednoduchý dokument XML, který popisuje posloupnost příchozí a odchozí příkazy. Hello XML lze upravovat přímo v okně Definice hello. Seznam příkazů je zadaný toohello práva a příkazy toohello použít aktuální obor jsou povolené a zvýrazní; jak je předvedeno podle hello **omezení četnosti volání** příkaz v výše uvedený snímek obrazovky hello.

Kliknutím na povoleno příkaz přidá hello odpovídající XML v umístění hello hello kurzoru v hello Definice zobrazení. 

> [!NOTE]
> Pokud hello zásady, které chcete tooadd není povolena, ujistěte se, abyste byli v hello správný obor pro tuto zásadu. Každý prohlášení o zásadách je určená pro použití v určité obory a zásad oddílů. části zásady hello tooreview a obory pro zásadu, zkontrolujte hello **využití** oddíl pro tuto zásadu v hello [referenční informace o zásadách][Policy Reference].
> 
> 

Úplný seznam příkazy zásad a jejich nastavení jsou k dispozici v hello [referenční informace o zásadách][Policy Reference].

Například tooadd nový příkaz toorestrict příchozí požadavky toospecified IP adresy, umístěte kurzor hello pouze uvnitř hello obsah hello `inbound` XML elementu a klikněte na tlačítko hello **volající omezení IP adres** příkaz.

![Zásady omezení][policies-restrict]

Bude přidáno toohello fragment kódu XML `inbound` element, který obsahuje informace o tom, jak tooconfigure hello příkaz.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

toolimit příchozí požadavky a přijmout pouze ty z IP adresy 1.2.3.4 hello XML upravit takto:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Uložit][policies-save]

Po dokončení konfigurace hello příkazy hello zásady, klikněte na tlačítko **Uložit** a změny hello bude brána pro správu rozhraní API šířený toohello okamžitě.

## <a name="sections"></a>Principy zásad konfigurace
Zásady je řada příkazů, které jsou spouštěny v pořadí pro požadavek a odpověď. Konfigurace Hello je správně rozdělené do `inbound`, `backend`, `outbound`, a `on-error` částech, jak je znázorněno v následující konfiguraci hello.

```xml
<policies>
  <inbound>
    <!-- statements toobe applied toohello request go here -->
  </inbound>
  <backend>
    <!-- statements toobe applied before hello request is forwarded too
         hello backend service go here -->
  </backend>
  <outbound>
    <!-- statements toobe applied toohello response go here -->
  </outbound>
  <on-error>
    <!-- statements toobe applied if there is an error condition go here -->
  </on-error>
</policies> 
```

Pokud dojde k chybě během zpracování hello požadavku, všechny zbývající kroky v hello `inbound`, `backend`, nebo `outbound` oddíly jsou přeskočeny, a provádění skáče toohello příkazy v hello `on-error` části. Tím, že příkazy zásad hello `on-error` části hello chyby můžete zkontrolovat pomocí hello `context.LastError` vlastnost, zkontrolovat a upravit pomocí hello hello chybové odpovědi `set-body` zásady a nakonfigurovat, co se stane, když dojde k chybě. Existují kódy chyb pro vestavěné kroky a chyby, které mohou nastat během zpracování hello příkazy zásad. Další informace najdete v tématu [zpracování chyb v zásady služby API Management](https://msdn.microsoft.com/library/azure/mt629506.aspx).

Vzhledem k tomu, že zásady můžete zadat na různých úrovních (globální, produktu, rozhraní api a operace) hello konfigurace poskytuje způsob, jak toospecify hello pořadí, ve kterém příkazy definice zásady hello provést s ohledem toohello nadřazené zásady pro vás. 

Zásady obory jsou vyhodnocovány v hello následující pořadí.

1. Globální obor
2. Rozsah produktu
3. Rozhraní API oboru
4. Operace oboru

Hello příkazy v nich vyhodnocují podle umístění toohello hello `base` elementu, pokud je k dispozici. Globální zásady má žádné nadřazené zásady a pomocí hello `<base>` element v ní nemá žádný vliv.

Například pokud máte zásadu na globální úrovni hello a zásada nakonfigurovaná pro rozhraní API, pak vždy, když se používá toto rozhraní API konkrétní obě zásady se použijí. API Management umožňuje deterministickou řazení příkazy kombinované zásad prostřednictvím hello base element. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

V definici zásady hello v příkladu výše, hello `cross-domain` příkaz by provést před všechny vyšší zásady, které by naopak následovat hello `find-and-replace` zásad. 

Klikněte na tlačítko toosee hello zásad v aktuálním oboru hello v editoru zásad hello **přepočítat efektivní zásady pro vybraný obor**.

## <a name="next-steps"></a>Další kroky
Podívejte se na následující videa na výrazech zásad.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png

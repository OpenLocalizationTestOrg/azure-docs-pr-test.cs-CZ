---
title: "problémy aaaResolve licence pro skupinu v Azure Active Directory | Microsoft Docs"
description: "Jak tooidentify a vyřešte licence přiřazení problémů, když používáte Azure Active Directory na základě skupiny licencí"
services: active-directory
keywords: "Licencování Azure AD"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ec9c74214a9e246f21fcf767b0bbd586ae702445
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Identifikovat a vyřešit problémy přiřazení licence pro skupinu v Azure Active Directory

Na základě skupiny licencování v Azure Active Directory (Azure AD) zavádí koncept hello uživatelů v licencování chybový stav. V tomto článku vám vysvětlíme hello důvodů, proč se uživatelé muset zvýšit v tomto stavu.

Po přiřazení licence přímo tooindividual uživatele, bez použití správy na základě skupiny licencí, může selhat operace přiřazení hello. Například při spuštění rutiny prostředí PowerShell hello `Set-MsolUserLicense` na uživatele, můžete u z mnoha důvodů, které jsou logiku související toobusiness nezdaří hello rutiny. Například může být ke dostatečný počet licencí nebo konfliktu mezi dvěma plány služby, které nelze přiřadit na hello stejný čas. Hello problém okamžitě údajně tooyou zpět.

Při použití na základě skupiny licencí, hello, které může dojít k chybám stejné, ale dojít hello pozadí při hello služby Azure AD je přiřazování licencí. Z tohoto důvodu hello chyby nelze informovat tooyou okamžitě. Místo toho se zaznamenávají v objektu user hello a pak hlášené prostřednictvím portálu pro správu hello. Všimněte si, že hello původního záměrné toolicense hello uživatele se nikdy ztraceny, ale se zaznamenává v chybovém stavu pro budoucí šetření a překlad.

## <a name="how-toofind-license-assignment-errors"></a>Jak toofind licence přiřazení chyby

1. Uživatelé toofind chybový stav ve specifické skupině, otevřete hello okno pro skupinu hello. V části **licence**, bude oznámení zobrazí, pokud jsou všechny uživatele v chybovém stavu.

![Skupiny, oznámení o chybě](media/active-directory-licensing-group-problem-resolution-azure-portal/group-error-notification.png)

2. Klikněte na tlačítko hello oznámení tooopen seznam všech ovlivněných uživatelů. Kliknutím na každého uživatele zvlášť toosee více podrobností.

![Skupiny, seznam uživatelů v chybovém stavu](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-users-with-errors.png)

3. toofind všech skupin, které obsahují alespoň jednu chybu na hello **Azure Active Directory** okně vyberte **licence** a potom **přehled**. Informace vyberte se zobrazí, pokud některé skupiny vyžadují vaši pozornost.

![Přehled, informace o skupinách v chybovém stavu](media/active-directory-licensing-group-problem-resolution-azure-portal/group-errors-widget.png)

4. Klikněte na pole toosee hello seznam všech skupin s chybami. Můžete kliknout na každou skupinu další podrobnosti.

![Přehled, seznam skupin s chybami](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-groups-with-errors.png)


Níže je uveden popis každý potenciální způsob tooresolve problému a hello ho.

## <a name="not-enough-licenses"></a>Není dostatek licencí

**Problém:** nejsou k dispozici dostatek dostupných licencí pro jednu hello produktů, které je zadána ve skupině hello. Je třeba tooeither Zakupte více licencí produktu hello nebo uvolněte nevyužitých licencí z jiné uživatele nebo skupiny.

toosee kolik licencí je k dispozici, přejděte příliš**Azure Active Directory** > **licence** > **všechny produkty**.

toosee, kteří uživatelé a skupiny spotřebovávají licencí, klikněte na tlačítko produktu. V části **licencované uživatele**, zobrazí se všichni uživatelé, kteří jste měli přiřazen přímo nebo prostřednictvím jedné nebo více skupin licencí. V části **licenci skupiny**, se zobrazí všechny skupiny, které mají přiřazené produktu.

**Prostředí PowerShell:** rutiny prostředí PowerShell, ohlaste tuto chybu jako _CountViolation_.

## <a name="conflicting-service-plans"></a>Konfliktní plány služeb

**Problém:** jeden hello produktů, které je zadána ve skupině hello obsahuje plán služeb, který je v konfliktu s jinou plán služeb, který je už přiřazená toohello uživatele na základě různých produktů. Některé plány služby jsou nakonfigurovány tak, že nemohou být přiděleny toohello stejného uživatele, který jiný plán související služby.

Vezměte v úvahu následující ukázka hello. Uživatel, který má licenci pro Office 365 Enterprise **E1** přímo, má přiřazeny všechny plány hello povolena. Hello uživatel přidaný tooa skupiny, která má hello Office 365 Enterprise **E3** tooit produktu přiřazen. Tento produkt obsahuje plány služby, které se nesmí překrývat s plány hello součástí E1, takže přiřazení licence skupiny hello se nezdaří s hello "Konfliktní služby plány" Chyba. V tomto příkladu jsou plány hello konfliktní služby:

-   SharePoint Online (plán 2) v konfliktu s SharePoint Online (plán 1).
-   Exchange Online (plán 2) je v konfliktu s Exchange Online (plán 1).

toosolve tomuto konfliktu, je nutné toodisable tyto dvě plány buď na hello E1 licence to je přímo přiřazené toohello uživatele. Nebo můžete potřebovat přiřazení toomodify hello celou skupinu licencí a zakázat plány hello v licenci E3 hello. Alternativně můžete rozhodnout licence E1 hello tooremove od uživatele hello Pokud je redundantní v kontextu hello hello E3 licence.

Hello rozhodnutí o tom, jak tooresolve konfliktní licence na produkty, vždy patří toohello správce. Azure AD automaticky nepřekládá konfliktu licencí.

**Prostředí PowerShell:** rutiny prostředí PowerShell, ohlaste tuto chybu jako _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Na této licenci závisí další produkty.

**Problém:** jeden hello produktů, které je zadána ve skupině hello obsahuje plán služeb, který musí být povolen pro jiný plán služby, v jiném produktu, funkce. K této chybě dojde, když se pokusí tooremove hello základní plán služby Azure AD. Například tomu může dojít v důsledku hello uživatele se odebere ze skupiny hello.

toosolve tyto potíže odstranit, budete potřebovat toomake zda vyžaduje plán hello stále přidělený toousers pomocí jiné metody, nebo že hello závislých služeb pro tyto uživatele vypnuté. Po učinit, můžete od těchto uživatelů odebrat správně hello skupiny licencí.

**Prostředí PowerShell:** rutiny prostředí PowerShell, ohlaste tuto chybu jako _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Místo využívání není povolen.

**Problém:** některé služby společnosti Microsoft nejsou k dispozici ve všech umístěních z důvodu místních zákonů a nařízení. Než můžete přiřadit uživatele tooa licence, musíte vlastnost "Využití umístění" hello toospecify pro uživatele hello. Můžete to provést v rámci hello **uživatele** > **profil** > **nastavení** část v hello portálu Azure.

Když se Azure AD pokusí tooassign uživatele skupiny licencí tooa jejichž využití umístění nepodporuje, je se nezdaří a záznam této chyby na hello uživatele.

toosolve tyto potíže odstranit, odeberte uživatele z nepodporované umístění, ze skupiny hello licenci. Případně pokud hodnoty hello aktuální využití umístění není představují hello skutečného uživatelského umístění, můžete upravit je tak, aby licence hello správně přiřazené příštím (Pokud nové umístění hello je podporované).

**Prostředí PowerShell:** rutiny prostředí PowerShell, ohlaste tuto chybu jako _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> Pokud Azure AD přiřadí skupinu licencí, zdědí všechny uživatele bez zadané umístění využití hello umístění adresáře hello. Doporučujeme, aby správci nastavit správné použití hello hodnoty umístění na uživatele před použitím na základě skupiny licencování toocomply s místními zákony a nařízení.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Co se stane, když je více než jednu licenci produktu na skupinu?

Můžete přiřadit více než jedné skupině tooa licence. Například můžete přiřadit Office 365 Enterprise E3 a Enterprise Mobility + Security tooa skupiny tooeasily povolte všechny obsažených služeb pro uživatele.

Azure AD pokusí tooassign všechny licence, které jsou určené v hello skupiny tooeach uživatele. Pokud Azure AD nelze přiřadit jeden z produktů hello kvůli obchodní logiky problémy (například, pokud nejsou k dispozici dostatek licencí pro všechny, nebo pokud dojde ke konfliktu s jinými službami, které jsou povolené uživatele hello), nepřiřadí hello licencí na jiné produkty ve skupině hello buď.

Budete možné toosee hello, uživatelé, kteří se nezdařilo tooget přiřazené se nezdařilo a produktů, které byly ovlivněny tato zaškrtnutí.

## <a name="license-assignment-fails-silently-for-a-user-due-tooduplicate-proxy-addresses-in-exchange-online"></a>Přiřazení licence selže bez upozornění pro uživatele z důvodu tooduplicate adresy proxy serveru v systému Exchange Online

Pokud používáte Exchange Online, někteří uživatelé v klientovi je pravděpodobně nesprávně nakonfigurována s hello stejnou hodnotu adresu proxy serveru. Když se na základě skupiny licencování pokusí tooassign licence toosuch uživatele, se nezdaří a nebude záznam chybu (na rozdíl od v hello ostatních případech chyba popsané výše) – jedná se o omezení ve verzi preview hello této funkce a přidáme tooaddress ho před  *Obecné dostupnosti*.

> [!TIP]
> Pokud si všimnete, že někteří uživatelé nepřijala licenci a se nezobrazí žádná chyba zjištěných na tyto uživatele, nejdřív zkontrolujte, pokud mají duplicitní proxy adres.
> To můžete provést spuštěním následující rutiny prostředí PowerShell pro Exchange Online hello:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> [Tento článek](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online) obsahuje další podrobnosti o tomto problému, včetně informací na [jak tooconnect tooExchange Online pomocí vzdáleného prostředí PowerShell](https://technet.microsoft.com/library/jj984289.aspx).

Po problémy adresu proxy serveru bylo vyřešeno pro hello vliv na uživatele, ujistěte se, že licence tooforce zpracování na toomake skupiny hello se, že licence je nyní možné použít znovu.

## <a name="how-do-you-force-license-processing-in-a-group-tooresolve-errors"></a>Jak můžete vynutit licence zpracování v seskupování tooresolve chyb?

Podle toho, jak jste prováděné tooresolve chyb, může být nutné toomanually aktivační událost zpracování stavu skupiny tooupdate hello uživatele.

Například některé licence se uvolní odebráním přiřazením přímé licencí od uživatelů, budete potřebovat tootrigger zpracování skupiny, které dříve selhaly toofully licence všichni členové uživatele. Otevřete toodo, najít okně skupiny hello **licence**a vyberte hello **znovu zpracovat** tlačítka na panelu nástrojů hello.

## <a name="next-steps"></a>Další kroky

toolearn Další informace o scénáře pro správu licencí pomocí skupin, najdete v části hello následující:

* [Přiřazení licencí tooa skupiny ve službě Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md)
* [Co je na základě skupin licencování v Azure Active Directory?](active-directory-licensing-whatis-azure-portal.md)
* [Jak toomigrate jednotlivé licencované uživatele toogroup základě licencování v Azure Active Directory](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory na základě skupin licencí další scénáře](active-directory-licensing-group-advanced.md)

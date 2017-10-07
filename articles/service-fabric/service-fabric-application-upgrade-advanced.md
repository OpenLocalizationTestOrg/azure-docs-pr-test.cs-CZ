---
title: "aaaAdvanced témata upgradu aplikace | Microsoft Docs"
description: "Tento článek se zabývá některá Pokročilá témata, která se týkají tooupgrading aplikace Service Fabric."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: bdaf3db6209c574d39f57e0bf9951fad5ad1cbec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Upgrade aplikace Service Fabric: advanced témata
## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Přidání nebo odebrání služby během upgradu aplikace
Pokud nové služby je přidali tooan aplikace, která je již nasazen a publikovat jako upgrade, je nová služba hello přidané toohello nasazené aplikace.  Takové upgrade neovlivní žádné hello služeb, které již byly součástí aplikace hello. Však musí být spuštěná instance hello služby, která byla přidána pro hello nové služby toobe active (pomocí hello `New-ServiceFabricService` rutiny).

Služby může být odebrán také z aplikace jako součást upgradu. Ale musí být všechny aktuální instance služby k-be odstraněné hello zastaven před pokračováním upgradu hello (pomocí hello `Remove-ServiceFabricService` rutiny).

## <a name="manual-upgrade-mode"></a>Režim manuálního upgradu
> [!NOTE]
> měli byste zvážit Hello sledována ruční režim pouze pro upgrade selhání nebo pozastavený. režim monitorovaných Hello je, že hello doporučená režimu upgradu pro Service Fabric aplikace.
>
>

Azure Service Fabric nabízí více upgradu režimy toosupport vývoj a produkční clustery. Zvolené možnosti nasazení se může lišit pro různá prostředí.

Hello monitorovat postupného upgradu aplikace je nejčastější upgradu toouse hello hello produkčního prostředí. Při upgradu hello zadat zásady, Service Fabric zajišťuje, že aplikace hello je v pořádku, před provedením upgradu hello.

 Hello aplikace může správce ruční hello vrácení aplikace režimu upgradu toohave celkový kontrolu nad průběh upgradu hello prostřednictvím hello různých domén upgradu. Tento režim je užitečné, když se děje-konvenční upgrade nebo zásad vyhodnocení stavu přizpůsobené nebo komplexní, je požadována (například hello aplikace je již dojít ke ztrátě dat.).

Nakonec hello automatizované postupného upgradu aplikace je užitečné pro vývoj nebo testování prostředí tooprovide rychlé iterace cyklus během vývoje služby.

## <a name="change-toomanual-upgrade-mode"></a>Změna režimu upgradu toomanual
**Ruční**– upgradu aplikace hello zastavit v aktuální UD a změňte hello hello upgrade režimu tooUnmonitored ručně. Hello musí správce volání toomanually **MoveNextApplicationUpgradeDomainAsync** tooproceed s hello upgradovat nebo aktivovat vrácení zpět pomocí inicializace nového upgradu. Po upgradu hello vstoupí v ručním režimu hello, zůstane v režimu ručního hello dokud nového upgradu je zahájeno. Hello **GetApplicationUpgradeProgressAsync** příkaz vrátí FABRIC\_aplikace\_UPGRADE\_stavu\_kolejová\_dál\_čeká na vyřízení.

## <a name="upgrade-with-a-diff-package"></a>Upgrade s balíčkem rozdílů
Aplikace Service Fabric může být upgradována pomocí zřizování s balíčkem aplikace úplné, úplný a samostatný. Aplikace lze také upgradovat pomocí rozdílové balíček, který obsahuje pouze soubory aplikace hello aktualizovat, hello aktualizovala manifest aplikace a soubory manifestu hello služby.

Celou aplikaci balíčku obsahuje všechny potřebné toostart hello soubory a spuštění aplikace Service Fabric. Diff balíček obsahuje pouze hello soubory, které změnily mezi hello poslední zřídit a aktuální upgrade hello plus manifest úplné aplikace hello a hello service manifest soubory. Všechny odkazy v manifestu aplikace hello nebo manifest služby, který nebyl nalezen v sestavení rozložení hello je hledán v hello úložiště bitových kopií.

Celou aplikaci balíčky se vyžadují pro instalaci první hello clusteru toohello aplikace. Následné aktualizace může být balíček celou aplikaci nebo balíček rozdílové.

Situace při pomocí balíčku rozdílové by být vhodné použít:

* Diff balíček je upřednostňované, když máte velké aplikace balíček, který odkazuje na několik souborů manifestu služby nebo několik balíčků kódu, konfigurace balíčky nebo balíčky data.
* Balíček rozdílové jsou upřednostňované, když máte nasazení systém, který generuje hello sestavení rozložení přímo z vašeho procesu sestavení aplikace. V takovém případě sice hello kódu se nezměnila, nově vytvořený sestavení získá různé kontrolního součtu. Pomocí balíčku pro celou aplikaci by vyžadovaly jste tooupdate hello verze na všechny balíčky kódu. Pomocí balíčku rozdílů, pouze poskytnete hello soubory, které změnily a soubory manifestu hello nichž došlo ke změně verze hello.

Při upgradu aplikace pomocí sady Visual Studio hello rozdílové balíček je automaticky publikován. toocreate balíček rozdílové ručně hello manifest aplikace a manifesty hello služby musí být aktualizované, ale jenom balíčky hello změnit by měl být součástí balíčku poslední aplikace hello.

Například začneme hello následující aplikace (čísla verzí zadaná pro snadné pochopení):

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Nyní předpokládejme chtěli tooupdate pouze hello kód balíček service1 pomocí balíčku rozdílové pomocí prostředí PowerShell. Nyní aplikace aktualizovaná má hello následující strukturu složek:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

V takovém případě je aktualizovat too2.0.0 manifestu aplikace hello a hello service manifest pro service1 tooreflect hello kód balíčku aktualizace. Hello složku balíčku aplikace by mít hello strukturu:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Další kroky
[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.

[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.

Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).

Aby aplikace upgradů kompatibilní metodou učení jak toouse [serializace dat](service-fabric-application-upgrade-data-serialization.md).

Řešení běžných potíží v upgradů aplikací tím, že odkazuje toohello kroky v [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).

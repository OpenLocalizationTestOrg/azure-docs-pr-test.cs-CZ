---
title: "aaaAzure vzdálené aplikace RemoteApp – jak šířku pásma sítě a kvalitu prostředí pracovní společně? | Dokumentace Microsoftu"
description: "Zjistěte, jak šířku pásma sítě v Azure RemoteApp může mít vliv na kvalitu vaše uživatelské prostředí."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 74ebc1fb-5187-4056-b08c-0e03b5ecaca6
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 62b0caadf63359eceb63d27fae6ad289b682ff63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp – jak šířku pásma sítě a kvalitu prostředí pracovní společně?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Při hledání v hello [celková šířka pásma sítě,](remoteapp-bandwidth.md) požadováno pro Azure RemoteApp, mějte na paměti hello následující faktory – jsou všechny součástí dynamický systém, ovlivňuje hello celkové uživatelské prostředí. 

* **Dostupná šířka pásma sítě a aktuálním stavu sítě** -sadu parametrů (ztrátě, latenci, kolísání) na hello stejné sítě v daném okamžiku může mít vliv na aplikace hello streamování prostředí, což znamená snížený celkové uživatelské prostředí. ve vaší síti dostupné šířky pásma Hello je funkce zahlcení, náhodné ztrátě, latenci, protože tyto parametry ovlivnit mechanismu řízení zahlcení hello, které v ovládacích prvcích zapnout hello kolizí tooavoid rychlost přenosu.  Například míru ztrát síť nebo síť s vysokou latencí budou hello uživatelské prostředí chybný i v síti s 1 000 MB šířky pásma. ztráta Hello a latence lišit v závislosti na hello počet uživatelů, kteří jsou na hello stejné síti a tyto činnosti uživatelů (například učíte při sledování videa, stahování nebo nahrávání velkých souborů, tisku).
* **Scénáře použití** -hello prostředí závisí na jaké hello uživatelé dělají jako jednotlivce a jako skupiny na hello stejné síti. Například čtení jeden snímek vyžaduje pouze toobe jeden snímek aktualizovat; Pokud uživatel hello skims a posune přes hello obsah textu dokumentu, potřebují vyšší počet snímků toobe aktualizovat za sekundu. Hello komunikace zpět a stanovilo toohello serveru v tomto scénáři nakonec využívat větší šířku pásma sítě. Také zvážit příklad extrémně: více uživatelů jsou učíte při sledování videa vysokým rozlišením (například řešení 4 kB), která uchovává HD konferenční hovory, hraní her 3D video nebo práci v systémech CAD. Všechny tyto může způsobit nepoužitelnost i síť skutečně velkou šířku pásma prakticky.
* **Obrazovky řešení a hello počet obrazovky** -větší šířku pásma sítě je požadovaná toofull aktualizace větší obrazovky než menších obrazovkách. základní technologii Hello nemá poměrně dobrý úlohy kódování a přenos pouze hello oblasti hello obrazovky, které byly aktualizovány, ale jednou za čas, celé úvodní obrazovka musí toobe aktualizovat. Pokud má uživatel hello vyšší rozlišení obrazovky (například řešení 4 kB), že aktualizace vyžaduje větší šířku pásma sítě než obrazovky s nižší rozlišení (např. 1024x768px). Tato stejná logika platí, pokud používáte více než jednu obrazovku pro přesměrování. Šířka pásma musí tooincrease s počtem hello obrazovky.
* **Přesměrování schránky a zařízení** – jedná se o problém velmi zřejmé, ale v mnoha případech Pokud uživatel ukládá velké bloku dat schránky toohello, jak dlouho trvá bit času pro tento tootransfer informace z klienta vzdálené plochy hello toohello serveru. Hello podřízené prostředí může mít vliv hello činnost odesílání obsahu schránky hello proti proudu. Hello totéž platí pro přesměrování zařízení – Pokud skener nebo webová kamera vytváří velké množství dat, který potřebuje toobe odeslané toohello nadřazeného serveru, nebo tiskárnu tooreceive velké dokumentu, nebo která místní úložiště toobe dostupné tooan aplikaci spuštěnou v cloudu toocopy hello velkých souborů, mohou uživatelé všimnout vynechání snímků nebo dočasně "ukotvené" video protože hello data potřebná pro přesměrování zařízení hello roste hello šířku pásma sítě musí. 

Při hodnocení potřeb šířky pásma sítě, ujistěte se, že tooconsider všechny tyto faktory, které fungují jako systém.

Nyní, přejděte zpět toohello [článku šířky pásma sítě hlavní](remoteapp-bandwidth.md), nebo přesunout na tootesting vaše [šířku pásma sítě](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Další informace
* [Odhadnout využití šířky pásma sítě Azure RemoteApp](remoteapp-bandwidth.md)
* [Azure RemoteApp – testování využití šířky pásma sítě s některé běžné scénáře](remoteapp-bandwidthtests.md)
* [Azure RemoteApp šířka pásma sítě – obecné pokyny (Pokud nelze testovat vlastní)](remoteapp-bandwidthguidelines.md)


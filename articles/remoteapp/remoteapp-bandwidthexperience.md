---
title: "Azure RemoteApp – jak šířku pásma sítě a kvalitu prostředí pracovní společně? | Dokumentace Microsoftu"
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
ms.openlocfilehash: 74116902e973fba440b3c662fdf76202d052b4c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp – jak šířku pásma sítě a kvalitu prostředí pracovní společně?
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Při hledání na [celková šířka pásma sítě,](remoteapp-bandwidth.md) požadováno pro Azure RemoteApp, mějte na paměti následující faktory – jsou všechny součástí dynamické systém, který má dopad na celkové činnost koncového uživatele. 

* **Dostupná šířka pásma sítě a aktuálním stavu sítě** -sadu parametrů (ztrátě, latenci, kolísání) ve stejné síti v daném okamžiku může ovlivnit aplikaci streamování prostředí, což znamená snížený celkové činnost koncového uživatele. Ve vaší síti dostupné šířky pásma je funkce zahlcení, náhodné ztrátě, latence, protože všechny tyto parametry mají vliv mechanismu řízení zahlcení, které se v změní ovládací prvky přenosové rychlosti, aby se zabránilo kolizí.  Například míru ztrát síť nebo síť s vysokou latencí bude uživatelské prostředí chybný i v síti s 1 000 MB šířky pásma. Ztráta a latence lišit v závislosti na počtu uživatelů, kteří jsou ve stejné síti a tyto činnosti uživatelů (například učíte při sledování videa, stahování nebo nahrávání velkých souborů, tisku).
* **Scénáře použití** -možnosti závisí na co uživatelé dělají v jako jednotlivce a jako skupinu ve stejné síti. Například čtení jeden snímek vyžaduje jen jeden snímek aktualizovat; Pokud uživatel skims a posune přes obsah textu dokumentu, které potřebují vyšší počet snímků za sekundu aktualizovat. Komunikace a zpět na server v tomto scénáři bude nakonec využívat větší šířku pásma sítě. Také zvážit příklad extrémně: více uživatelů jsou učíte při sledování videa vysokým rozlišením (například řešení 4 kB), která uchovává HD konferenční hovory, hraní her 3D video nebo práci v systémech CAD. Všechny tyto může způsobit nepoužitelnost i síť skutečně velkou šířku pásma prakticky.
* **Obrazovky řešení a počet obrazovky** -větší šířku pásma sítě je potřeba úplné aktualizace obrazovky větší než menších obrazovkách. Základní technologii nemá poměrně dobrý úlohy kódování a přenos pouze ty oblasti obrazovky, které byly aktualizovány, ale jednou za čas, celé obrazovky je třeba aktualizovat. Pokud má uživatel vyšší rozlišení obrazovky (například řešení 4 kB), že aktualizace vyžaduje větší šířku pásma sítě než obrazovky s nižší rozlišení (např. 1024x768px). Tato stejná logika platí, pokud používáte více než jednu obrazovku pro přesměrování. Šířka pásma je potřeba zvýšit počet obrazovky.
* **Přesměrování schránky a zařízení** – jedná se o problém velmi zřejmé, ale v mnoha případech Pokud uživatel ukládá velké bloku dat do schránky, jak dlouho trvá bit času pro tyto informace k přenosu z klienta vzdálené plochy k serveru. Podřízené prostředí může být ovlivněno možností zasílání daného obsahu schránky proti proudu. Totéž platí pro přesměrování zařízení – Pokud skener nebo webové kamery vytváří velké množství dat, který potřebuje k odeslání nadřazeného na server, nebo tiskárnu potřebuje pro příjem velkého dokumentu nebo místní úložiště musí být k dispozici pro spouštění v cloudu pro kopírování velkého souboru aplikace , mohou si uživatelé všimnout vynechání snímků nebo dočasně "ukotvené" video protože data potřebná pro přesměrování zařízení zvětšuje šířku pásma sítě musí. 

Při hodnocení potřeb šířky pásma sítě, ujistěte se, že jste zvážit všechny tyto faktory, které fungují jako systém.

Nyní, přejděte zpět na [článku šířky pásma sítě hlavní](remoteapp-bandwidth.md), nebo přejít k testování vaší [šířku pásma sítě](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Další informace
* [Odhadnout využití šířky pásma sítě Azure RemoteApp](remoteapp-bandwidth.md)
* [Azure RemoteApp – testování využití šířky pásma sítě s některé běžné scénáře](remoteapp-bandwidthtests.md)
* [Azure RemoteApp šířka pásma sítě – obecné pokyny (Pokud nelze testovat vlastní)](remoteapp-bandwidthguidelines.md)


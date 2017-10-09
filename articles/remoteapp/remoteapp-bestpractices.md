---
title: "osvědčené postupy pro vzdálené aplikace RemoteApp aaaAzure | Microsoft Docs"
description: "Osvědčené postupy pro konfiguraci a použití Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b851865b-bec4-4f29-82c0-7b9770c1a520
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: f4d09ef30816eaebb74b69f26f3242c69ea27591
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Osvědčené postupy pro konfiguraci a použití Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://blogs.technet.microsoft.com/enterprisemobility/2016/08/12/application-remoting-and-the-cloud/) podrobnosti.
> 
> 

Hello následující informace vám může pomoct konfigurovat a používat Azure RemoteApp efektivní.

## <a name="connectivity"></a>Připojení
* Vždy používejte nejnovější verzi klienta hello. Použití starší klienty může způsobit problémy s připojením a dalších činnostech sníženou. Povolení automatické aktualizace aplikace pro zařízení se ujistěte se, že tento klient nejnovější hello je vždy nainstalován.
* Vždy používejte hello nejvíce stabilní a spolehlivá internetové připojení k dispozici tooyou.  
* Použití podporuje připojení proxy server jenom pro připojení optimální výkon.  Hello SOCKS proxy není podporována.

## <a name="applications"></a>Aplikace
* Uložte a zavřete aplikace RemoteApp, když jste hotovi s aplikací hello. Není zavření aplikace hello může způsobit ztrátu dat.
* Ověření vlastních aplikací před jejich používání v Azure Remoteappu. To zahrnuje kontrolu jejich fungovat na platformě více relací a nemáte využívat nepotřebné prostředkům, například paměti a procesoru, který může omezují jiného uživatele hello stejné kolekci. Informace, stáhnout a revidovat hello [aplikace kompatibility osvědčené postupy pro služby Vzdálená plocha](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfigurace a Správa
* Zachovat vaší Image šablony až toodate, instalace aktualizace softwaru a další důležité opravy podle potřeby. Tím se zajistí, že jako Azure RemoteApp auto měřítka toomeet kapacitu, každá instance je opravit.  
* Zajistěte, aby že vaše nasazení služby Active Directory Federation Services (AD FS) je zabezpečený a spolehlivý. V opačném případě může selhat ověřování klienta, brání uživatelům v přístupu k Azure RemoteApp.
* Nakonfigurujte Image šablony nainstalovaných aplikací, role nebo funkce tak, aby byly bezstavové. Se neměli spoléhat na všechny instance hello virtuální počítače ve službě Vzdálené aplikace RemoteApp je ve stavu trvalé.
  * Ukládání dat všech uživatelů v uživatelských profilů nebo jiné služby externí toohello umístění úložiště, jako je místní sdílené složky nebo OneDrive.
  * Úložiště sdílených dat v modulu snap-in Služba externí toohello umístění úložiště, jako je místní sdílené složky nebo OneDrive.
  * Nakonfigurujte všechna nastavení systémového v image šablony hello a nikoli na jednotlivé virtuální počítače ve službě.
  * Zakázat automatické aktualizace softwaru pro publikování aplikace – místo toho použít je ručně image šablony toohello a otestovat je před nasazením z šablony hello.


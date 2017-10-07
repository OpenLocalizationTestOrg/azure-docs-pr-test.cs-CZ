---
title: "aaaTroubleshoot brány virtuální sítě Azure a připojení – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak řešit toouse hello sledovací proces sítě Azure rutiny prostředí PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: bd568d34091209390c57d22475559bdb99ad2c59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Řešení potíží s brány virtuální sítě a připojení pomocí prostředí PowerShell sledovací proces sítě Azure

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Sledovací proces sítě obsahuje řadu funkcí, protože se týká toounderstanding síťovým prostředkům v Azure. Jeden z těchto funkcí je prostředek řešení potíží. Řešení potíží s prostředků je možné volat prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API. Při volání, sledovací proces sítě kontroluje stav hello bránu virtuální sítě nebo připojení a vrátí nalezených výsledcích.

## <a name="before-you-begin"></a>Než začnete

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.

Seznam podporovaných brány typy návštěvě [typy podporované brány](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Přehled

Řešení potíží s prostředků poskytuje hello možnosti řešení potíží, které nastat u brány virtuální sítě a připojení. Když je žádosti tooresource řešení potíží s protokoly Probíhá dotaz a prověřovány. Po dokončení kontroly hello výsledky se vrátí. Požadavky na zdroje, řešení problémů s požadavky jsou dlouho spuštěný, což může trvat několik minut tooreturn výsledku. Hello protokoly z řešení potíží se ukládají v kontejneru v účtu úložiště, který je zadán.

## <a name="troubleshoot-a-gateway-or-connection"></a>Řešení potíží s bránu nebo připojení

1. Přejděte toohello [portál Azure](https://portal.azure.com) a klikněte na tlačítko **sítě** > **sledovací proces sítě** > **Diagnostika sítě VPN**
2. Vyberte **předplatné**, **skupiny prostředků**, a **umístění**.
3. Řešení potíží s prostředků vrátí data o stavu hello hello prostředku. Ukládá také účet úložiště tooa relevantní protokoly toobe zkontrolovat. Klikněte na tlačítko **účet úložiště** tooselect účet úložiště.
4. Na hello **účty úložiště** okně vyberte stávající účet úložiště, nebo klikněte na **+ účet úložiště** toocreate novou.
5. Na hello **kontejnery** okně zvolte existující kontejner, nebo klikněte na tlačítko **+ kontejner** toocreate nový kontejner. Po dokončení klikněte na tlačítko **vyberte**
6. Vyberte prostředky tootroubleshoot hello brány a připojení a klikněte na **spustit Poradce při potížích**

Pokud je vybraných víc zdrojů, řešení potíží s běží souběžně na prostředky brány. Nelze spustit na připojení a je přiřazen brány v uzlu hello stejnou dobu. Je doporučeno tootroubleshoot brány nejprve odstraňování problémů s připojením je delší proces. Diagnostika sítě VPN spuštěného na prostředek hello **stav řešení potíží s** sloupec bude zobrazovat stav **systémem**

Po dokončení hello změny stavu sloupec příliš**stavu v pořádku** nebo **není v pořádku**.

![řešení potíží s dokončení][2]

## <a name="understanding-hello-results"></a>Seznámení s výsledky hello

V hello **podrobnosti** části okna hello hello **stav** kartě se zobrazují stav hello hello poslední řešení potíží spuštění na hello vybrané prostředku. Výsledky nejnovější diagnostiky hello jsou uvedené xx minut po hello posledního spuštění.

|Vlastnost  |Popis  |
|---------|---------|
|Prostředek     | Prostředek toohello odkaz.        |
|Cestu k úložišti     |  Cesta toohello účtu úložiště a kontejneru obsahující hello protokolů (Pokud žádné byly vytvořeny během hello spuštění). Toto nastavení není zachována po opuštění hello portálu.        |
|Souhrn     | Shrnutí stavu prostředků hello.        |
|Podrobnosti     | Podrobné informace o stavu prostředků hello.        |
|Poslední spuštění     | Hello hello čas poslední čas řešení potíží se spustil.        |


Hello **akce** karta obsahuje obecné pokyny k jak tooresolve hello problém. Pokud může být akce hello problému, je k dispozici odkaz s další pokyny. V případě hello tam, kde není žádná další pokyny, hello odpovědi poskytuje hello url tooopen případu podpory.  Další informace o vlastnostech hello hello odpovědi a co je součástí najdete v článku [řešení sledovací proces sítě – přehled](network-watcher-troubleshoot-overview.md)


## <a name="next-steps"></a>Další kroky

Pokud nastavení bylo změněno tohoto připojení VPN zastavit, přečtěte si téma [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které může být nejistá.


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png

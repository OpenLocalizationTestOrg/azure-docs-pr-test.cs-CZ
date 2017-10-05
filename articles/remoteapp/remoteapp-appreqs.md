---
title: "Požadavky na aplikaci pro Azure Remoteappu | Microsoft Docs"
description: "Další informace o požadavcích pro aplikace, které chcete použít v Azure Remoteappu"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 4427eef6-288a-49e1-97eb-fee67d99f26a
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 13d42df97ea2b090180f5865a4eac25945f9f34c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="app-requirements"></a>Požadavky aplikace
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp podporuje streamování 32bitové nebo 64bitové aplikace pro systém Windows z bitové kopie systému Windows Server 2012 R2. Většina existující 32bitové nebo 64bitové aplikace pro systém Windows spusťte "tak, jak je v Azure Remoteappu (služby Vzdálená plocha nebo dříve známé jako Terminálové služby) prostředí. Nicméně je rozdíl mezi spuštění a dobře – některé aplikace fungovat správně a provádět i, zatímco jiné nikoli. Následující informace poskytují pokyny pro vývoj aplikací v prostředí vzdálené plochy a testování pro zajištění kompatibility.

Tip: Pracujeme na vytváření příklady pracovní aplikace za vás. Zobrazí se nová témata, které popisují pomocí aplikace Microsoft Access, QuickBooks a App-V v Remoteappu.

## <a name="requirements"></a>Požadavky
Tyto tři požadavky, pokud dodrženy, pomoci aplikace spustit i v RemoteApp:

1. Aplikace, které splňují všechny [požadavky na certifikaci aplikací klasické pracovní plochy Windows](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) a dodržovat [služby Vzdálená plocha programování pokyny](https://msdn.microsoft.com/library/aa383490.aspx) bude mít úplný kompatibilitu s vzdálené aplikace RemoteApp.
2. Aplikace by nikdy ukládat data místně na bitovou kopii nebo vzdálené aplikace RemoteApp instancí, které mohou být ztracena.  Po vytvoření kolekce vzdálené aplikace RemoteApp instance jsou klonovat a jsou bezstavové a smí obsahovat pouze aplikace. Uložit data do externího zdroje nebo v rámci profilu uživatele.
3. Vlastní Image nikdy by mělo obsahovat data, která mohou být ztraceny.  

## <a name="testing-your-apps"></a>Testování aplikace
Použijte následující postup testování aplikací:

1. Instalace systému Windows Server 2012 R2 a aplikace
2. Povolení Vzdálené plochy
3. Vytvořte dva uživatelské účty, uživatele a b, přidání oba uživatelské účty do skupiny zabezpečení vzdálené plochy.
4. Zkontrolujte kompatibilitu s více relace vytvořením dvou souběžných RDS relace počítač při spuštění aplikace.
5. Ověření chování aplikace

## <a name="application-development-guidelines"></a>Pokyny pro vývoj aplikací
Použijte následující pokyny pro vývoj aplikací pro vzdálenou aplikaci RemoteApp.

### <a name="multiple-users"></a>Více uživatelů
* Instalace [aplikace pro jednoho uživatele ](https://msdn.microsoft.com/library/aa380661.aspx)může způsobit problémy v prostředí s více uživateli.
* Aplikace by měla [ukládat informace o uživateli](https://msdn.microsoft.com/library/aa383452.aspx) v umístěních specifický pro uživatele, nezávisle na globální informace, které platí pro všechny uživatele.
* Vzdálené aplikace RemoteApp používá více [obory názvů pro objekty jádra](https://msdn.microsoft.com/library/aa382954.aspx); globální obor názvů se používá hlavně službami v aplikacích klienta nebo serveru.
* Není bezpečné předpokládat, že název počítače nebo [IP adresu](https://msdn.microsoft.com/library/aa382942.aspx) přiřazena k tomuto počítači jsou přidružené jenom jednoho konkrétního uživatele, protože více uživatelů může být současně přihlášení k serveru hostitele relace vzdálené plochy (hostitele relace VP).

### <a name="performance"></a>Výkon
* Zakázat [grafické efekty](https://msdn.microsoft.com/library/aa380822.aspx) před přidáním vaší aplikace do vzdálené aplikace RemoteApp.
* Chcete-li maximalizovat využití procesoru dostupnosti pro všechny uživatele, buď zakažte [úlohy na pozadí ](https://msdn.microsoft.com/library/aa380665.aspx) nebo vytvořit efektivní pozadí úlohy, které nejsou náročná.
* Má ladit a vyvážení aplikace [vláken využití](https://msdn.microsoft.com/library/aa383520.aspx) pro prostředí s více uživateli, s více procesory.
* Za účelem optimalizace výkonu, je vhodné pro aplikace pro [zjistit](https://msdn.microsoft.com/library/aa380798.aspx) tom, zda jsou spuštěny v relaci klienta.


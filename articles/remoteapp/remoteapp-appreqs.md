---
title: "aaaApp požadavky pro Azure Remoteappu | Microsoft Docs"
description: "Další informace o požadavcích hello pro aplikace, které chcete toouse v Azure Remoteappu"
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
ms.openlocfilehash: 3fa2bcdaab457a6fbee8ac52a81d1c4154bbdce1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="app-requirements"></a>Požadavky aplikace
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Azure RemoteApp podporuje streamování 32bitové nebo 64bitové aplikace pro systém Windows z bitové kopie systému Windows Server 2012 R2. Většina existující 32bitové nebo 64bitové aplikace pro systém Windows spusťte "tak, jak je v Azure Remoteappu (služby Vzdálená plocha nebo dříve známé jako Terminálové služby) prostředí. Nicméně je rozdíl mezi spuštění a dobře – některé aplikace fungovat správně a provádět i, zatímco jiné nikoli. Následující informace Hello obsahuje pokyny pro vývoj aplikací v prostředí vzdálené plochy a testování kompatibility tooensure.

Tip: Pracujeme na vytváření příklady pracovní aplikace za vás. Zobrazí se nová témata, které popisují pomocí aplikace Microsoft Access, QuickBooks a App-V v Remoteappu.

## <a name="requirements"></a>Požadavky
Tyto tři požadavky, pokud dodrženy, pomoci aplikace spustit i v RemoteApp:

1. Aplikace, které splňují všechny [požadavky na certifikaci aplikací klasické pracovní plochy Windows](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) a dodržovat příliš[služby Vzdálená plocha programování pokyny](https://msdn.microsoft.com/library/aa383490.aspx) bude mít úplný kompatibilitu s vzdálené aplikace RemoteApp.
2. Aplikace by nikdy ukládat data místně v bitové kopii hello nebo vzdálené aplikace RemoteApp instancí, které mohou být ztracena.  Po vytvoření kolekce vzdálené aplikace RemoteApp hello instance jsou klonovat a jsou bezstavové a smí obsahovat pouze aplikace. Uložit data do externího zdroje nebo v rámci profilu uživatele hello.
3. Vlastní Image nikdy by mělo obsahovat data, která mohou být ztraceny.  

## <a name="testing-your-apps"></a>Testování aplikace
Použijte tyto kroky tootesting aplikace:

1. Instalace systému Windows Server 2012 R2 a aplikace
2. Povolení Vzdálené plochy
3. Vytvořte dva uživatelské účty, uživatele a b, přidání skupiny zabezpečení toohello vzdálené plochy oba uživatelské účty.
4. Zkontrolujte kompatibilitu s více relace vytvořením dvou souběžných RDS relací toohello počítač při spuštění aplikace hello.
5. Ověření chování aplikace

## <a name="application-development-guidelines"></a>Pokyny pro vývoj aplikací
Použijte následující pokyny pro vývoj aplikací pro vzdálenou aplikaci RemoteApp hello.

### <a name="multiple-users"></a>Více uživatelů
* Instalace [aplikace pro jednoho uživatele ](https://msdn.microsoft.com/library/aa380661.aspx)může způsobit problémy v prostředí s více uživateli.
* Aplikace by měla [ukládat informace o uživateli](https://msdn.microsoft.com/library/aa383452.aspx) v umístění specifický pro uživatele, samostatně z globální informací, platí tooall uživatele.
* Vzdálené aplikace RemoteApp používá více [obory názvů pro objekty jádra](https://msdn.microsoft.com/library/aa382954.aspx); globální obor názvů se používá hlavně službami v aplikacích klienta nebo serveru.
* Není bezpečné tooassume, který hello názvu počítače nebo hello [IP adresu](https://msdn.microsoft.com/library/aa382942.aspx) přiřazené toohello počítači jsou přidružené jenom jednoho konkrétního uživatele, protože současně tooa hostitele relace vzdálené plochy (RD relace je přihlášeno více uživatelů Server hostitele).

### <a name="performance"></a>Výkon
* Zakázat [grafické efekty](https://msdn.microsoft.com/library/aa380822.aspx) předtím, než přidáte tooRemoteApp vaší aplikace.
* toomaximize procesoru dostupnosti pro všechny uživatele, buď zakažte [úlohy na pozadí ](https://msdn.microsoft.com/library/aa380665.aspx) nebo vytvořit efektivní pozadí úlohy, které nejsou náročná.
* Má ladit a vyvážení aplikace [vláken využití](https://msdn.microsoft.com/library/aa383520.aspx) pro prostředí s více uživateli, s více procesory.
* toooptimize výkon, je vhodné pro aplikace příliš[zjistit](https://msdn.microsoft.com/library/aa380798.aspx) tom, zda jsou spuštěny v relaci klienta.


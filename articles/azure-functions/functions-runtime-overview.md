---
title: "aaaAzure přehled Runtime Functions | Microsoft Docs"
description: "Přehled hello Azure funkce Runtime Preview"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 8ce3e5037731d499c330b395c89c90109d18d65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-runtime-overview"></a>Přehled Azure Functions modulu Runtime

Hello Azure Functions Runtime nabízí nový způsob je tootake výhod hello jednoduchost a flexibilitu hello Azure Functions programovací model na místě. Založený na hello stejné otevřete zdroj kořeny jako Azure Functions, Azure Functions Runtime je téměř shodná vývojovém prostředí jako cloudová služba hello tooprovide nasadit místně.

![Modul Runtime Azure Functions na portálu Preview][1]

Hello Azure Functions Runtime poskytuje způsob pro vás tooexperience Azure Functions před potvrzením toohello cloudu. Tímto způsobem můžete hello kód prostředky, které vytvoříte potom provedou toohello cloudu při migraci.  modul runtime Hello také otevře nové možnosti pro vás, například pomocí hello k výměně za chodu výpočetní výkon vaší místní počítače toorun batch procesů přes noc. Zařízení můžete použít také v rámci vaší organizace tooconditionally odesílání dat tooother systémů, jak místně a v cloudu hello.

Hello Azure Functions Runtime se skládá ze dvou částí:
* Role správy Runtime Azure Functions
* Role pracovního procesu modulu Runtime Azure Functions

## <a name="azure-functions-management-role"></a>Role správy Azure Functions

Hello Role správy funkce Azure poskytuje pro hello správu funkce místní hostitel. Tato role provádí hello následující úlohy:

* Hostování hello portálu pro správu funkce Azure, což je hello hello stejný jako ten, můžete vidět v hello [portál Azure](https://portal.azure.com). Díky tomu se budete vyvíjet funkcí v hello stejný způsobem jako v hello portálu Azure.
* Distribuci funkcí mezi více pracovníků funkce.
* Poskytování publikování koncový bod, aby mohly publikovat vaše funkce přímo ze sady Microsoft Visual Studio.

## <a name="azure-functions-worker-role"></a>Role pracovního procesu Azure Functions

Role pracovního procesu funkce Azure Hello nasazených v kontejnerech Windows a toto je, kde se provádí funkce kódu.  Můžete nasadit více rolí pracovního procesu ve vaší organizaci a je klíče způsob, ve kterém zákazníci měli používat k výměně za chodu výpočetním výkonu.

## <a name="minimum-requirements"></a>Minimální požadavky

tooget začít s hello Runtime Azure Functions musí mít počítač s **systému Windows Server 2016 nebo Windows 10 Creators Update** s přístup tooa **systému SQL Server** instance.

## <a name="next-steps"></a>Další kroky

Nainstalujte hello [Runtime funkce Azure preview](https://aka.ms/azafr)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png

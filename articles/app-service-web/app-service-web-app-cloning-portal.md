---
title: "Webové aplikace klonování pomocí portálu Azure"
description: "Zjistěte, jak vytvořit kopii vaší webové aplikace do nové webové aplikace pomocí portálu Azure."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 20b0ae4e-67e8-4bae-9d74-8a24dc445cce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2016
ms.author: aelnably
ms.openlocfilehash: 9ebfa91c7972ab3c264032ead8376c23c1197a4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Aplikační služby Azure klonování pomocí portálu Azure
Funkce klonování v [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) umožňuje snadno klonovat existující webové aplikace do nově vytvořené aplikace v jiné oblasti nebo ve stejné oblasti. To vám umožní zákazníkům snadno a rychle nasadit počet aplikací v různých oblastech.

Klonování aplikace je momentálně podporovaná jenom pro plány služby app úrovně premium. Nová funkce používá stejná omezení jako funkci zálohování webové aplikace, najdete v části [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Klonování existující aplikace
Webové aplikace musí být spuštěna v **Premium** režimu v pořadí, můžete naklonoval pro webovou aplikaci.

1. V [portálu Azure](https://portal.azure.com/), otevřete okno vaší webové aplikace.
2. Klikněte na tlačítko **nástroje**. Potom v **nástroje** okně klikněte na tlačítko **klonování aplikace**.
   
    ![][1]
   
   > [!NOTE]
   > Pokud není webová aplikace už v **Premium** režimu, obdržíte zprávu s upozorněním, podporované režimy pro aplikace klonování. Nyní máte možnost vybrat **upgradu**.
   > 
   > 
3. V **klonování aplikace** okně zadejte název nové webové aplikace, skupinu prostředků a plán služby App Service. Také uživatel bude moct vybrat, jestli klonovat počet zdroj nastavení webové aplikace nebo ne.
   
    ![][2]
4. Po kliknutí na **vytvořit** platformou bude začít pracovat na vytvoření kopie zdrojové webové aplikace.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonování stávající aplikace pro služby App Service Environment
V **klonování aplikace** okno zákazníka bude mít možnost Vybrat fond aplikací ve stávající App Service Environment.

## <a name="current-restrictions"></a>Aktuální omezení
Tato funkce je aktuálně ve verzi preview, pracujeme na přidání nové funkce v čase, v následujícím seznamu jsou známé omezení pro aktuální podporu klonování aplikace v portálu Azure:

* Nastavení Azure Traffic Manager nejsou klonovat.
* Nastavení automatického škálování nejsou klonovat.
* Nastavení plánu zálohování nejsou klonovat.
* Nastavení sítě VNET nejsou klonovat.
* Přehled aplikace nejsou automaticky na nastavit cílové webové aplikace
* Nejsou klonovat snadno nastavení ověřování
* Kudu rozšíření nejsou klonovat.
* TiP pravidla nejsou klonovat.
* Nejsou klonovat databáze obsahu

### <a name="references"></a>Odkazy
* [Webové aplikace klonování pomocí prostředí PowerShell](app-service-web-app-cloning.md)
* [Zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md)
* [Způsob vytvoření prostředí App Service Environment](app-service-web-how-to-create-an-app-service-environment.md)
* [Vytvoření webové aplikace v App Service Environment](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Úvod do prostředí App Service](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png

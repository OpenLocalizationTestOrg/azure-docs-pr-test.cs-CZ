---
title: "aaaWeb klonování aplikace pomocí portálu Azure"
description: "Zjistěte, jak tooclone toonew vaší webové aplikace webových aplikací pomocí portálu Azure."
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
ms.openlocfilehash: 605c4879f34d568e9981c34109f9496731c9ed18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Aplikační služby Azure klonování pomocí portálu Azure
funkce v klonování Hello [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) hello umožňuje snadno klonovat existující webové aplikace tooa nově vytvořený aplikace v jiné oblasti nebo ve stejné oblasti. Tím povolíte zákazníci toodeploy počet aplikací v různých oblastech snadno a rychle.

Klonování aplikace je momentálně podporovaná jenom pro plány služby app úrovně premium. nové funkce používá Hello hello stejná omezení jako funkci zálohování webové aplikace, najdete v části [zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Klonování existující aplikace
Hello webové aplikace musí být spuštěna v hello **Premium** režimu, aby toocreate klon pro hello webovou aplikaci.

1. V hello [portálu Azure](https://portal.azure.com/), otevřete okno vaší webové aplikace.
2. Klikněte na tlačítko **nástroje**. Potom v hello **nástroje** okně klikněte na tlačítko **klonování aplikace**.
   
    ![][1]
   
   > [!NOTE]
   > Pokud hello webové aplikace ještě není v hello **Premium** režimu, obdržíte zprávu s upozorněním hello podporované režimy pro aplikace klonování. Nyní máte možnost tooselect hello **upgradu**.
   > 
   > 
3. V hello **klonování aplikace** okně zadejte název nové webové aplikace hello, skupinu prostředků a plán služby App Service. Také hello uživatel se bude moct toochoose jestli tooclone počet zdroj nastavení webové aplikace nebo ne.
   
    ![][2]
4. Po kliknutí na **vytvořit** hello platformy bude začít pracovat na vytvoření klonu hello zdrojové webové aplikace.

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a>Klonování existující aplikace tooan App Service Environment
V hello **klonování aplikace** okno hello zákazníka bude mít možnost toochoose hello fondem aplikací ve stávající App Service Environment.

## <a name="current-restrictions"></a>Aktuální omezení
Tato funkce je aktuálně ve verzi preview, pracujeme tooadd nové funkce v čase, hello následujícím seznamu jsou hello známé omezení podpory aktuální hello klonování aplikace na portálu Azure:

* Nastavení Azure Traffic Manager nejsou klonovat.
* Nastavení automatického škálování nejsou klonovat.
* Nastavení plánu zálohování nejsou klonovat.
* Nastavení sítě VNET nejsou klonovat.
* Přehled aplikace nejsou automaticky na nastavit hello cílové webové aplikace
* Nejsou klonovat snadno nastavení ověřování
* Kudu rozšíření nejsou klonovat.
* TiP pravidla nejsou klonovat.
* Nejsou klonovat databáze obsahu

### <a name="references"></a>Odkazy
* [Webové aplikace klonování pomocí prostředí PowerShell](app-service-web-app-cloning.md)
* [Zálohování webové aplikace ve službě Azure App Service](web-sites-backup.md)
* [Jak tooCreate služby App Service Environment](app-service-web-how-to-create-an-app-service-environment.md)
* [Vytvoření webové aplikace v App Service Environment](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Úvod tooApp Service Environment](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png

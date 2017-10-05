---
title: "Řešení potíží s cloud kolekce vzdálené aplikace RemoteApp – vytvoření | Microsoft Docs"
description: "Zjistěte, jak vyřešit chyby při vytváření cloudové kolekce vzdálené aplikace RemoteApp"
services: remoteapp
documentationcenter: 
author: vkbucha
manager: mbaldwin
ms.assetid: 95eb7797-7b5e-4781-ad23-f15dd85b213d
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 304ba7c5057b27c459bccbb75d3a711567757675
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Řešení potíží s vytvoření cloudové kolekce vzdálené aplikace RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Pokud máte problémy s vytvářením cloudové kolekce, podívejte se na následující informace.

## <a name="your-image-is-invalid"></a>Bitové kopie je neplatný
Pokud při čekají na Azure a zřídit kolekce se zobrazí zpráva podobná "GoldImageInvalid", znamená to, že nesplňuje image šablony [definované požadavky na image](remoteapp-imagereqs.md). Ano, přejděte číst ty [požadavky](remoteapp-imagereqs.md), oprava bitové kopie a pokuste se vytvořit kolekci znovu.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Běžné chyby vidět v portálu pro správu Azure
    DNS server could not be reached
    ProvisioningTimeout

Cloudová kolekce často selhání během vytváření kvůli můžete používají vlastní Image.  Pokud najdete v jednom z výše uvedených chyb a vlastní image se používá k vytvoření této kolekce, zkontrolujte následující akce:

* Zkontrolujte, zda vlastní image, který jste nahráli splňuje požadavky na image.
* Nejčastěji častých problémů je, že obrázek nebyl správně syspreped.  
* Ověřte bitovou kopii můžete spustit v rámci technologie Hyper-V nebo zkuste vytvořit přímo ve vašem předplatném Azure pomocí bitové kopie virtuální počítač IAAS. Pokud virtuální počítač se nepodaří spustit a nespustí, obvykle označuje, že nebyl správně připraven vlastní image.  Zkontrolujte vlastní image byla vytvořena následujících pokynů k vytvoření vlastního image šablony pro vzdálené aplikace RemoteApp

Pokud použijete jednu z imagí Microsoft součástí vašeho předplatného, pokuste se vytvořit kolekci znovu. Pokud potíže potrvají kontaktujte podporu společnosti Microsoft.

    PlatformImageTrialModeOnly

Pokud se zobrazí tato chyba to obvykle znamená, které nebyly upgradovány na placený účet, ale se pokoušíte použít image společnosti Microsoft, který je platný pouze během zkušební režim služby. V takovém případě zkuste vytvoření cloudové kolekce znovu, ale je nutné zadat správné bitové kopie.


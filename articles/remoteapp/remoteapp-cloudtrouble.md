---
title: "aaaTroubleshoot vzdálené aplikace RemoteApp cloudové kolekce – vytvoření | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot vzdálené aplikace RemoteApp cloudové chyby při vytváření kolekce"
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
ms.openlocfilehash: 9484ecbdb048ede8df725215b313e049cc7648f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Řešení potíží s vytvoření cloudové kolekce vzdálené aplikace RemoteApp
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Pokud máte problémy s vytvářením cloudové kolekce, podívejte se na hello následující informace.

## <a name="your-image-is-invalid"></a>Bitové kopie je neplatný
Pokud při čekání Azure tooprovision kolekce se zobrazí zpráva podobná "GoldImageInvalid", znamená to, že image šablony nesplňuje hello [definované požadavky na image](remoteapp-imagereqs.md). Ano, přejděte číst ty [požadavky](remoteapp-imagereqs.md)opravte bitové kopie a akci toocreate kolekci znovu.

## <a name="common-errors-seen-in-hello-azure-management-portal"></a>Běžné chyby vidět v portálu pro správu Azure hello
    DNS server could not be reached
    ProvisioningTimeout

Cloudová kolekce často selhání během vytváření kvůli můžete používají vlastní Image.  Pokud dojde k jedné hello výše chyby a používáte vlastní image toocreate hello kolekce, Zkontrolujte prosím hello následující věci:

* Zajistěte, aby této hello vlastní bitové kopie, který jste nahráli splňuje požadavky na image.
* Nejčastěji hello častých problémů je že této bitové kopie hello nebyla správně syspreped.  
* Ověřte hello image můžete spustit v rámci technologie Hyper-V nebo zkuste vytvořit přímo ve vašem předplatném Azure pomocí bitové kopie hello virtuálních počítačů IAAS. Pokud hello virtuálního počítače selže tooboot a nespustí, pak to obvykle znamená, že této bitové kopie vlastní hello nebyl připraven správně.  Ověřte, zda byl vytvořený vlastní image hello následující hello jak toocreate vlastní šablony obrázků pro vzdálené aplikace RemoteApp

Pokud použijete jednu z imagí Microsoft hello součástí vašeho předplatného, zkuste toocreate hello kolekci znovu. Pokud hello potíže potrvají, kontaktujte podporu společnosti Microsoft.

    PlatformImageTrialModeOnly

Pokud se zobrazí tato chyba obvykle znamená to, byl upgradován tooa placené účet ale pokoušíte toouse bitovou kopii společnosti Microsoft, který je platný pouze během zkušební režim hello hello služby. V takovém případě zkuste toocreate cloudové kolekce znovu, ale být jisti toospecify hello správné bitové kopie.


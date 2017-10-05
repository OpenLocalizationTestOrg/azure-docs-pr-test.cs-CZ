---
title: "Řešení potíží Azure RemoteApp – selhání připojení a spuštění aplikace | Microsoft Docs"
description: "Informace o řešení problémů s spuštění a připojení k aplikacím v Azure Remoteappu."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e5cf7171-d1c2-4053-a38b-5af7821305e1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: fc9d538991adce7fc13e9654b9a7c6d113d03fde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Řešení potíží s Azure RemoteApp - selhání připojení a spuštění aplikace
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Aplikace hostované v Azure Remoteappu může selhat spustit několik různých důvodů. Tento článek popisuje různé příčiny a chybové zprávy uživatelů může dojít při pokusu o spuštění aplikace. Také pojednává o selhání připojení. (Ale tento článek nepopisuje potíže při přihlašování do klientovi Azure Remoteappu.)  

Přečtěte si informace o běžných chybové zprávy z důvodu chyby připojení a spuštění aplikace.

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Nemůžeme se získávání nastavení... Zkuste to znovu za 10 minut.
Tato chyba znamená, že Azure RemoteApp je škálování splňují požadavky na kapacitu uživatelů. Na pozadí se vytváří více instancí virtuálního počítače Azure RemoteApp pro zvládání potřeb kapacity uživatelů. Obvykle to trvá přibližně pět minut ale může trvat až 10 minut. V některých případech to nedojde dostatečně rychle a prostředky jsou potřeba okamžitě. Například 9: 00 situace, kdy je potřeba řada uživatelů používají vaši aplikaci v Azure RemoteAppn ve stejnou dobu. V takovém případě vám můžeme provést aktivaci **kapacity režimu** na back-end. Uděláte to otevřete lístek podpory Azure. Ujistěte se, zahrnout svoje ID předplatného v žádosti.  

![Nemůžeme se zobrazuje při nastavování](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Nelze znovu připojit automaticky do aplikací, znovu spusťte aplikaci
Tato chybová zpráva je často vidět Pokud jste používali Azure RemoteApp a pak přesuňte počítače do režimu spánku déle než 4 hodiny a pak probudila vašeho počítače, a klientovi Azure Remoteappu pokusí automaticky znovu připojit a byl překročen časový limit.  Vyzvat uživatele, aby přejděte zpět do aplikace a pokus o otevření v klientovi Azure Remoteappu.

![Nelze automaticky-znovu připojit k vaší aplikace](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Problémy s dočasnými profily
K této chybě dojde, když profilu uživatele (Disk profilu uživatele) se nepodařilo připojit a uživatel obdrží dočasných profilů.  Správci by měl přejděte do kolekce na portálu Azure a potom přejděte na **relací** kartě a pokusit se **Odhlásit** uživatele. To bude vynutit úplné protokolu z uživatelské relace - pak uživatel pokusí znovu spusťte aplikaci. Pokud to nepomůže, obraťte se na podporu Azure.

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp přestal pracovat.
Tato chybová zpráva znamená problém s klientovi Azure Remoteappu a je třeba restartovat. Vyzvat uživatele, aby zavřete: vyberte **ukončení programu** a poté znovu spusťte klientovi Azure Remoteappu.  Pokud problém nezmizí, otevřete a lístku podpory Azure.

![Azure RemoteApp přestal pracovat.](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Připojení ke vzdálené ploše se přístupu k tomuto prostředku došlo k chybě. Pokus o připojení nebo se obraťte na správce systému
Toto je obecná chyba zpráva – požádejte podporu Azure, takže jsme prozkoumat. 

![Obecná zpráva Azure RemoteApp](./media/remoteapp-apptrouble/ra-apptrouble4.png) 


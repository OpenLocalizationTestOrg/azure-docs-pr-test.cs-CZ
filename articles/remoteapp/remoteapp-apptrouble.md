---
title: "řešení potíží vzdálené aplikace RemoteApp – aaaAzure selhání připojení a spuštění aplikace | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot problémy se spuštěním a připojením tooapplications v Azure Remoteappu."
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
ms.openlocfilehash: e51d480c9d3fa1f2076f95b63c7a8cd2f6956a4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Řešení potíží s Azure RemoteApp - selhání připojení a spuštění aplikace
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Aplikace hostované v Azure Remoteappu může selhat toolaunch několik různých důvodů. Tento článek popisuje různé příčiny a chybové zprávy uživatelů může dojít při pokusu o toolaunch aplikace. Také pojednává o selhání připojení. (Ale tento článek nepopisuje problémy při přihlašování do hello klientovi Azure Remoteappu.)  

Přečtěte si informace o běžných chybové zprávy z důvodu selhání tooapp spuštění a připojení.

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a>Nemůžeme se získávání nastavení... Zkuste to znovu za 10 minut.
Tato chyba znamená, že Azure RemoteApp je vertikálním navýšení kapacity potřebná kapacita hello toomeet uživatelů. Pozadí hello více instancí virtuálního počítače Azure RemoteApp vytváření požadavků na kapacitu hello toohandle uživatelů. Obvykle to trvá přibližně pět minut ale může trvat až too10 minut. V některých případech to nedojde dostatečně rychle a prostředky jsou potřeba okamžitě. Například scénář 9: 00 kde mnoho uživatelů musí toouse aplikaci v Azure RemoteAppn ve hello stejnou dobu. V takovém případě tooyou můžeme provést aktivaci **kapacity režimu** na hello back-end. toodo na tomto otevřete lístek podpory Azure. Být určité tooinclude svoje ID předplatného v žádosti o hello.  

![Nemůžeme se zobrazuje při nastavování](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-tooyour-applications-please-re-launch-your-application"></a>Nelze automatické opětné připojení tooyour aplikace, znovu spusťte aplikaci
Tato chybová zpráva často dochází, pokud byly pomocí Azure Remoteappu a potom se spojí váš počítač toosleep delší než 4 hodiny a pak probudila váš počítač a znovu připojit hello Azure RemoteApp klienta pokus o tooauto a byl překročen časový limit.  Vyzvat uživatele toonavigate back toohello aplikace a pokusit se tooopen z hello klientovi Azure Remoteappu.

![Nelze automatické opětné připojení tooyour aplikace](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-hello-temp-profile"></a>Problémy s dočasnými profily hello
K této chybě dojde, když profilu uživatele (Disk profilu uživatele) se nezdařilo toomount a hello uživatel obdrží dočasných profilů.  Správci by měl přejděte toohello kolekce v hello portál Azure a potom přejděte toohello **relací** kartě a pokusit se příliš**Odhlásit** hello uživatele. To bude vynutit úplné protokolu z relace uživatele hello - potom mít hello uživatelské pokus o toolaunch aplikaci znovu. Pokud to nepomůže, obraťte se na podporu Azure.

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp přestal pracovat.
Tato chybová zpráva znamená klientovi Azure Remoteappu hello má problém a je třeba toobe restartovat. Vyzvat uživatele tooclose: vyberte **ukončení programu** a poté znovu spusťte hello klientovi Azure Remoteappu.  Pokud hello problém potrvá, otevřete a lístku podpory Azure.

![Azure RemoteApp přestal pracovat.](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-hello-connection-or-contact-your-system-administrator"></a>Připojení ke vzdálené ploše se přístupu k tomuto prostředku došlo k chybě. Opakujte pokus o připojení hello nebo se obraťte na správce systému
Toto je obecná chyba zpráva – požádejte podporu Azure, takže jsme prozkoumat. 

![Obecná zpráva Azure RemoteApp](./media/remoteapp-apptrouble/ra-apptrouble4.png) 


---
title: "aaaDeactivate a odstranění zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak zařízení StorSimple tooremove ze služby tak, že nejprve deaktivace služby a poté ji odstranit."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed86bcd089aa957128e14b1709c836d938c131a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a>Deaktivace a odstranění zařízení řady StorSimple 8000 prostřednictvím služby StorSimple Manager
## <a name="overview"></a>Přehled
Můžete tootake zařízení StorSimple mimo provoz (např. Pokud jsou výměna nebo upgrade zařízení nebo pokud už používáte StorSimple). Pokud je to hello případ, budete potřebovat toodeactivate hello zařízení před odstraněním jej. Deaktivace servery hello připojení mezi hello zařízení a služby StorSimple Manager odpovídající hello. Tento kurz vysvětluje, jak tooremove zařízení StorSimple ze služby tak, že nejprve deaktivace služby a poté ji odstranit. 

Po deaktivaci zařízení žádná data, která byla uložená místně na hello zařízení už nebude přístupný. Lze obnovit pouze hello data přidružená k hello zařízení, která byla uložená v cloudu hello.  

> [!WARNING]
> Deaktivace je trvalé a nedá se vrátit zpátky. Deaktivované zařízení nemůže být zaregistrován u služby StorSimple Manager hello Pokud je první resetovat toohello výchozí tovární nastavení. 
> 
> Hello obnovit tovární nastavení proces odstraní všechna data hello, která byla uložená místně na vašem zařízení. Proto je důležité provést cloudový snímek všechna vaše data, před deaktivací zařízení. To vám umožní toorecover všechny hello data v pozdější fázi.
> 
> 

Tento kurz vysvětluje postup:

* Deaktivace zařízení a odstranění dat hello
* Deaktivace zařízení a umožňuje uchovávat hello data

Taky se dozvíte, jak deaktivaci a odstranění funguje na virtuální zařízení StorSimple.

> [!NOTE]
> Před deaktivací fyzický nebo virtuální zařízení StorSimple, ujistěte se, že toostop nebo odstranit klienty a hostitele, které jsou závislé na tomto zařízení.
> 
> 

## <a name="deactivate-and-delete-data"></a>Deaktivace a odstranění dat
Pokud mají zájem o odstraňování zařízení hello úplně a nechcete, aby tooretain hello dat na zařízení hello, dokončete následující kroky hello.

#### <a name="toodeactivate-hello-device-and-delete-hello-data"></a>toodeactivate hello zařízení a odstranění hello dat
1. Předchozí toodeactivating zařízení, je nutné odstranit všechny hello svazku kontejnery (a svazky hello) přidružené hello zařízení. Kontejnery svazků můžete odstranit až poté, co jste odstranili hello přidružené zálohy.
2. Deaktivace zařízení hello následujícím způsobem:
   
   1. Na hello služby StorSimple Manager **zařízení** stránky, vyberte hello zařízení, kterou chcete toodeactivate a, v dolní části hello hello stránky, klikněte na tlačítko **deaktivovat**.
   2. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** toocontinue. Hello deaktivovat proces spustí a toocomplete pár minut trvat.
3. Po deaktivaci můžete odstranit zařízení hello úplně. Odstranění zařízení odebere ze seznamu hello zařízení připojených toohello služby. Služba Hello pak již nebude možné spravovat zařízení hello odstranit. Použijte následující postup toodelete hello zařízení hello:
   
   1. Na hello služby StorSimple Manager **zařízení** vyberte deaktivované zařízení chcete toodelete.
   2. V dolní části hello na stránce hello, klikněte na tlačítko **odstranit**.
   3. Zobrazí se výzva k potvrzení. Klikněte na tlačítko **Ano** toocontinue.
      
      To může trvat několik minut, než toobe hello zařízení odstranit.

## <a name="deactivate-and-retain-data"></a>Deaktivovat a zachovat data
Pokud jsou zajímá odstraňování hello zařízení, ale mají tooretain hello data, dokončete následující kroky hello.

#### <a name="toodeactivate-a-device-and-retain-hello-data"></a>toodeactivate zařízení a umožňuje uchovávat hello data
1. Deaktivujte zařízení hello. Všechny hello kontejnery svazků a snímky hello hello zařízení zůstanou.
   
   1. Na hello služby StorSimple Manager **zařízení** stránky, vyberte hello zařízení, kterou chcete toodeactivate a, v dolní části hello hello stránky, klikněte na tlačítko **deaktivovat**.
   2. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** toocontinue. Hello deaktivovat proces spustí a toocomplete pár minut trvat.
2. Nyní můžete převzít kontejnery svazků hello a hello přidružené snímky. Postupy, přejděte příliš[převzetí služeb při selhání a zotavení po havárii pro zařízení StorSimple](storsimple-device-failover-disaster-recovery.md).
3. Po deaktivaci a převzetí služeb při selhání můžete odstranit zařízení hello úplně. Odstranění zařízení odebere ze seznamu hello zařízení připojených toohello služby. Služba Hello pak již nebude možné spravovat zařízení hello odstranit. Proveďte následující kroky toodelete hello zařízení hello:
   
   1. Na hello služby StorSimple Manager **zařízení** vyberte deaktivované zařízení chcete toodelete.
   2. V dolní části hello na stránce hello, klikněte na tlačítko **odstranit**.
   3. Zobrazí se výzva k potvrzení. Klikněte na tlačítko **Ano** toocontinue.
      
      To může trvat několik minut, než toobe hello zařízení odstranit.

## <a name="deactivate-and-delete-a-virtual-device"></a>Deaktivace a odstranění virtuálního zařízení
Pro virtuální zařízení StorSimple deaktivace zruší přidělení hello virtuálního počítače. Potom můžete odstranit hello virtuální počítač a prostředky hello vytvořené při jeho zřizování. Po deaktivaci virtuálního zařízení hello je nelze obnovit tooits předchozího stavu. 

Deaktivace výsledkem hello následující akce:

* virtuální zařízení StorSimple Hello je odebráno.
* Hello OSDisk a datové disky, které jsou vytvořené pro virtuální zařízení StorSimple hello se odeberou.
* Hello hostované služby a virtuální sítě, které byly vytvořeny při zřizování zůstanou zachovány. Pokud nepoužíváte tyto entity, odstraňte je ručně.
* Cloudové snímky vytvořené virtuální zařízení StorSimple hello zůstanou zachovány.

## <a name="next-steps"></a>Další kroky
* toorestore hello výchozí toofactory deaktivované zařízení, přejděte příliš[resetovat hello zařízení toofactory výchozí nastavení](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* Pro technickou podporu [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md).
* Další informace o toolearn jak toouse hello služby StorSimple Manager, přejděte příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md). 


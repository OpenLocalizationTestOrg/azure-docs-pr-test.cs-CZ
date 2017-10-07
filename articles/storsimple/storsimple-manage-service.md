---
title: "hello aaaDeploy služby StorSimple Manager | Microsoft Docs"
description: "Popisuje, jak toocreate a odstranění hello služby StorSimple Manager v hello portál Azure classic a popisuje, jak toomanage hello registrační klíč služby."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: bc1d5650-275c-42ed-bc77-cdb596f85943
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/14/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f49b647d91b03bb89ebd0e5cce196e50e3c00296
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-storsimple-manager-service-in-hello-azure-classic-portal"></a>Nasazení služby StorSimple Manager hello v hello portál Azure classic

## <a name="overview"></a>Přehled
Hello služby StorSimple Manager běží v Microsoft Azure a připojí zařízení StorSimple toomultiple. Po vytvoření služby hello, můžete ji použít toomanage hello zařízení z hello Microsoft Azure classic portálu spuštěnému v prohlížeči. To vám umožní toomonitor všechna hello zařízení, které jsou připojené toohello StorSimple Manager služby z jedné centrální umístění, a současně minimalizujete její související administrativní zátěže.

Hello StorSimple Manager cílová stránka obsahuje seznam všech služeb StorSimple Manager hello, které můžete použít toomanage zařízení StorSimple úložiště. Pro každou službu StorSimple Manager je hello následující informace uvedené na stránce StorSimple Manager hello:

* **Název** – hello název, který byl přiřazen tooyour služby StorSimple Manager, pokud byla vytvořena. **název služby Hello nelze změnit po vytvoření služby hello. To platí také pro ostatní entity, jako jsou zařízení, svazky, kontejnery svazků a zásady zálohování, které nelze přejmenovat v hello portál Azure classic.**
* **Stav** – hello stav hello služby, který může být **Active**, **vytváření**, nebo **Online**.
* **Umístění** – hello zeměpisné umístění, ve které hello StorSimple zařízení nasadí.
* **Předplatné** – hello fakturace předplatného, které souvisí s vaší služby.

Hello běžné úkoly, které lze provést prostřednictvím stránku hello StorSimple Manager jsou:

* Vytvoření služby
* Odstranění služby
* Získat registrační klíč služby hello
* Znovu vygenerovat registrační klíč služby hello

Tento kurz popisuje, jak tooperform z těchto úloh.

## <a name="create-a-service"></a>Vytvoření služby
Použití hello **rychle vytvořit** možnost toocreate služby StorSimple Manager, pokud chcete toodeploy zařízení StorSimple. toocreate služby, je třeba toohave:

* Předplatné s smlouvu Enterprise Agreement
* Aktivní účet úložiště Microsoft Azure
* fakturační informace, které se používá pro správu přístupu Hello

Můžete také toogenerate výchozí účet úložiště při vytváření služby hello.

Jeden služby můžete spravovat více zařízení. Zařízení, ale nemůžou zahrnovat víc služeb. Velký podnik může mít více instancí toowork služby pomocí různých předplatných, organizace nebo i umístění nasazení. Upozorňujeme, že potřebujete samostatné instance řadu zařízení StorSimple Manager service toomanage StorSimple 8000 a pole virtuální zařízení StorSimple.

> [!IMPORTANT] 
> Pokud máte nepoužívané služby vytvořili (žádné zařízení operace měla provést u tohoto prostředku) předchozí tooAugust 2016, nelze jej spravovat prostřednictvím portálu Azure nebo portál Azure classic. Doporučujeme vytvořit novou službu v hello portálu Azure.

Proveďte následující kroky toocreate služby hello.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Odstranění služby
Před odstraněním služby, ujistěte se, že žádné připojená zařízení ji používají. Pokud služba hello se používá, deaktivujte hello připojené zařízení. Hello deaktivovat operaci severu hello připojení mezi hello zařízení a hello služby, ale zachovat data zařízení hello v cloudu hello.

> [!IMPORTANT] 
> Po odstranění služby hello operace se nedá vrátit. Jakékoli zařízení, které se pomocí služby hello potřebovat toobe tovární nastavení, než se dá použít s jinou službu. V tomto scénáři hello místní data na zařízení hello, jakož i hello konfigurace budou ztraceny.

Proveďte následující kroky toodelete služby hello.

### <a name="toodelete-a-service"></a>toodelete služby
1. Na hello **služby StorSimple Manager** stránky, že si přejete toodelete služby vyberte hello.
2. Klikněte na tlačítko **odstranit** v hello dolní části stránky hello.
3. Klikněte na tlačítko **Ano** v hello potvrzení oznámení. To může trvat několik minut, než toobe služby hello odstranit.

## <a name="get-hello-service-registration-key"></a>Získat registrační klíč služby hello
Po úspěšném vytvoření služby budete potřebovat tooregister zařízení StorSimple službou hello. tooregister prvního zařízení StorSimple, bude nutné hello registrační klíč služby. tooregister další zařízení s existující službu StorSimple, budete potřebovat hello registrační klíč a hello šifrovacího klíče dat služby (který je generován během registrace na první zařízení hello). Další informace o hello šifrovacího klíče dat služby najdete v tématu [zabezpečení zařízení StorSimple](storsimple-security.md). Hello registrační klíč lze získat přístup k **registrační klíč** na hello **služby** stránky.

Proveďte následující kroky tooget hello služby registrační klíč hello.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Registrační klíč služby hello mějte do bezpečného umístění. Budete potřebovat tento klíč, a také šifrovacího klíče dat služby hello, tooregister další zařízení s touto službou. Po získání registračního klíče služby hello, budete potřebovat tooconfigure zařízení prostřednictvím hello Windows Powershellu pro StorSimple rozhraní.

Podrobnosti o tom toouse klíče, najdete v tématu registrace [krok 3: Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-hello-service-registration-key"></a>Znovu vygenerovat registrační klíč služby hello
Je nutné tooregenerate registrační klíč služby, pokud jsou požadované tooperform střídání klíče nebo pokud došlo ke změně hello seznamu správců služeb. Pokud jste znovu vygenerovat klíč hello, hello nový klíč slouží pouze k registraci dalších zařízení. Hello zařízení, které již byly zaregistrovány nejsou ovlivněny tímto procesem.

Proveďte následující kroky tooregenerate registrační klíč služby hello.

### <a name="tooregenerate-hello-service-registration-key"></a>registrační klíč služby hello tooregenerate
1. Na hello **služby StorSimple Manager** klikněte na tlačítko **registrační klíč**.
2. V hello **registrační klíč služby** dialogové okno, klikněte na tlačítko **znovu vygenerovat**.
3. Zobrazí se potvrzovací zpráva. Klikněte na tlačítko **OK** toocontinue s opětovné generování hello.
4. Zobrazí se nový registrační klíč služby.
5. Zkopírujte tento klíč a uložit ho pro registraci nové zařízení s touto službou.
6. Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-manage-service/HCS_CheckIcon.png) tooclose tohoto dialogového okna.

## <a name="next-steps"></a>Další kroky
* Další informace o hello [proces nasazení zařízení StorSimple](storsimple-deployment-walkthrough-u2.md).
* Další informace o [Správa účtu úložiště StorSimple](storsimple-manage-storage-accounts.md).
* Další informace o příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

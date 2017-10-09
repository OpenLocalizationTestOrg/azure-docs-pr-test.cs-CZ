---
title: "aaaUpdate zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak aktualizovat toouse hello StorSimple funkce tooinstall regulární údržby režim aktualizace a opravy hotfix."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: v-sharos
ms.openlocfilehash: 05acf05c8fc89bbb4343f67ad103235bbe3dba0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-storsimple-8000-series-device"></a>Aktualizace zařízení StorSimple řady 8000
## <a name="overview"></a>Přehled
Hello StorSimple aktualizace funkcí umožňují tooeasily aktualizaci zařízení StorSimple. V závislosti na typu hello aktualizace můžete použít aktualizace toohello zařízení prostřednictvím hello portál Azure classic nebo prostřednictvím rozhraní Windows PowerShell hello. Tento kurz popisuje typy aktualizací hello a jak tooinstall každý z nich.

Můžete použít dva typy aktualizace zařízení: 

* Regulární (nebo v normálním režimu) aktualizací
* Aktualizace režimu údržby

Můžete nainstalovat pravidelné aktualizace prostřednictvím hello portál Azure classic nebo prostředí Windows PowerShell; však musíte použít prostředí Windows PowerShell tooinstall údržby režim aktualizace. 

Každý typ aktualizace je popsána samostatně, níže.

### <a name="regular-updates"></a>Pravidelné aktualizace
Pravidelné aktualizace jsou omezovaly aktualizace, které lze nainstalovat, když je zařízení hello v normálním režimu. Tyto aktualizace se uplatňuje skrze hello Microsoft Update webu tooeach zařízení řadiče. 

> [!IMPORTANT]
> Během procesu aktualizace hello může dojít k selhání řadiče. Ale to nebude mít vliv na dostupnost systému nebo operace.
> 
> 

* Podrobnosti o tom, jak tooinstall regular aktualizací prostřednictvím hello portál Azure classic, najdete v části [nainstalovat pravidelné aktualizace prostřednictvím portálu Azure classic hello](#install-regular-updates-via-the-azure-classic-portal).
* Můžete také nainstalovat pravidelné aktualizace pomocí prostředí Windows PowerShell pro StorSimple. Podrobnosti najdete v tématu [instalace pravidelné aktualizace prostřednictvím Windows Powershellu pro StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Aktualizace režimu údržby
Aktualizace režimu údržby jsou rušivý aktualizace, jako je například upgrade firmwaru disku. Tyto aktualizace vyžadují toobe hello zařízení uvést do režimu údržby. Podrobnosti najdete v tématu [krok 2: Zadejte údržby režimu](#step2). Hello Azure classic portálu tooinstall údržby režim aktualizace nejde použít. Místo toho musíte použít Windows PowerShell pro StorSimple. 

Podrobnosti o způsobu režimu údržby tooinstall aktualizace, najdete v části [aktualizace režimu údržby nainstalovat prostřednictvím Windows Powershellu pro StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [!IMPORTANT]
> Režim údržby, které aktualizace musí být použity samostatně tooeach řadiče. 
> 
> 

## <a name="install-regular-updates-via-hello-azure-classic-portal"></a>Nainstalujte pravidelné aktualizace prostřednictvím hello portál Azure classic
Můžete použít zařízení StorSimple tooyour aktualizace Azure classic portálu tooapply hello.

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Instalace pravidelné aktualizace prostřednictvím Windows Powershellu pro StorSimple
Alternativně můžete použít prostředí Windows PowerShell pro StorSimple tooapply regular (normální režim) aktualizace.

> [!IMPORTANT]
> I když můžete nainstalovat pravidelné aktualizace pomocí Windows Powershellu pro StorSimple, důrazně doporučujeme nainstalovat pravidelné aktualizace prostřednictvím hello portál Azure classic. Počínaje Update 1, bude předběžné kontroly aktualizací provádí předchozí tooinstalling z portálu hello. Tyto předběžné kontroly se mají prioritu před selhání a zajistěte hladší prostředí. 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Instalovat aktualizace režimu údržby pomocí prostředí Windows PowerShell pro StorSimple
Pomocí Windows Powershellu pro StorSimple tooapply údržby režim aktualizace tooyour zařízení StorSimple. V tomto režimu jsou pozastavena všechny vstupně-výstupní požadavky. Také se zastaví službám, jako je paměť s náhodným přístupem stálé (paměti NVRAM) nebo hello Clusterová služba. Oba řadiče se restartují, když zadáte nebo ukončit tento režim. Při ukončení tohoto režimu se všechny služby hello bude pokračovat a musí být v pořádku. (To může trvat několik minut.)

Pokud budete potřebovat aktualizace režimu údržby tooapply, obdržíte výstrahu prostřednictvím hello portál Azure classic, abyste měli aktualizace, které musí být nainstalován. Tato výstraha bude obsahovat pokyny pro používání prostředí Windows PowerShell pro StorSimple tooinstall hello aktualizace. Po aktualizaci zařízení hello použijte stejný postup toochange hello zařízení tooRegular režimu. Podrobné pokyny najdete v tématu [krok 4: režim údržby ukončení](#step4).

> [!IMPORTANT]
> * Před přechodem do režimu údržby, ověřte, zda jsou oba řadiče zařízení v pořádku kontrolou hello **stavu hardwaru** na hello **údržby** stránku hello portál Azure classic. Pokud řadič hello není v pořádku, požádejte o další kroky hello Microsoft Support. Další informace přejděte tooContact Microsoft Support. 
> * Pokud jste v režimu údržby, je nutné tooapply hello aktualizace nejdřív na jednom řadiči a pak na hello jiný řadič.
> 
> 

### <a name="step-1-connect-toohello-serial-console-a-namestep1"></a>Krok 1: Připojte toohello konzoly sériového portu<a name="step1">
Nejprve pomocí aplikace, jako je PuTTY tooaccess hello konzoly sériového portu. Hello následující postup vysvětluje, jak toouse PuTTY tooconnect toohello konzoly sériového portu.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Krok 2: Zadejte režimu údržby<a name="step2">
Po připojení konzoly toohello určit, jestli jsou aktualizace tooinstall a zadejte tooinstall režimu údržby je.

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Krok 3: Instalace aktualizace<a name="step3">
Dále nainstalujte vaše aktualizace.

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Krok 4: Režim údržby ukončení<a name="step4">
Nakonec ukončení režimu údržby.

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Instalaci oprav hotfix prostřednictvím Windows Powershellu pro StorSimple
Na rozdíl od aktualizace pro Microsoft Azure StorSimple jsou nainstalované opravy hotfix ze sdílené složky. Jako s aktualizacemi, existují dva typy opravy hotfix: 

* Regulární opravy hotfix 
* Opravy hotfix režimu údržby  

Hello následující postupy popisují, jak toouse Windows Powershellu pro StorSimple tooinstall regulární a opravy hotfix režimu údržby.

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-tooupdates-if-you-perform-a-factory-reset-of-hello-device"></a>Co se stane, že tooupdates Pokud provádíte objekt pro vytváření resetovat hello zařízení?
Pokud se zařízení resetovat toofactory nastavení, všechny hello aktualizace budou ztraceny. Po obnovení továrního nastavení zařízení hello registraci a konfiguraci, je nutné nainstalovat aktualizace toomanually prostřednictvím hello portál Azure classic nebo prostředí Windows PowerShell pro StorSimple. Další informace o obnovení továrního nastavení najdete v tématu [resetovat hello zařízení toofactory výchozí nastavení](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Další kroky
* Další informace o [pomocí Windows Powershellu pro StorSimple tooadminister zařízení StorSimple](storsimple-windows-powershell-administration.md).
* Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).


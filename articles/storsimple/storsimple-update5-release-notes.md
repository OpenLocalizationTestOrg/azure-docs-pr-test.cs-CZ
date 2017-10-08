---
title: "Poznámky k verzi aaaStorSimple 8000 Update 5 řady | Microsoft Docs"
description: "Popisuje nové funkce hello, problémy a řešení pro zařízení StorSimple 8000 řady aktualizací 5."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/23/2017
ms.author: alkohli
ms.openlocfilehash: 9eb8ffb97b41ce3d4f1ffdf2975f904d0a2958e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-5-release-notes"></a>Poznámky k verzi zařízení StorSimple 8000 řady aktualizací 5

## <a name="overview"></a>Přehled

Hello následující poznámky k verzi popisují hello nové funkce a identifikovat hello kritické otevřené problémy pro StorSimple 8000 řady aktualizací 5. Také obsahují seznam aktualizací softwaru zařízení StorSimple hello součástí této verze.

Update 5 může být použité tooany zařízení StorSimple Update 0.1 spuštění prostřednictvím aktualizace 4. Hello zařízení verzi spojenou s aktualizací 5 je 6.3.9600.17845.

Zkontrolujte hello informace obsažené v uvolněte hello poznámky před nasazením hello aktualizovat v řešení StorSimple.

> [!IMPORTANT]
> * Aktualizace 5 má zařízení softwaru, firmwaru disku, zabezpečení operačního systému a další aktualizace operačního systému. Tato aktualizace trvá přibližně tooinstall 4 hodiny. Aktualizace firmwaru disku je rušivý aktualizace a výsledkem výpadku pro vaše zařízení. Doporučujeme použít aktualizaci 5 tookeep aktuální zařízení.
> * Pro nové verze, nemusí zobrazit aktualizace okamžitě, protože provedeme postupné zavádění hello aktualizací. Počkejte několik dní, a pak vyhledejte aktualizace znovu jako tyto aktualizace brzy bude dostupná.

## <a name="whats-new-in-update-5"></a>Co je nového v aktualizací 5

Hello následující klíčových vylepšení a opravy chyb byly provedeny v aktualizací 5.

* **Použití Azure Active Directory (AAD) tooauthenticate službou Správce zařízení StorSimple** – od aktualizace 5 a vyšší, Azure Active Directory je použité tooauthenticate službou hello Správce zařízení StorSimple. 2017 prosinec přestanou Hello staré ověřovací mechanismus. Všichni uživatelé hello musí obsahovat hello nové adresy URL ověřování v jejich pravidla brány firewall. Další informace, přejděte příliš[ověřování adresy URL uvedené v hello sítě požadavky pro zařízení StorSimple](storsimple-8000-system-requirements.md#url-patterns-for-azure-portal).

    Pokud adresa URL pro ověření hello není součástí hello pravidla brány firewall, hello uživatelům se zobrazí kritickou výstrahu, že jejich zařízení StorSimple nelze ověřit službou hello. Pokud uživatelé hello zobrazit tuto výstrahu, potřebují tooinclude hello nové adresy URL ověřování. Další informace, přejděte příliš[StorSimple sítě výstrahy](storsimple-8000-manage-alerts.md#networking-alerts).

* **Nová verze služby StorSimple Snapshot Manager** – vydání nové verze služby StorSimple Snapshot Manager s aktualizací 5. Doporučujeme vám, že aktualizujete toothis verze. Tato verze není kompatibilní s všechna zařízení StorSimple hello, které běží Update 3 nebo novější. Další informace, přejděte příliš[nasazení zařízení StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).


## <a name="issues-fixed-in-update-5"></a>Chyby v aktualizaci 5

Hello následující tabulka obsahuje souhrn problémy, které jsme vyřešili v aktualizací 5.

| Ne | Funkce | Problém | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- |
| 1 |Vzdálená komunikace prostředí Windows PowerShell |V předchozí verzi hello uživatel by k chybě při pokusu o tooestablish toohello vzdáleného připojení zařízení StorSimple cloudu pomocí prostředí Windows PowerShell. Tento problém byl způsobeno kořenové a pevné v této verzi. |Ne |Ano |
| 2 |Šablony šířky pásma |Ve starší verzi se problém s šablony šířky pásma, které za následek menší šířkou pásma, než jaké hello zařízení byla nakonfigurována pro. Tento problém se vyřeší v této verzi. |Ano |Ano |
| 3 |Převzetí služeb při selhání |V předchozí verzi když zařízení s vysoký počet svazků byla při selhání tooanother zařízení se systémem aktualizace 4 hello proces skončí s chybou při pokusu o záznamy řízení přístupu tooapply hello. Tento problém vyřešen v této verzi. |Ano |Ano |



## <a name="known-issues-in-update-5-from-previous-releases"></a>Známé problémy v aktualizaci 5 z předchozích verzí

V aktualizaci 5 nejsou žádné nové známé problémy. Pro seznam problémů, které přenášejí tooUpdate 5 z předchozích verzí, přejděte příliš[poznámky k verzi Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-5"></a>Serial-attached SCSI (SAS) řadiče a aktualizace firmwaru v aktualizací 5

Tato verze obsahuje řadič SAS a LSI ovladače a firmware aktualizace. Další informace o tom, jak tooinstall tyto aktualizace, najdete v části [instalaci aktualizací 5](storsimple-8000-install-update-5.md) zařízení StorSimple.

## <a name="storsimple-cloud-appliance-updates-in-update-5"></a>Aktualizace zařízení StorSimple cloudu v aktualizací 5

Tato aktualizace nemůže být použité toohello cloudu zařízení StorSimple (také označované jako hello virtuální zařízení). Nové zařízení cloudu potřebovat toobe vytvořené pomocí image hello aktualizací 5. Informace o tom toocreate o cloudu zařízení StorSimple, přejděte příliš[nasadit a spravovat zařízení s StorSimple cloudu](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Další krok

Zjistěte, jak příliš[instalaci aktualizací 5](storsimple-8000-install-update-5.md) zařízení StorSimple.


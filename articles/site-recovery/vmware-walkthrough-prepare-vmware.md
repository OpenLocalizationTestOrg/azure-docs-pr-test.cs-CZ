---
title: "aaaPrepare místní prostředky VMware pro replikaci tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje hello kroky, které potřebujete pro replikaci úloh běžících na virtuálních počítačích VMware tooAzure úložiště"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 6aba0e89-ad7c-467e-9db2-cfb3bfe4c7d6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 09d81f15f6ee764135a62f5555e458c55fa30048
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-on-premises-vmware-replication-tooazure"></a>Krok 6: Příprava tooAzure replikace VMware na místě

Použijte hello pokyny v této článku tooprepare místní VMware servery toointeract s Azure Site Recovery a připravit virtuální počítače VMWare pro instalaci služby Mobility hello. agent služby Mobility Hello musí být nainstalován na všech virtuálních počítačích na místě, které chcete tooreplicate tooAzure.

## <a name="prepare-for-automatic-discovery"></a>Příprava pro automatické zjišťování

Site Recovery automaticky vyhledá virtuální počítače spuštěné v hostitelích ESXi vSphere (s nebo bez vCenter server). Pro automatické zjišťování, musí Site Recovery účtu tooaccess hostitelů a serverů:

1. toouse vyhrazený účet, vytvoření role (na úrovni hello vCenter s oprávněními hello popsané v tabulce hello. Pojmenujte ji jako **Azure_Site_Recovery**.
2. Potom vytvořte uživatele na serveru hostitele/vCenter vSphere hello a přiřaďte hello role toohello uživatele. Tento uživatelský účet je zadat během nasazování Site Recovery.


### <a name="vmware-account-permissions"></a>Oprávnění pro uživatelský účet VMware

Site Recovery je potřeba tooVMware přístup pro hello proces serveru tooautomatically zjišťování virtuálních počítačů a pro převzetí služeb při selhání a navrácení služeb po obnovení virtuálních počítačů.

- **Migrace**: Pokud chcete pouze toomigrate tooAzure virtuální počítače VMware, bez někdy je selhání zpět, je použít účet VMware s rolí jen pro čtení. Tyto role můžete spustit převzetí služeb při selhání, ale nelze vypnout chráněných zdrojových počítačů. Tato akce není nezbytné pro migraci.
- **Replikovat nebo obnovit**: Pokud chcete účet hello toodeploy úplná replikace (replikace, převzetí služeb při selhání, navrácení služeb po obnovení) musí být schopný toorun operací, jako je vytváření a odebrání disků, zapínání virtuální počítače atd.
- **Automatické zjišťování**: je požadován alespoň účet jen pro čtení.


**Úkol** | **Požadovaný účet nebo role** | **Oprávnění** | **Podrobnosti**
--- | --- | --- | ---
**Procesový server automaticky vyhledá virtuální počítače VMware** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate tooChild objekt, role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup k objektům hello tooall v datovém centru hello.<br/><br/> toorestrict přístup, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objektu toohello podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).
**Převzetí služeb při selhání** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate tooChild objekt, role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup k objektům hello tooall v datovém centru hello.<br/><br/> toorestrict přístup, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objektu toohello podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).<br/><br/> Tato možnost je užitečná pro účely migrace, ale není úplná replikace, převzetí služeb při selhání a navrácení služeb po obnovení.
**Převzetí služeb při selhání a navrácení služeb po obnovení** | Doporučujeme vytvořit roli (Azure_Site_Recovery) s hello vyžaduje oprávnění a pak mu přiřaďte hello role tooa VMware uživatel nebo skupina | Objekt datového centra –> Propagate tooChild objekt, role = Azure_Site_Recovery<br/><br/> Úložiště dat -> přidělte místo, procházet úložiště dat, operace se soubory nízké úrovně, odstraňte soubor, aktualizovat soubory virtuálního počítače<br/><br/> Síť -> přiřazení sítě<br/><br/> Prostředků -> fondu tooresource přiřazení virtuálního počítače, migraci napájený vypnout virtuální počítač, migrace napájený na virtuálním počítači<br/><br/> Úlohy -> Vytvořit úlohu, úloha aktualizace<br/><br/> Virtuální počítač -> Konfigurace<br/><br/> Virtuální počítač -> interakcí -> odpovědí otázku, připojení zařízení, nakonfigurovat média CD, nakonfigurovat disketová média, vypnout, zapnutí, instalaci nástroje VMware<br/><br/> Virtuální počítač -> inventáře -> vytvořit, registraci, zrušení registrace<br/><br/> Virtuální počítač -> zřizování -> Povolit stahování virtuálního počítače, povolí nahrát soubory virtuálního počítače<br/><br/> Virtuální počítač -> snímky -> odebrat snímky | Uživatel přiřazené úrovni datacenter a má přístup k objektům hello tooall v datovém centru hello.<br/><br/> toorestrict přístup, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objektu toohello podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).


## <a name="prepare-for-push-installation-of-hello-mobility-service"></a>Příprava pro nabízenou instalaci služby Mobility hello

Služba Mobility Hello musí být nainstalován na všech virtuálních počítačích chcete tooreplicate. Existuje několik způsobů tooinstall hello služby, včetně ruční instalace, instalace pomocí metod, jako je například System Center Configuration Manager a nabízené instalace z hello Site Recovery procesový server.

Pokud chcete, aby toouse nabízenou instalaci, je třeba tooprepare účet, pomocí Site Recovery můžete tooaccess hello virtuálních počítačů.

- Můžete použít domény nebo místní účet
- Pro Windows Pokud nepoužíváte účet domény, budete potřebovat toodisable řízení vzdáleného přístupu uživatele v místním počítači hello. toodo, informace v hello zaregistrovat v rámci **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD hello **LocalAccountTokenFilterPolicy**, s hodnotou 1.
- Pokud chcete položku registru hello tooadd pro Windows z rozhraní příkazového řádku, zadejte:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Pro systémy Linux hello účet by měl být kořenový na zdrojovém serveru se Linux hello.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 7: vytvoření trezoru](vmware-walkthrough-create-vault.md)

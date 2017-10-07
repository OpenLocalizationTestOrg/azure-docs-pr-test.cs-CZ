---
title: " Správa serveru VMware vCenter server v Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak přidávat a spravovat v Azure Site Recovery serveru VMware vCenter."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 5be995f137d0c0efaf3050b5366a107098cae15a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-vmware-vcenter-server-in-azure-site-recovery"></a>Správa serveru VMware vCenter Server v Azure Site Recovery
Tento článek popisuje hello různé operace obnovení lokality, které lze provést na VMware vCenter.

## <a name="prerequisites"></a>Požadavky

**Podpora serveru VMware vCenter a VMware vSphere hostitele ESX** | **Podrobnosti** |
|--- | --- |
|**Na místní servery VMware** | Jeden nebo více VMware vSphere serverů, 6.0, 5.5, 5.1 s nejnovější aktualizace. Servery musí nacházet ve stejné sítě jako konfigurační server hello (nebo samostatný procesový server) hello.<br/><br/> Doporučujeme systému vCenter server toomanage hostitele, kde běží 6.0 nebo 5.5 s nejnovějšími aktualizacemi hello. Při nasazení verze 6.0 jsou podporovány pouze funkce, které jsou k dispozici v 5.5.|

## <a name="prepare-an-account-for-automatic-discovery"></a>Příprava účtu pro automatické zjišťování
Site Recovery potřebuje přístup tooVMware pro hello proces serveru tooautomatically zjistit virtuální počítače a pro převzetí služeb při selhání a navrácení služeb po obnovení virtuálních počítačů.

* **Migrace**: Pokud chcete pouze toomigrate tooAzure virtuální počítače VMware, bez někdy je selhání zpět, je použít účet VMware s rolí jen pro čtení. Tyto role můžete spustit převzetí služeb při selhání, ale nelze vypnout chráněných zdrojových počítačů. To není nezbytné pro migraci.
* **Replikovat nebo obnovit**: Pokud chcete účet hello toodeploy úplná replikace (replikace, převzetí služeb při selhání, navrácení služeb po obnovení) musí být schopný toorun operací, jako je vytváření a odebrání disků, zapínání virtuálního počítače.
* **Automatické zjišťování**: je požadován alespoň účet jen pro čtení.


|**Úlohy** | **Požadovaný účet nebo role** | **Oprávnění** | **Podrobnosti**|
|--- | --- | --- | ---|
|**Procesový server automaticky vyhledá virtuální počítače VMware** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate tooChild objekt, role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup k objektům hello tooall v datovém centru hello.<br/><br/> toorestrict přístup, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objekt, toohello podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).|
|**Převzetí služeb při selhání** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate tooChild objekt, role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup k objektům hello tooall v datovém centru hello.<br/><br/> toorestrict přístup, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objektu toohello podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).<br/><br/> Tato možnost je užitečná pro účely migrace, ale není úplná replikace, převzetí služeb při selhání a navrácení služeb po obnovení.|
|**Převzetí služeb při selhání a navrácení služeb po obnovení** | Doporučujeme vytvořit roli (AzureSiteRecoveryRole) s hello vyžaduje oprávnění a pak mu přiřaďte hello role tooa VMware uživatel nebo skupina | Objekt datového centra –> Propagate tooChild objekt, role = AzureSiteRecoveryRole<br/><br/> Úložiště dat -> přidělte místo, procházet úložiště dat, operace se soubory nízké úrovně, odstraňte soubor, aktualizovat soubory virtuálního počítače<br/><br/> Síť -> přiřazení sítě<br/><br/> Prostředků -> fondu tooresource přiřazení virtuálního počítače, migraci napájený vypnout virtuální počítač, migrace napájený na virtuálním počítači<br/><br/> Úlohy -> Vytvořit úlohu, úloha aktualizace<br/><br/> Virtuální počítač -> Konfigurace<br/><br/> Virtuální počítač -> interakcí -> odpovědí otázku, připojení zařízení, nakonfigurovat média CD, nakonfigurovat disketová média, vypnout, zapnutí, instalaci nástroje VMware<br/><br/> Virtuální počítač -> inventáře -> vytvořit, registraci, zrušení registrace<br/><br/> Virtuální počítač -> zřizování -> Povolit stahování virtuálního počítače, povolí nahrát soubory virtuálního počítače<br/><br/> Virtuální počítač -> snímky -> odebrat snímky | Uživatel přiřazené úrovni datacenter a má přístup k objektům hello tooall v datovém centru hello.<br/><br/> toorestrict přístup, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objekt, toohello podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).|

## <a name="create-an-account-tooconnect-toovmware-vcenter-server-vmware-vsphere-exsi-host"></a>Vytvoření tooVMware vCenter tooconnect účet Server nebo VMware vSphere EXSi hostitele
1. Přihlaste se do hello konfigurace serveru a spouštět hello cspsconfigtool.exe pomocí zástupce hello umístit na hello plochy.
2. Klikněte na tlačítko **přidat účet** na hello **spravovat účet** kartě.

  ![Přidat účet](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Zadejte podrobnosti o hello účtu a klikněte na tlačítko OK tooadd hello účet. Hello účet by měl mít oprávnění hello uvedené v hello [Příprava účet pro automatické zjišťování](#prepare-an-account-for-automatic-discovery) části.

  >[!NOTE]
  Jak dlouho trvá asi 15 minut pro hello účet informace toobe synchronizovat se službou Site Recovery hello.


## <a name="associate-a-vmware-vcenter-vmware-vsphere-esx-host-add-vcenter"></a>Přidružení VMware vCenter / VMware vSphere ESX host (Přidat vCenter)
* Na portálu Azure text hello, procházet příliš*YourRecoveryServicesVault* > **infrastruktura Site Recovery** > **servery konfigurace**  >  *ConfigurationServer*
* Na stránce s podrobnostmi o hello konfigurační server klikněte na hello + vCenter tlačítko.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials-used-tooconnect-toohello-vcenter-server-vsphere-esxi-host"></a>Upravit přihlašovací údaje používané tooconnect toohello vCenter server nebo hostiteli ESXi vSphere

1. Přihlášení do hello konfigurace serveru a spouštět hello cspsconfigtool.exe
2. Klikněte na tlačítko **přidat účet** na hello **spravovat účet** kartě.

  ![Přidat účet](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Zadejte hello nové podrobnosti účtu a klikněte na tlačítko OK tooadd hello účtu. Hello účet by měl mít oprávnění hello uvedené v hello [Příprava účet pro automatické zjišťování](#prepare-an-account-for-automatic-discovery) části.
4. Na portálu Azure text hello, procházet příliš*YourRecoveryServicesVault* > **infrastruktura Site Recovery** > **servery konfigurace**  >  *ConfigurationServer*
5. Na stránce s podrobnostmi o hello konfigurační server klikněte na hello **aktualizace serveru** tlačítko.
6. Po dokončení úlohy serveru aktualizace hello vyberte hello vCenter Server tooopen hello vCenter souhrnná stránka.
7. Vyberte hello nově přidali účet v hello **účet hostitele server vSphere vCenter** pole a klikněte na tlačítko hello **Uložit** tlačítko.

  ![Upravit účet](./media/site-recovery-vmware-to-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-in-azure-site-recovery"></a>Odstraňte vCenter v Azure Site Recovery
1. Na portálu Azure text hello, procházet příliš*YourRecoveryServicesVault* > **infrastruktura Site Recovery** > **servery konfigurace**  >  *ConfigurationServer*
2. Na stránce Podrobnosti hello konfigurační server vyberte hello vCenter Server tooopen hello vCenter souhrnná stránka.
3. Klikněte na hello **odstranit** tlačítko toodelete hello vCenter

  ![Odstranění účtu](./media/site-recovery-vmware-to-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Pokud budete potřebovat toomodify hello Vcenter IP adresu nebo plně kvalifikovaný název domény, podrobnosti o portu, potom můžete potřebovat toodelete hello vCenter Server a přidejte ji zpět znovu.

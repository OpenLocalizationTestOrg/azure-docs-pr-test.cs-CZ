---
title: "aaaInstall služba Mobility (VMware nebo fyzických tooAzure) | Microsoft Docs"
description: "Zjistěte, jak tooinstall hello tooprotect agenta služby Mobility místní počítače."
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
ms.openlocfilehash: f7836e6b35d3838bae1eff927838ce4b245b9f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-mobility-service-vmware-or-physical-tooazure"></a>Instalaci služby Mobility (VMware nebo fyzických tooAzure)
Služba Mobility Azure Site Recovery zaznamenává datové zápisy na počítači a předává je toohello procesový server. Nasaďte službu Mobility počítače tooevery (virtuálního počítače VMware nebo fyzických serverů), které chcete tooreplicate tooAzure. Můžete nasadit službu Mobility toohello serverů, které má tooprotect pomocí hello následující metody:


* [Instalace služby Mobility pomocí nástroje pro nasazení softwaru jako je System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
* [Instalace služby Mobility pomocí Azure Automation a konfigurace požadovaného stavu (DSC automatizace)](site-recovery-automate-mobility-service-install.md)
* [Nainstalujte službu Mobility ručně pomocí hello grafické uživatelské rozhraní (GUI)](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui)
* [Ruční instalace služby Mobility na příkazovém řádku](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt)
* [Instalaci služby Mobility pomocí nabízené instalace z Azure Site Recovery](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery)


>[!IMPORTANT]
> Počínaje verzí 9.7.0.0, na virtuální počítače (VM) s Windows hello služba Mobility instalační program nainstaluje také hello nejnovější dostupné [agenta virtuálního počítače Azure](../virtual-machines/windows/extensions-features.md#azure-vm-agent). Když se počítač nepovede přes tooAzure, hello počítač splňuje instalace agenta hello požadovaných pro používání všech rozšíření virtuálního počítače.

## <a name="prerequisites"></a>Požadavky
Než nainstalujete službu Mobility ručně na vašem serveru, proveďte následující požadované kroky:
1. Přihlaste se tooyour konfigurační server a pak otevřete okno příkazového řádku jako správce.
2. Změnit složku Koš toohello hello a pak vytvořte soubor přístupové heslo:

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Uložte soubor hello přístupové heslo v zabezpečeném umístění. Použijete soubor hello během hello instalace služby Mobility.
4. Instalační služba mobility pro všechny podporované operační systémy jsou ve složce %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository hello.

### <a name="mobility-service-installer-to-operating-system-mapping"></a>Mapování mobility služby Instalační služby operačního systému

| Název šablony souboru instalačního programu| Operační systém |
|---|--|
|Microsoft automatické obnovení systému\_uživatelský Agent\*Windows\*release.exe | Windows Server 2008 R2 SP1 (64 bitů) </br> Windows Server 2012 (64 bitů) </br> Windows Server 2012 R2 (64 bitů) |
|Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL6 64*release.tar.gz| Red Hat Enterprise Linux (RHEL) 6.4, 6.5, 6.6, 6.7, 6.8 (pouze 64bitové) </br> CentOS 6.4, 6.5, 6.6, 6.7, 6.8 (pouze 64bitové) |
|Microsoft automatické obnovení systému\_uživatelský Agent\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.1, 7.2 (pouze 64bitové) </br> CentOS 7.0, 7.1, 7.2 (pouze 64bitové)</br> CentOs 7.3 (pouze migrace) |
|Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (pouze 64bitové)|
|Microsoft automatické obnovení systému\_uživatelský Agent\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 (pouze 64bitové)|
|Microsoft automatické obnovení systému\_uživatelský Agent\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (pouze 64bitové)|
|Microsoft automatické obnovení systému\_uživatelský Agent\*UBUNTU 14.04-64\*release.tar.gz | Ubuntu Linux 14.04 (pouze 64bitové)|


## <a name="install-mobility-service-manually-by-using-hello-gui"></a>Nainstalujte službu Mobility ručně pomocí grafického uživatelského rozhraní hello

>[!IMPORTANT]
> Pokud používáte **konfigurační Server** tooreplicate **virtuální počítače Azure IaaS** z jednoho předplatného Azure nebo oblast tooanother pak **pomocí instalace z příkazového řádku založené na hello**  – metoda

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## <a name="install-mobility-service-manually-at-a-command-prompt"></a>Ruční instalace služby Mobility na příkazovém řádku

### <a name="command-line-installation-on-a-windows-computer"></a>Instalace z příkazového řádku na počítači se systémem Windows
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### <a name="command-line-installation-on-a-linux-computer"></a>Instalace z příkazového řádku na počítač se systémem Linux
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## <a name="install-mobility-service-by-push-installation-from-azure-site-recovery"></a>Instalaci služby Mobility pomocí nabízené instalace z Azure Site Recovery
toodo nabízenou instalaci služby Mobility pomocí Site Recovery, všechny cílové počítače musí splňovat následující požadavky hello.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


> [!NOTE]
Po instalaci služby Mobility, v hello portál Azure, vyberte hello **replikovat** tlačítko toostart ochranu těchto virtuálních počítačů.

## <a name="uninstall-mobility-service-on-a-windows-server-computer"></a>Odinstalujte službu Mobility na počítači s Windows serverem
Použijte jeden z následujících metod toouninstall služba Mobility na počítači s Windows serverem hello.

### <a name="uninstall-by-using-hello-gui"></a>Odinstalovat pomocí grafického uživatelského rozhraní hello
1. V Ovládacích panelech vyberte **programy**.
2. Vyberte **Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server**a potom vyberte **odinstalovat**.

### <a name="uninstall-at-a-command-prompt"></a>Odinstalujte na příkazovém řádku
1. Otevřete okno příkazového řádku jako správce.
2. toouninstall služby Mobility, spusťte následující příkaz hello:

```
MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
```

## <a name="uninstall-mobility-service-on-a-linux-computer"></a>Odinstalujte službu Mobility na počítač se systémem Linux
1. Na serveru systému Linux, přihlaste se jako **kořenové** uživatele.
2. V terminálu přejděte příliš/uživatel/místní nebo automatické obnovení systému.
3. toouninstall služby Mobility, spusťte následující příkaz hello:

```
uninstall.sh -Y
```

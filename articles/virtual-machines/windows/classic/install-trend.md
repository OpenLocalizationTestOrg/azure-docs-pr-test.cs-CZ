---
title: "aaaInstall Trend Micro hluboké Security na virtuálním počítači | Microsoft Docs"
description: "Tento článek popisuje, jak tooinstall a konfigurace Trend Micro zabezpečení na virtuálním počítači vytvořené pomocí modelu nasazení Classic hello v Azure."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: iainfou
ms.openlocfilehash: dc5492db07a37a2296df5da673183a14c6d5b1f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Jak tooinstall a nakonfigurujte Trend Micro hluboké Security jako služba na virtuální počítač s Windows
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Tento článek ukazuje, jak tooinstall a nakonfigurujte Trend Micro hluboké Security jako služba na nový nebo existující virtuální počítač (VM) s Windows serverem. Hloubkové zabezpečení jako služba zahrnuje ochrana proti malwaru, brána firewall, narušení prevence systém a monitorování integrity.

Hello klienta se instaluje jako rozšíření zabezpečení prostřednictvím hello agenta virtuálního počítače. Na nový virtuální počítač nainstalujte hello hloubkové Agent zabezpečení, jako hello agenta virtuálního počítače je vytvářena automaticky nástrojem hello portálu Azure.

Existující virtuální počítač vytvořený pomocí portálu classic hello, hello rozhraní příkazového řádku Azure nebo PowerShell nemusí mít agenta virtuálního počítače. Pro stávajícího virtuálního počítače, který nemá hello agenta virtuálního počítače třeba toodownload a nainstalujte ji jako první. Tento článek se týká obou situacích.

Pokud máte z Trend Micro aktuální předplatné pro místní řešení, můžete ho toohelp chránit virtuální počítače Azure. Pokud vám ještě nejsou zákazníka, můžete zaregistrovat zkušební předplatné. Další informace o tomto řešení najdete v příspěvku blogu Trend Micro hello [Microsoft Azure virtuálního počítače agenta rozšíření pro přímý zabezpečení](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-hello-deep-security-agent-on-a-new-vm"></a>Nainstalujte hello hluboké Security Agent na nový virtuální počítač

Hello [portál Azure](http://portal.azure.com) umožňuje nainstalovat rozšíření zabezpečení Trend Micro hello při použití bitové kopie z hello **Marketplace** toocreate hello virtuálního počítače. Pokud vytváříte jeden virtuální počítač, pomocí portálu hello je snadný způsob tooadd ochrany z Trend Micro.

Pomocí položky z hello **Marketplace** otevře průvodce, který vám pomůže nastavit hello virtuálního počítače. Použít hello **nastavení** okna, panel třetí hello hello průvodci tooinstall hello Trend Micro rozšíření zabezpečení.  Obecné pokyny najdete v tématu [vytvoření virtuálního počítače se systémem Windows v portálu Azure hello](tutorial.md).

Když získáte toohello **nastavení** okno Průvodce hello hello následující kroky:

1. Klikněte na tlačítko **rozšíření**, pak klikněte na tlačítko **přidat rozšíření** v podokně Další hello.

   ![Začněte přidávat hello rozšíření][1]

2. Vyberte **hloubkové Agent zabezpečení** v hello **nový prostředek** podokně. V podokně hello hloubkové Agent zabezpečení, klikněte na **vytvořit**.

   ![Identifikovat Agent hloubkové zabezpečení][2]

3. Zadejte hello **identifikátor klienta** a **heslo aktivace klienta** hello rozšíření. Volitelně můžete zadat **identifikátor zásady zabezpečení**. Potom klikněte na **OK** tooadd hello klienta.

   ![Zadejte podrobnosti o rozšíření][3]

## <a name="install-hello-deep-security-agent-on-an-existing-vm"></a>Nainstalujte hello hluboké Security Agent na existující virtuální počítač
agent hello tooinstall na existující virtuální počítač, je třeba hello následující položky:

* modul Azure PowerShell text Hello, verze 0.8.2 nebo novější, nainstalovaná v místním počítači. Můžete zkontrolovat hello verzi prostředí Azure PowerShell, který jste nainstalovali pomocí hello **Get-Module azure | verze formátu tabulky** příkaz. Pokyny a odkaz nejnovější verzi toohello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). Přihlaste se pomocí předplatného Azure tooyour `Add-AzureAccount`.
* Hello Agent virtuálního počítače nainstalovaný na hello cílového virtuálního počítače.

Nejprve ověří, že hello agenta virtuálního počítače je již nainstalována. Zadejte název hello cloudové služby a název virtuálního počítače a pak spusťte následující příkazy příkazového řádku Azure PowerShell úrovni správce hello. Nahraďte vše v rámci hello uvozovky, včetně hello < a > znaků.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Pokud si nejste jisti hello Cloudová služba a název virtuálního počítače, spusťte **Get-AzureVM** toodisplay že hello informace pro všechny virtuální počítače v aktuálním předplatném.

Pokud hello **zápisu hostitele** příkaz vrátí **True**, hello je nainstalovaný Agent virtuálního počítače. Vrátí-li **False**, najdete v části hello pokyny a odkaz toohello, stáhněte si v hello Azure příspěvku na blogu [agenta virtuálního počítače a rozšíření – část 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Pokud je nainstalovaná hello agenta virtuálního počítače, spusťte tyto příkazy.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Další kroky
Jak dlouho trvá několik minut, než toostart hello agent spuštěn, když je nainstalovaná. Potom musíte tooactivate hloubkové zabezpečení na virtuálním počítači hello tak můžete spravovat pomocí hloubkové zabezpečení správce. Viz následující články pro další pokyny hello:

* Trend na článek o tomto řešení [Instant-On cloudu zabezpečení pro Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
* A [ukázkový skript prostředí Windows PowerShell](http://go.microsoft.com/fwlink/?LinkId=404100) tooconfigure hello virtuálního počítače
* [Pokyny](http://go.microsoft.com/fwlink/?LinkId=404099) pro ukázku hello

## <a name="additional-resources"></a>Další zdroje
[Jak toolog na tooa virtuální počítač se systémem Windows Server]

[Rozšíření virtuálního počítače Azure a funkce]

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Jak toolog na tooa virtuální počítač se systémem Windows Server]:connect-logon.md
[Rozšíření virtuálního počítače Azure a funkce]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409

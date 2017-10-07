---
title: "aaaInstall Symantec Endpoint Protection na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall a nakonfigurovat rozšíření zabezpečení hello Symantec Endpoint Protection na nový nebo existující virtuální počítač Azure vytvořené pomocí modelu nasazení Classic hello."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: iainfou
ms.openlocfilehash: cb6083366185c26c9f43ff94d835cd90725de5b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Jak tooinstall a konfigurace Symantec Endpoint Protection na virtuální počítač s Windows
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Tento článek ukazuje, jak tooinstall a nakonfigurovat klienta hello Symantec Endpoint Protection na existující virtuální počítač (VM) s Windows serverem. Tato úplná klienta zahrnuje služby jako virů a spywaru ochrany, brány firewall a prevence vniknutí. Hello klienta se instaluje jako rozšíření zabezpečení pomocí hello agenta virtuálního počítače.

Pokud máte stávající předplatné od společnosti Symantec pro místní řešení, můžete ho tooprotect virtuální počítače Azure. Pokud vám ještě nejsou zákazníka, můžete zaregistrovat zkušební předplatné. Další informace o tomto řešení najdete v tématu [Symantec Endpoint Protection na platformě Azure společnosti Microsoft][Symantec]. Tato stránka také obsahuje odkazy toolicensing informace a pokyny pro instalaci klienta hello, pokud jste již zákazník Symantec.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Nainstalujte Symantec Endpoint Protection na existující virtuální počítač
Než začnete, je třeba hello následující:

* modul Azure PowerShell text Hello, verze 0.8.2 nebo novější, na počítači v práci. Můžete zkontrolovat hello verzi prostředí Azure PowerShell, který jste nainstalovali s hello **Get-Module azure | verze formátu tabulky** příkaz. Pokyny a odkaz nejnovější verzi toohello najdete v tématu [jak tooInstall a konfigurace prostředí Azure PowerShell][PS]. Přihlaste se pomocí předplatného Azure tooyour `Add-AzureAccount`.
* Hello agenta virtuálního počítače se systémem hello virtuálního počítače Azure.

Nejprve ověří, že hello agenta virtuálního počítače je již nainstalována na virtuálním počítači hello. Zadejte název hello cloudové služby a název virtuálního počítače a pak spusťte následující příkazy příkazového řádku Azure PowerShell úrovni správce hello. Nahraďte vše v rámci hello uvozovky, včetně hello < a > znaků.

> [!TIP]
> Pokud si nejste jisti hello Cloudová služba a názvy virtuálních počítačů, spusťte **Get-AzureVM** toolist hello názvy pro všechny virtuální počítače v aktuálním předplatném.

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

Pokud hello **zápisu hostitele** příkaz zobrazí **True**, hello je nainstalovaný Agent virtuálního počítače. Pokud se zobrazí **False**, najdete v části hello pokyny a odkaz toohello, stáhněte si v hello Azure příspěvku na blogu [agenta virtuálního počítače a rozšíření – část 2][Agent].

Pokud je nainstalována hello agenta virtuálního počítače, spusťte tyto příkazy tooinstall hello Symantec Endpoint Protection agent.

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

tooverify, který hello rozšíření zabezpečení Symantec byla nainstalována a je aktuální:

1. Přihlaste se toohello virtuálního počítače. Pokyny najdete v tématu [jak tooLog na tooa virtuální počítač spuštěný Windows Server][Logon].
2. Windows Server 2008 R2, klikněte na tlačítko **Start > Symantec Endpoint Protection**. Windows Server 2012 nebo Windows Server 2012 R2, hello úvodní obrazovce zadejte **Symantec**a potom klikněte na **Symantec Endpoint Protection**.
3. Z hello **stav** kartě hello **stav Symantec Endpoint Protection** časové období, aktualizace nebo v případě potřeby restartovat.

## <a name="additional-resources"></a>Další zdroje
[Jak tooLog na tooa virtuální počítač spuštěný Windows Server][Logon]

[Rozšíření virtuálního počítače Azure a funkce][Ext]

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493

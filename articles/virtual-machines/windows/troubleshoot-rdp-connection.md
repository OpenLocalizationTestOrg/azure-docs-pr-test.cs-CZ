---
title: "aaaCannot připojit pomocí protokolu RDP tooa virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Řešení problémů, když se nelze připojit virtuální počítač Windows tooyour v Azure pomocí vzdálené plochy"
keywords: "Vzdálené plochy chyba, Chyba připojení ke vzdálené ploše, nelze se připojit tooVM, řešení potíží vzdálené plochy"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 07/25/2017
ms.author: genli
ms.openlocfilehash: bbb36136e7a4b187fe8deea621d2b8d46d8aa102
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-remote-desktop-connections-tooan-azure-virtual-machine"></a>Řešení potíží s tooan připojení ke vzdálené ploše virtuálního počítače Azure
Hello protokolu RDP (Remote Desktop) připojení tooyour systému Windows Azure virtuální počítač (VM) může selhat z různých důvodů, můžete ponechat nelze tooaccess virtuálního počítače. Hello problém může být s hello služby Vzdálená plocha na hello virtuálních počítačů, hello síťové připojení nebo hello klienta vzdálené plochy v hostitelském počítači. Tento článek vás provede některé hello nejběžnější metody tooresolve potíží s připojeními RDP. 

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, obraťte se na hello Azure odborníky na [hello fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte toohello [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Rychlé řešení potíží
Po dokončení každého kroku řešení potíží pokuste o připojení toohello virtuálních počítačů:

1. Resetování konfigurace vzdálené plochy.
2. Zkontrolujte skupiny zabezpečení sítě pravidla / Cloud Services koncové body.
3. Zkontrolujte protokoly konzoly virtuálního počítače.
4. Resetovat hello síťovou kartu pro hello virtuálních počítačů.
5. Zkontrolujte hello stavu prostředků virtuálních počítačů.
6. Resetování hesla virtuálních počítačů.
7. Restartujte virtuální počítač.
8. Opětovné nasazení virtuálního počítače.

Pokud potřebujete podrobnější kroky a vysvětlení pokračujte ve čtení. Ověřte, že místní síťové vybavení, jako jsou směrovače a brány firewall neblokují odchozí TCP port 3389, jak jsme uvedli v [podrobné RDP scénáře řešení potíží](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!TIP]
> Pokud hello **připojit** tlačítko pro virtuální počítač neaktivní odhlašování hello portálu a nejste připojené tooAzure prostřednictvím [Express Route](../../expressroute/expressroute-introduction.md) nebo [Site-to-Site VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) připojení, je nutné toocreate a přiřazení adres veřejnou IP adresu virtuálního počítače před použitím protokolu RDP. Podle potřeby si můžete přečíst další informace o [veřejných IP adresách v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-tootroubleshoot-rdp-issues"></a>Způsoby tootroubleshoot problémy protokolu RDP
Řešení potíží s virtuální počítače vytvořené pomocí modelu nasazení Resource Manager hello pomocí jedné z následujících metod hello:

* [Portál Azure](#using-the-azure-portal) – výborné Pokud potřebujete tooquickly resetovat hello RDP konfigurace nebo přihlašovací údaje uživatele a nebudete mít hello nainstalovány nástroje pro Azure.
* [Prostředí Azure PowerShell](#using-azure-powershell) – Pokud umíte pracovat s příkazovém řádku prostředí PowerShell rychle resetování hello RDP konfigurace nebo přihlašovací údaje uživatele pomocí rutin prostředí Azure PowerShell hello.

Můžete také získat kroky k vyřešení problémů s virtuální počítače vytvořené pomocí hello [model nasazení Classic](#troubleshoot-vms-created-using-the-classic-deployment-model).

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-hello-azure-portal"></a>Řešení potíží pomocí hello portálu Azure
Po dokončení každého kroku řešení potíží zkuste se znovu připojit tooyour virtuálních počítačů. Pokud se pořád nemůžete připojit, zkuste hello další krok.

1. **Resetování připojení RDP**. Tento krok řešení potíží obnoví konfiguraci RDP hello při vzdálená připojení jsou zakázané nebo pravidla brány Windows Firewall blokuje protokolu RDP, třeba.
   
    Vyberte virtuální počítač v hello portálu Azure. Projděte dolů hello nastavení podokně toohello **podporu + Poradce při potížích s** části dolní části seznamu hello. Klikněte na tlačítko hello **resetovat heslo** tlačítko. Sada hello **režimu** příliš**jenom resetování konfigurace** a pak klikněte na tlačítko hello **aktualizace** tlačítko:
   
    ![Resetování konfigurace RDP hello v hello portálu Azure](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Skupina zabezpečení sítě ověřte, zda pravidla**. Použití [IP tok ověření](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) tooconfirm Pokud pravidla v skupinu zabezpečení sítě blokuje tooor provoz z virtuálního počítače. Můžete také zkontrolovat efektivní zabezpečení skupiny pravidel tooensure příchozí "Povolit" NSG pravidel existuje a prioritu pro RDP port (standardně 3389). Další informace najdete v tématu [tok provozu pomocí efektivní pravidla zabezpečení tootroubleshoot virtuálních počítačů](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).

3. **Diagnostika spouštění virtuálních počítačů zkontrolujte**. Tento krok řešení potíží zkontroluje toodetermine protokoly konzoly virtuálního počítače hello, pokud hello virtuálního počítače je oznámení chyby. Ne všechny virtuální počítače mají Diagnostika spouštění povoleno, takže tento krok řešení problémů může být volitelné.
   
    Konkrétní kroky pro řešení potíží jsou nad rámec tohoto článku hello, ale může znamenat širší problém, který ovlivňuje připojení RDP. Další informace o kontrole hello protokoly konzoly a snímek virtuálního počítače najdete v tématu [Diagnostika spouštění pro virtuální počítače](boot-diagnostics.md).

4. **Resetování hello síťový adaptér pro virtuální počítač hello**. Další informace najdete v tématu [jak tooreset síťový adaptér virtuálního počítače Windows Azure](reset-network-interface.md).
5. **Zkontrolujte hello stavu prostředků virtuálních počítačů**. Toto řešení potíží krok ověřuje, že zde nejsou žádné známé problémy s hello platformy Azure, který může mít vliv na toohello připojení virtuálních počítačů.
   
    Vyberte virtuální počítač v hello portálu Azure. Projděte dolů hello nastavení podokně toohello **podporu + Poradce při potížích s** části dolní části seznamu hello. Klikněte na tlačítko hello **stav prostředku** tlačítko. V pořádku virtuálního počítače hlásí, že je **dostupné**:
   
    ![Zkontrolujte stav prostředků virtuálních počítačů v hello portálu Azure](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Resetovat přihlašovací údaje uživatele**. Tento krok řešení potíží resetuje heslo hello na účet místního správce, když nejste jistí nebo zapomněli hello přihlašovací údaje.
   
    Vyberte virtuální počítač v hello portálu Azure. Projděte dolů hello nastavení podokně toohello **podporu + Poradce při potížích s** části dolní části seznamu hello. Klikněte na tlačítko hello **resetovat heslo** tlačítko. Ujistěte se, zda text hello **režimu** je nastaven příliš**resetovat heslo** a pak zadejte svoje uživatelské jméno a nové heslo. Nakonec klikněte na hello **aktualizace** tlačítko:
   
    ![Resetovat přihlašovací údaje uživatele hello v hello portálu Azure](./media/troubleshoot-rdp-connection/reset-password.png)
7. **Restartujte virtuální počítač**. Tento krok řešení potíží můžete vyřešit všechny základní problémy s hello virtuální počítač.
   
    Vyberte virtuální počítač v hello portál Azure a klikněte na tlačítko hello **přehled** kartě. Klikněte na tlačítko hello **restartujte** tlačítko:
   
    ![Restartování hello virtuálních počítačů v hello portálu Azure](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **Opětovné nasazení virtuálního počítače**. Tento krok řešení potíží opětovně nasadí tooanother hostiteli virtuálních počítačů v rámci Azure toocorrect všechny základní platformu nebo síťové problémy.
   
    Vyberte virtuální počítač v hello portálu Azure. Projděte dolů hello nastavení podokně toohello **podporu + Poradce při potížích s** části dolní části seznamu hello. Klikněte na tlačítko hello **znovu nasaďte** tlačítko a potom klikněte na **znovu nasaďte**:
   
    ![Znovu nasaďte hello virtuálních počítačů v hello portálu Azure](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    Po dokončení této operace je dočasné diskových dat ztraceny a dynamické IP adresy, které jsou přidružené hello virtuálních počítačů jsou aktualizovány.

Pokud jsou stále dochází k problémům RDP, můžete [otevřete žádost o podporu](https://azure.microsoft.com/support/options/) nebo si můžete přečíst [podrobnější RDP, koncepty a kroky pro řešení potíží](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-using-azure-powershell"></a>Řešení potíží pomocí prostředí Azure PowerShell
Pokud jste to ještě neudělali, [instalace a konfigurace hello nejnovější prostředí Azure PowerShell](/powershell/azure/overview).

Hello následující příklady použití proměnných, jako `myResourceGroup`, `myVM`, a `myVMAccessExtension`. Tyto názvy proměnných a umístění nahraďte vlastními hodnotami.

> [!NOTE]
> Resetovat hello uživatelských přihlašovacích údajů a konfiguraci RDP hello pomocí hello [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) rutiny prostředí PowerShell. V následujících příkladech hello `myVMAccessExtension` je název, který zadáte jako součást procesu hello. Pokud jste dříve pracovali s hello VMAccessAgent, můžete získat název hello hello existující rozšíření pomocí `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` toocheck hello vlastnosti hello virtuálních počítačů. tooview hello název, naleznete v části hello rozšíření hello výstupu.

Po dokončení každého kroku řešení potíží zkuste se znovu připojit tooyour virtuálních počítačů. Pokud se pořád nemůžete připojit, zkuste hello další krok.

1. **Resetování připojení RDP**. Tento krok řešení potíží obnoví konfiguraci RDP hello při vzdálená připojení jsou zakázané nebo pravidla brány Windows Firewall blokuje protokolu RDP, třeba.
   
    postupujte podle příkladu Hello resetuje hello připojení RDP na virtuální počítač s názvem `myVM` v hello `WestUS` umístění a v hello skupinu prostředků s názvem `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Skupina zabezpečení sítě ověřte, zda pravidla**. Tento řešení potíží krok ověřuje, že máte pravidlo v provozu protokolu RDP toopermit skupinu zabezpečení sítě. Hello výchozí port pro protokol RDP je TCP port 3389. Provoz protokolu RDP toopermit pravidla nemusí být vytvořen automaticky při vytváření virtuálního počítače.
   
    Nejprve přiřadit všechny hello konfigurační data pro vaší skupinu zabezpečení sítě toohello `$rules` proměnné. Hello následující příklad získává informace o hello skupinu zabezpečení sítě s názvem `myNetworkSecurityGroup` v hello skupinu prostředků s názvem `myResourceGroup`:
   
    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Prohlédnout hello pravidla, které jsou nakonfigurované pro tuto skupinu zabezpečení sítě. Ověřte, zda pravidlo existuje tooallow TCP port 3389 pro příchozí připojení následujícím způsobem:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    Hello následující příklad ukazuje pravidlo platný zabezpečení, které povoluje provoz protokolu RDP. Můžete zobrazit `Protocol`, `DestinationPortRange`, `Access`, a `Direction` jsou správně nakonfigurovány:
   
    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```
   
    Pokud nemáte pravidlo, které povoluje provoz protokolu RDP, [vytvořte skupinu zabezpečení sítě pravidlo](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Povolte TCP port 3389.
3. **Resetovat přihlašovací údaje uživatele**. Tento krok řešení potíží resetuje heslo hello na hello účet místního správce, který můžete zadat, když jste si nejste jisti nebo zapomněli hello přihlašovací údaje.
   
    Nejdřív zadejte hello uživatelské jméno a nové heslo přiřazením pověření toohello `$cred` proměnná následujícím způsobem:
   
    ```powershell
    $cred=Get-Credential
    ```
   
    Nyní aktualizujte hello přihlašovací údaje na vašem virtuálním počítači. Hello následující příklad aktualizuje hello pověření na virtuálním počítači s názvem `myVM` v hello `WestUS` umístění a v hello skupinu prostředků s názvem `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **Restartujte virtuální počítač**. Tento krok řešení potíží můžete vyřešit všechny základní problémy s hello virtuální počítač.
   
    Následující příklad restartování Hello hello virtuálního počítače s názvem `myVM` v hello skupinu prostředků s názvem `myResourceGroup`:
   
    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **Opětovné nasazení virtuálního počítače**. Tento krok řešení potíží opětovně nasadí tooanother hostiteli virtuálních počítačů v rámci Azure toocorrect všechny základní platformu nebo síťové problémy.
   
    Následující příklad opětovně nasadí Hello hello virtuálního počítače s názvem `myVM` v hello `WestUS` umístění a v hello skupinu prostředků s názvem `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Pokud jsou stále dochází k problémům RDP, můžete [otevřete žádost o podporu](https://azure.microsoft.com/support/options/) nebo si můžete přečíst [podrobnější RDP, koncepty a kroky pro řešení potíží](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-vms-created-using-hello-classic-deployment-model"></a>Řešení potíží s virtuální počítače vytvořené pomocí modelu nasazení Classic hello
Po dokončení každého kroku řešení potíží pokuste o připojení toohello virtuálních počítačů.

1. **Resetování připojení RDP**. Tento krok řešení potíží obnoví konfiguraci RDP hello při vzdálená připojení jsou zakázané nebo pravidla brány Windows Firewall blokuje protokolu RDP, třeba.
   
    Vyberte virtuální počítač v hello portálu Azure. Klikněte na tlačítko hello **... Další** tlačítko a pak klikněte na **resetovat vzdálený přístup**:
   
    ![Resetování konfigurace RDP hello v hello portálu Azure](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Ověření koncových bodů cloudové služby**. Tento krok řešení potíží ověřuje, mají koncové body v provozu protokolu RDP toopermit cloudové služby. Hello výchozí port pro protokol RDP je TCP port 3389. Provoz protokolu RDP toopermit pravidla nemusí být vytvořen automaticky při vytváření virtuálního počítače.
   
   Vyberte virtuální počítač v hello portálu Azure. Klikněte na tlačítko hello **koncové body** tlačítko koncové body hello tooview, která je konfigurovaná pro virtuální počítač. Ověřte, že existují koncových bodů, povolit provoz protokolu RDP na TCP port 3389.
   
   Hello následující příklad ukazuje platný koncové body, které umožňují provoz protokolu RDP:
   
   ![Ověření koncových bodů cloudové služby v hello portálu Azure](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   Pokud nemáte koncový bod, který umožňuje provoz protokolu RDP, [vytvoření koncového bodu služby Cloud Services](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Povolte TCP tooprivate portu 3389.
3. **Diagnostika spouštění virtuálních počítačů zkontrolujte**. Tento krok řešení potíží zkontroluje toodetermine protokoly konzoly virtuálního počítače hello, pokud hello virtuálního počítače je oznámení chyby. Ne všechny virtuální počítače mají Diagnostika spouštění povoleno, takže tento krok řešení problémů může být volitelné.
   
    Konkrétní kroky pro řešení potíží jsou nad rámec tohoto článku hello, ale může znamenat širší problém, který ovlivňuje připojení RDP. Další informace o kontrole hello protokoly konzoly a snímek virtuálního počítače najdete v tématu [Diagnostika spouštění pro virtuální počítače](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).
4. **Zkontrolujte hello stavu prostředků virtuálních počítačů**. Toto řešení potíží krok ověřuje, že zde nejsou žádné známé problémy s hello platformy Azure, který může mít vliv na toohello připojení virtuálních počítačů.
   
    Vyberte virtuální počítač v hello portálu Azure. Projděte dolů hello nastavení podokně toohello **podporu + Poradce při potížích s** části dolní části seznamu hello. Klikněte na tlačítko hello **stav prostředku** tlačítko. V pořádku virtuálního počítače hlásí, že je **dostupné**:
   
    ![Zkontrolujte stav prostředků virtuálních počítačů v hello portálu Azure](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Resetovat přihlašovací údaje uživatele**. Tento krok řešení potíží resetuje heslo hello na účet místního správce hello je zadat, když nejste jistí nebo zapomněli hello přihlašovací údaje.
   
    Vyberte virtuální počítač v hello portálu Azure. Projděte dolů hello nastavení podokně toohello **podporu + Poradce při potížích s** části dolní části seznamu hello. Klikněte na tlačítko hello **resetovat heslo** tlačítko. Zadejte svoje uživatelské jméno a nové heslo. Nakonec klikněte na hello **Uložit** tlačítko:
   
    ![Resetovat přihlašovací údaje uživatele hello v hello portálu Azure](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **Restartujte virtuální počítač**. Tento krok řešení potíží můžete vyřešit všechny základní problémy s hello virtuální počítač.
   
    Vyberte virtuální počítač v hello portál Azure a klikněte na tlačítko hello **přehled** kartě. Klikněte na tlačítko hello **restartujte** tlačítko:
   
    ![Restartování hello virtuálních počítačů v hello portálu Azure](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

Pokud jsou stále dochází k problémům RDP, můžete [otevřete žádost o podporu](https://azure.microsoft.com/support/options/) nebo si můžete přečíst [podrobnější RDP, koncepty a kroky pro řešení potíží](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-specific-rdp-errors"></a>Řešení specifických chyb protokolu RDP
Při pokusu o tooconnect tooyour virtuálního počítače prostřednictvím protokolu RDP se můžete setkat s konkrétní chybová zpráva. Hello následují hello nejběžnější chybové zprávy:

* [Hello relace byla odpojena, protože neexistují žádné licenční servery vzdálené plochy k dispozici tooprovide licenci](troubleshoot-specific-rdp-errors.md#rdplicense).
* [Vzdálená plocha nenašla hello počítač "název"](troubleshoot-specific-rdp-errors.md#rdpname).
* [Došlo k chybě ověřování. Hello místní úřad zabezpečení nelze kontaktovat](troubleshoot-specific-rdp-errors.md#rdpauth).
* [Chyba zabezpečení systému Windows: přihlašovací údaje nebyla úspěšná](troubleshoot-specific-rdp-errors.md#wincred).
* [Počítač se nemůže připojit vzdálený počítač toohello](troubleshoot-specific-rdp-errors.md#rdpconnect).

## <a name="additional-resources"></a>Další zdroje
Pokud žádná z těchto chyb došlo k chybě a se pořád nemůžete připojit toohello virtuálního počítače prostřednictvím vzdálené plochy, přečtěte si podrobné hello [Průvodce řešením potíží pro vzdálené plochy](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Řešení potíží s kroky v přístupu k aplikacím spuštěným na virtuálním počítači, najdete v části [Poradce při potížích přístup tooan aplikace běžící na virtuálním počítači Azure](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Pokud máte problémy s tooa tooconnect Secure Shell (SSH) virtuálního počítače s Linuxem v Azure, najdete v části [řešení SSH připojení tooa virtuálního počítače s Linuxem v Azure](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


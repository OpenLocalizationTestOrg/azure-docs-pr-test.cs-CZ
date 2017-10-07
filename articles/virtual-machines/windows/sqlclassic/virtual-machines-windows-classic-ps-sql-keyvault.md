---
title: "aaaIntegrate Key Vault se systémem SQL Server na virtuálních počítačích Windows v Azure (klasický) | Microsoft Docs"
description: "Zjistěte, jak tooautomate hello konfigurace systému SQL Server šifrování pro použití s Azure Key Vault. Toto téma vysvětluje, jak vytvořit toouse Azure Key Vault integrace s virtuálními počítači systému SQL Server v modelu nasazení classic hello."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: ab8d41a7-1971-4032-ab71-eb435c455dc1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/17/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 54664875b76dac7271d5a9f00b3f41fdc9c08491
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-classic"></a>Konfigurace integrace Azure Key Vault pro SQL Server na virtuálních počítačích Azure (klasický)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-ps-sql-keyvault.md)
> * [Classic](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Přehled
Existuje více funkcí šifrování systému SQL Server, například [transparentní šifrování dat (šifrování TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [šifrování na úrovni sloupce (Vymazat)](https://msdn.microsoft.com/library/ms173744.aspx), a [zálohu šifrovacího](https://msdn.microsoft.com/library/dn449489.aspx). Tyto formuláře šifrování vyžadovat toomanage a uložení hello kryptografických klíčů, které používáte pro šifrování. Hello službou Azure Key Vault (AZURE) služby je navrženou tooimprove hello zabezpečení a správu tyto klíče v umístění zabezpečené a vysoce dostupné. Hello [konektor služby serveru SQL](http://www.microsoft.com/download/details.aspx?id=45344) umožňuje tyto klíče z Azure Key Vault toouse systému SQL Server.

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Pokud používáte systém SQL Server s místním počítačům, jsou [kroky, které vám pomůžou tooaccess Azure Key Vault z vašeho místního počítače systému SQL Server](https://msdn.microsoft.com/library/dn198405.aspx). Ale pro SQL Server ve virtuálních počítačích Azure, můžete ušetřit čas pomocí hello *Azure Key Vault integrace* funkce. S několika tooenable rutin prostředí Azure PowerShell tuto funkci můžete automatizovat hello konfigurace, které jsou nezbytné pro virtuální počítač SQL tooaccess trezoru klíčů.

Pokud je tato funkce povolena, automaticky se nainstaluje hello konektor služby serveru SQL, nakonfiguruje hello EKM provider tooaccess Azure Key Vault a vytvoří tooallow hello přihlašovacích údajů můžete tooaccess svůj trezor. Pokud hledá v hello kroky v hello už jsme zmínili místní dokumentaci, uvidíte, že tato funkce automatizuje kroky 2 a 3. Hello pouze věc stále nutné toodo ručně, je trezor klíčů hello toocreate a klíče. Z tohoto místa se automatizované hello celý nastavení virtuálního počítače SQL. Po dokončení této instalace této funkce můžete spustit T-SQL příkazy toobegin šifrování databáze nebo zálohy běžným způsobem.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>Konfigurace integrace se službou AZURE
Pomocí prostředí PowerShell tooconfigure Azure Key Vault integrace. Hello následující oddíly poskytují přehled o hello požadované parametry a potom ukázkový skript prostředí PowerShell.

### <a name="install-hello-sql-server-iaas-extension"></a>Nainstalujte hello IaaS rozšíření systému SQL Server
První, [nainstalovat hello rozšíření systému SQL Server IaaS](../classic/sql-server-agent-extension.md).

### <a name="understand-hello-input-parameters"></a>Pochopení hello vstupní parametry
Hello následující parametry hello seznamy tabulky vyžaduje skript prostředí PowerShell hello toorun v další části hello.

| Parametr | Popis | Příklad |
| --- | --- | --- |
| **$akvURL** |**Adresa URL trezoru klíčů Hello** |"https://contosokeyvault.vault.azure.net/" |
| **$spName** |**Hlavní název služby** |"fde2b411 - 33d 5-4e11-af04eb07b669ccf2" |
| **$spSecret** |**Tajný klíč objektu služby** |"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =" |
| **$credName** |**Název přihlašovacího údaje**: integrace se službou AZURE vytvoří přihlašovacích údajů v rámci SQL Server, a umožní hello virtuálních počítačů toohave přístup toohello klíče trezoru. Zvolte název pro tyto přihlašovací údaje. |"mycred1" |
| **$vmName** |**Název virtuálního počítače**: hello název vytvořeného virtuálního počítače s SQL. |"myvmname" |
| **$serviceName** |**Název služby**: název hello cloudové služby, který je přidružen hello virtuální počítač SQL. |"mycloudservicename" |

### <a name="enable-akv-integration-with-powershell"></a>Povolení integrace se službou AZURE pomocí prostředí PowerShell
Hello **New-AzureVMSqlServerKeyVaultCredentialConfig** rutina vytvoří objekt konfigurace pro funkci Azure Key Vault integrace hello. Hello **Set-AzureVMSqlServerExtension** nakonfiguruje této integrace s hello **KeyVaultCredentialSettings** parametr. Dobrý den, jak následující kroky zobrazit toouse těchto příkazů.

1. V prostředí Azure PowerShell nakonfigurujte nejprve hello vstupní parametry s konkrétními hodnotami, jak je popsáno v předchozích částech tohoto tématu hello. Následující skript Hello je příklad.
   
        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2. Pak použijte hello následující skript tooconfigure a povolení integrace se službou AZURE.
   
        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

Hello rozšíření agenta systému SQL IaaS aktualizuje hello virtuální počítač SQL tuto novou konfiguraci.

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]


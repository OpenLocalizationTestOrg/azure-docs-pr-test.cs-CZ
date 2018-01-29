---
title: "Připojení k virtuální síti Azure SSIS integrace runtime | Microsoft Docs"
description: "Zjistěte, jak propojit runtime integrace Azure SSIS virtuální sítě Azure."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
ms.openlocfilehash: 917c3e23fed468a04783456e7dc74a42bea60ae7
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Připojení modulu runtime integrace Azure SSIS k virtuální síti.
Modul runtime integrace Azure SSIS (IR) musí připojení k virtuální sítě Azure (VNet), pokud platí jedna z následujících podmínek: 

- Databázi katalogu služby SSIS hostujete na spravované instanci SQL Serveru (privátní verze Preview), která je součástí virtuální sítě.
- Z balíčků SSIS spuštěných v prostředí Azure SSIS Integration Runtime se chcete připojit k místním úložištím dat.

 Azure Data Factory verze 2 (Preview) umožňuje připojení k vaší runtime integrace Azure SSIS klasický nebo o virtuální síť Azure Resource Manager. 

 > [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), prostudujte si [dokumentaci služby Data Factory verze 1](v1/data-factory-introduction.md).

## <a name="access-on-premises-data-stores"></a>Přístup k místním úložištím dat.
Pokud balíčky SSIS přistupovat k úložištím dat pouze veřejného cloudu, nemusíte připojit k virtuální síti Azure SSIS reakcí na Incidenty. Pokud balíčky SSIS přístup k místním úložištím dat, musíte se připojit k Azure SSIS IR k virtuální síti, která je připojena k místní síti. Pokud katalog služby SSIS je hostovaná v Azure SQL Database, který není ve virtuální síti, budete muset otevřít správné porty. Pokud katalog služby SSIS je hostovaná v Azure SQL spravované Instance, která se nachází v o virtuální síť Azure Resource Manager nebo klasické virtuální sítě, můžete spojit Azure SSIS IR stejnou virtuální síť (nebo) jinou virtuální síť, který má připojení VNet-to-VNet se takový, který má spravované Instance Azure SQL. Další podrobnosti naleznete v následujících částech.

Tady je několik bodů důležité si uvědomit: 

- Pokud je žádné existující virtuální síť připojen k síti na pracovišti, nejprve vytvořit [virtuální sítě Azure Resource Manageru](../virtual-network/virtual-network-get-started-vnet-subnet.md#create-vnet) nebo [klasické virtuální síti](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) pro vaše integrace Azure SSIS modulu runtime pro připojení. Potom nakonfigurujte site-to-site [připojení brány sítě VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md)/[ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) připojení z této virtuální sítě k síti na pracovišti.
- Pokud dojde existující virtuální síť Azure Resource Manager nebo classic, které virtuální sítě připojený k síti na pracovišti ve stejném umístění jako vaše runtime integrace Azure SSIS, můžete spojit vaše runtime integrace Azure SSIS k němu.
- Pokud dojde existující klasické virtuální sítě připojený k síti na pracovišti v jiném umístění z vaší Runtime integrace Azure SSIS, je nejprve vytvořit [klasické virtuální síti](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) pro vaše integrace Azure SSIS modulu Runtime pro připojení. Potom nakonfigurujte [virtuální sítě classic classic](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) připojení. Nebo můžete vytvořit [virtuální sítě Azure Resource Manageru](../virtual-network/virtual-network-get-started-vnet-subnet.md#create-vnet) pro vaše integrace Azure SSIS modulu runtime pro připojení. Nakonfigurujte [classic Azure Resource Manager VNet](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) připojení.
- Pokud je stávající virtuální síť Azure Resource Manager z vaší Runtime integrace Azure SSIS připojené k síti na pracovišti v jiném umístění, je nejprve vytvořit [virtuální sítě Azure Resource Manageru](../virtual-network/virtual-network-get-started-vnet-subnet.md#create-vnet) pro Azure-SSIS integrační modul runtime pro připojení. Potom nakonfigurujte připojení k virtuální síť prostředků Azure Manager Azure Resource Manager. Nebo můžete vytvořit [klasické virtuální síti](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) pro vaše integrace Azure SSIS modulu runtime pro připojení. Potom nakonfigurujte [classic Azure Resource Manager VNet](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) připojení.

## <a name="domain-name-services-server"></a>Domain Name Services server 
Pokud budete muset použít vlastní server služby DNS (Domain Name) ve virtuální síti připojí pomocí vaší runtime integrace Azure SSIS, použijte pokyny, které [zajistit, že uzly vaší runtime integrace Azure SSIS ve virtuální síti může přeložit koncové body Azure](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="network-security-group"></a>Skupina zabezpečení sítě
Pokud potřebujete implementovat skupina zabezpečení sítě (NSG) ve virtuální síti připojí pomocí vaší Runtime integrace Azure SSIS, povolte příchozí nebo odchozí traffics pomocí následující porty:

| Porty | Směr | Přenosový protokol | Účel | Příchozí výstupního zdroje nebo cíle |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100<br/>20100<br/>30100  | Příchozí | TCP | Služby Azure používají tyto porty ke komunikaci s uzly, které vaše integrace modulu runtime Azure SSIS ve virtuální síti. | Internet | 
| 443 | Odchozí | TCP | Tento port použít uzly vaší runtime integrace Azure SSIS ve virtuální síti přístupu ke službám Azure, například Azure Storage, centra událostí, atd. | INTERNET | 
| 1433<br/>11000-11999<br/>14000-14999  | Odchozí | TCP | Uzly vaše integrace modulu runtime Azure SSIS ve virtuální síti používat tyto porty pro přístup k SSISDB hostované serverem Azure SQL Database (není k dispozici pro SSISDB hostované spravované Instance SQL Azure). | Internet | 

## <a name="configure-vnet"></a>Konfigurace virtuální sítě
Nejprve je nutné nakonfigurovat virtuální síť pomocí jedné z následujících způsobů (skriptu vs. Portál Azure) než připojíte Azure SSIS IR k virtuální síti. 

### <a name="script-to-configure-vnet"></a>Skript, který nakonfiguruje virtuální sítě 
Přidejte následující skript pro automatickou konfiguraci virtuální sítě nebo nastavení oprávnění pro vaše modulu runtime integrace Azure SSIS připojení k virtuální síti.

```powershell
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="use-portal-to-configure-a-classic-vnet"></a>Prostřednictvím portálu můžete nakonfigurovat klasické virtuální sítě
Spouštění skriptu je nejjednodušší způsob, jak nakonfigurovat virtuální síť. Pokud nemáte přístup ke konfiguraci této virtuální síti nebo služba Automatická konfigurace nezdaří, vlastník této virtuální sítě / můžete zkusit ruční konfigurace v následujících krocích:

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **další služby**. Filtrovat a vyberte **virtuální sítě (klasické)**.
3. Filtrovat a vybrat vaše **virtuální sítě** v seznamu. 
4. Na stránce virtuální sítě (klasické) vyberte **vlastnosti**. 

    ![ID prostředku klasické virtuální sítě](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. Klikněte na tlačítko Kopírovat **ID prostředku** ID prostředku pro klasické síťové zkopírovat do schránky. Uložte ID ze schránky v OneNote nebo souboru.
6. Klikněte na tlačítko **podsítě** v levé nabídce a ujistěte se, že počet **dostupné adresy** je větší než uzly ve vaší runtime integrace Azure SSIS.

    ![Počet dostupných adres ve virtuální síti](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. Připojení k **MicrosoftAzureBatch** k **Classic Přispěvatel virtuálních počítačů** role sítě vnet. 
    1. Klikněte na řízení přístupu (IAM) v levé nabídce a klikněte na tlačítko **přidat** na panelu nástrojů.
    
        ![Řízení přístupu -> Přidat](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png) 
    2. V **přidat oprávnění** vyberte **Classic Přispěvatel virtuálních počítačů** pro **Role**. Zkopírujte a vložte **ddbf3205-c6bd-46ae-8127-60eb93363864** v **vyberte** textového pole a pak vyberte **Microsoft Azure Batch** ze seznamu výsledků hledání. 
    
        ![Přidání oprávnění – vyhledávání](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)
    3. Kliknutím na Uložit uložte nastavení a zavřete stránku.
    
        ![Uložit nastavení přístupu](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)
    4. Zkontrolujte, jestli **MicrosoftAzureBatch** v seznamu přispěvatele.
    
        ![Potvrďte volbu AzureBatch přístup](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)
5. Ověřte, že tohoto zprostředkovatele Azure Batch je zaregistrován v rámci předplatného Azure, který má virtuální sítě nebo zaregistrujte zprostředkovatele Azure Batch. Pokud již máte účet Azure Batch v rámci vašeho předplatného, je zaregistrován předplatného pro Azure Batch.
    1. Na portálu Azure, klikněte na tlačítko **odběry** v levé nabídce. 
    2. Vyberte vaše **předplatné**. 
    3. Klikněte na tlačítko **zprostředkovatelé prostředků** na levé straně a ověřte, že `Microsoft.Batch` je registrovaný poskytovatel. 
    
        ![potvrzení batch zaregistrován](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

    Pokud nevidíte `Microsoft.Batch` je v seznamu, a zaregistrujte ho, [vytvořit prázdný účet Azure Batch](../batch/batch-account-create-portal.md) ve vašem předplatném. Chcete-li odstranit později. 

### <a name="use-portal-to-configure-an-azure-resource-manager-vnet"></a>Prostřednictvím portálu můžete nakonfigurovat o virtuální síť Azure Resource Manager
Spouštění skriptu je nejjednodušší způsob, jak nakonfigurovat virtuální síť. Pokud nemáte přístup ke konfiguraci této virtuální síti nebo služba Automatická konfigurace nezdaří, vlastník této virtuální sítě / můžete zkusit ruční konfigurace v následujících krocích:

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **další služby**. Filtrovat a vyberte **virtuální sítě**.
3. Filtrovat a vybrat vaše **virtuální sítě** v seznamu. 
4. Na stránce virtuální sítě, vyberte **vlastnosti**. 
5. Klikněte na tlačítko Kopírovat **ID prostředku** ID prostředku pro virtuální síť zkopírovat do schránky. Uložte ID ze schránky v OneNote nebo souboru.
6. Klikněte na tlačítko **podsítě** v levé nabídce a ujistěte se, že počet **dostupné adresy** je větší než uzly ve vaší runtime integrace Azure SSIS.
5. Ověřte, že tohoto zprostředkovatele Azure Batch je zaregistrován v rámci předplatného Azure, který má virtuální sítě nebo zaregistrujte zprostředkovatele Azure Batch. Pokud již máte účet Azure Batch v rámci vašeho předplatného, je zaregistrován předplatného pro Azure Batch.
    1. Na portálu Azure, klikněte na tlačítko **odběry** v levé nabídce. 
    2. Vyberte vaše **předplatné**. 
    3. Klikněte na tlačítko **zprostředkovatelé prostředků** na levé straně a ověřte, že `Microsoft.Batch` je registrovaný poskytovatel. 
    
        ![potvrzení batch zaregistrován](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

    Pokud nevidíte `Microsoft.Batch` je v seznamu, a zaregistrujte ho, [vytvořit prázdný účet Azure Batch](../batch/batch-account-create-portal.md) ve vašem předplatném. Chcete-li odstranit později.

## <a name="create-an-azure-ssis-ir-and-join-it-to-a-vnet"></a>Vytvoření služby Azure SSIS reakcí na Incidenty a připojení k virtuální síti
Můžete vytvořit Azure SSIS reakcí na Incidenty a připojení k virtuální síti současně. Dokončení skriptu a pokyny k vytvoření služby Azure SSIS reakcí na Incidenty a připojení k virtuální síti současně najdete v tématu [vytvořit Azure SSIS IR](create-azure-ssis-integration-runtime.md).

## <a name="join-an-existing-azure-ssis-ir-to-a-vnet"></a>Připojení existující Azure SSIS Reakcí do virtuální sítě
Skript u [vytvořit Azure-SSIS integrace runtime](create-azure-ssis-integration-runtime.md) článek ukazuje, jak vytvořit Azure SSIS reakcí na Incidenty a připojení k virtuální síti ve stejném skriptu. Pokud máte existující SSIS Azure, proveďte následující kroky pro připojení k virtuální síti. 

1. Zastavit infračerveného signálu Azure SSIS.
2. Konfigurace Azure SSIS IR propojit virtuální sítě. 
3. Spustit infračerveného signálu Azure SSIS. 

## <a name="define-the-variables"></a>Definování proměnné

```powershell
$ResourceGroupName = "<Azure resource group name>"
$DataFactoryName = "<Data factory name>" 
$AzureSSISName = "<Specify Azure-SSIS IR name>"
## These two parameters apply if you are using a VNet and an Azure SQL Managed Instance (private preview) 
# Specify information about your classic or Azure Resource Manager virtual network (VNet).
$VnetId = "<Name of your Azure virtual netowrk>"
$SubnetName = "<Name of the subnet in VNet>"
```

### <a name="stop-the-azure-ssis-ir"></a>Zastavit IR Azure SSIS
Modul runtime integrace Azure SSIS zastavte, než bude možné připojit k virtuální síti. Tento příkaz uvolní všechny jeho uzly a zastaví fakturace.

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force 
```
### <a name="configure-vnet-settings-for-the-azure-ssis-ir-to-join"></a>Nakonfigurujte nastavení virtuální sítě pro Azure SSIS IR pro připojení
Registrace poskytovatele prostředků Azure Batch:

```powershell
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Konfigurace Azure SSIS IR
Pomocí příkazu Set-AzureRmDataFactoryV2IntegrationRuntime ke konfiguraci modulu runtime integrace Azure SSIS připojení virtuální sítě: 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

## <a name="start-the-azure-ssis-ir"></a>Spustit IR Azure SSIS
Spuštěním následujícího příkazu spusťte prostředí Azure SSIS Integration Runtime: 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```
Dokončení tohoto příkazu trvá **20 až 30 minut**.

## <a name="next-steps"></a>Další postup
Další informace o běhu Azure SSIS najdete v následujících tématech: 

- [Modul Runtime integrace Azure SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). Tento článek obsahuje koncepční informace o integraci runtimes obecně včetně infračerveného signálu Azure SSIS. 
- [Kurz: Nasazení balíčků SSIS do Azure](tutorial-deploy-ssis-packages-azure.md) Tento článek obsahuje podrobné pokyny pro vytvoření Azure-SSIS IR a využívá databázi Azure SQL k hostování katalogu SSIS. 
- [Postup: Vytvoření prostředí Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md) Tento článek dál navazuje na tento kurz a obsahuje pokyny k použití spravované instance SQL Azure (privátní verze Preview) a připojení IR k virtuální síti. 
- [Monitorování Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime). Tento článek ukazuje, jak načíst informace o Azure-SSIS IR a popisy stavů ve vrácených informacích. 
- [Správa Azure-SSIS IR](manage-azure-ssis-integration-runtime.md). Tento článek ukazuje, jak zastavit, spustit nebo odebrat Azure-SSIS IR. Ukazuje také postup horizontálního navýšení kapacity Azure-SSIS IR přidáním více uzlů. 

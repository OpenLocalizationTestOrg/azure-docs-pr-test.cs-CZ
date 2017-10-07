---
title: "adresy aaaAzure úrovni Instance veřejnou IP adresu (klasická) | Microsoft Docs"
description: "Pochopení adresy IP (splnění) úrovni veřejné instance a jak toomanage je pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 07eef6ec-7dfe-4c4d-a2c2-be0abfb48ec5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: 832143ee6fdd80b634e1cebfddc759a8cacda583
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="instance-level-public-ip-classic-overview"></a>Přehled veřejné IP (klasické) instance
Instance úrovně veřejné IP (splnění) je veřejná IP adresa, kterou můžete přiřadit přímo instanci role virtuálního počítače nebo cloudové služby tooa, nikoli toohello Cloudová služba, která jsou umístěny ve vaší instanci virtuálního počítače nebo role. SPLNĚNÍ neberou v místě hello hello virtuální IP (VIP) přiřazený tooyour cloudové služby. Místo toho je další IP adresu, které můžete použít tooconnect přímo tooyour virtuálního počítače nebo instanci role.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje vytváření virtuálních počítačů prostřednictvím Správce prostředků. Musíte rozumět jak [IP adresy](virtual-network-ip-addresses-overview-classic.md) pracovní v Azure.

![Rozdíl mezi splnění a virtuální IP adresy](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Jak je znázorněno na obrázku 1, hello Cloudová služba je získat přístup pomocí virtuální IP adresu, při hello jednotlivé virtuální počítače jsou obvykle získat přístup pomocí virtuálních IP adres:&lt;číslo portu&gt;. Přiřazením splnění tooa konkrétní virtuální počítač, který může být virtuální počítač získat přístup, přímo pomocí tuto IP adresu.

Při vytváření cloudové služby v Azure, odpovídající záznamy DNS A automaticky vytvoří služba toohello tooallow přístup prostřednictvím platný plně kvalifikovaný název domény (FQDN), místo použití hello skutečné VIP. Dobrý den, který se stane, stejný proces pro splnění povolení přístupu toohello virtuálního počítače nebo instanci role ve plně kvalifikovaný název domény místo hello splnění. Například pokud vytvoříte cloudové služby s názvem *contosoadservice*, a konfigurace webové role s názvem *contosoweb* se dvěma instancemi Azure registry hello následující A zaznamenává pro instance hello:

* contosoweb\_IN_0.contosoadservice.cloudapp.net
* contosoweb\_IN_1.contosoadservice.cloudapp.net 

> [!NOTE]
> Můžete přiřadit pouze jeden splnění pro každou instanci virtuálního počítače nebo role. Můžete si too5 ILPIPs za předplatné. ILPIPs nejsou podporovány u virtuálních počítačů více síťovými Kartami.
> 
> 

## <a name="why-would-i-request-an-ilpip"></a>Proč se splnění požadavku?
Pokud chcete toobe možné tooconnect tooyour virtuálního počítače nebo instance role pomocí IP adresy přiřazené přímo tooit, místo použití hello cloudové služby virtuálních IP adres:&lt;číslo portu&gt;, žádosti splnění virtuálního počítače nebo role instance.

* **Aktivní FTP** -přiřazením splnění tooa virtuální počítač mohl přijímat provoz z jakéhokoli portu. Koncové body nejsou potřeba pro hello tooreceive přenosy virtuálních počítačů.  Podrobnosti v protokolu hello FTP najdete v (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [přehled protokolu FTP].
* **Odchozí IP** – odchozí přenosy pocházející z hello virtuálního počítače je mapovaná toohello splnění jako zdroj hello a hello splnění jednoznačně identifikuje entity tooexternal hello virtuálních počítačů.

> [!NOTE]
> V posledních hello byla adresu splnění tooas odkazuje na veřejnou adresu IP (PIP).
> 

## <a name="manage-an-ilpip-for-a-vm"></a>Správa splnění pro virtuální počítač
Hello následující úlohy umožňují toocreate, přiřazení a odebrání ILPIPs z virtuálních počítačů:

### <a name="how-toorequest-an-ilpip-during-vm-creation-using-powershell"></a>Jak toorequest splnění během vytváření virtuálních počítačů pomocí prostředí PowerShell
Hello následující skript prostředí PowerShell vytvoří cloudové služby s názvem *FTPService*, načte bitovou kopii z Azure, vytvoří virtuální počítač s názvem *FTPInstance* pomocí hello načíst image, nastaví toouse hello virtuálních počítačů splnění a přidá novou službu toohello hello virtuálních počítačů:

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-tooretrieve-ilpip-information-for-a-vm"></a>Jak tooretrieve splnění informace pro virtuální počítač
tooview hello splnění informace pro hello virtuální počítač vytvořen pomocí hello předchozí skriptu, spusťte následující příkaz prostředí PowerShell hello a sledovat hello hodnoty pro *PublicIPAddress* a *PublicIPName*:

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

Očekávaný výstup:
 
    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            :   Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

### <a name="how-tooremove-an-ilpip-from-a-vm"></a>Jak tooremove splnění z virtuálního počítače
tooremove hello splnění přidali toohello virtuálních počítačů v předchozí hello skriptu, spusťte následující příkaz prostředí PowerShell hello:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-tooadd-an-ilpip-tooan-existing-vm"></a>Jak tooadd tooan splnění existující virtuální počítač
tooadd splnění toohello virtuální počítač vytvořený pomocí skriptu hello předchozí, spusťte následující příkaz hello:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a>Správa splnění pro instanci role cloudové služby

tooadd instance role splnění tooa cloudové služby, dokončení hello následující kroky:

1. Stáhněte si soubor .cscfg hello pro cloudové služby hello provedením hello kroky hello [jak tooConfigure cloudových služeb](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) článku.
2. Aktualizace souboru .cscfg hello přidáním hello `InstanceAddress` elementu. Hello následující příklad přidá splnění s názvem *MyPublicIP* tooa instance role s názvem *WebRole1*: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ILPIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
          <ConfigurationSettings>
        <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
          </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <InstanceAddress roleName="WebRole1">
        <PublicIPs>
          <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. Nahrát soubor .cscfg hello pro cloudové služby hello provedením hello kroky hello [jak tooConfigure cloudových služeb](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) článku.

## <a name="next-steps"></a>Další kroky
* Pochopit, jak [IP adresování](virtual-network-ip-addresses-overview-classic.md) funguje v modelu nasazení classic hello.
* Další informace o [vyhrazené IP adresy](virtual-networks-reserved-public-ip.md).

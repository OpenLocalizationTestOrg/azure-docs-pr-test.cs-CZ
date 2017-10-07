---
title: "aaaManage Azure rezervovaných IP adres (klasické) – prostředí PowerShell | Microsoft Docs"
description: "Pochopení vyhrazené IP adresy (klasické) a jak toomanage je pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: c0a77b2ab8b1ab9bef6015c903eb735ea4358a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reserved-ip-addresses-classic"></a>Vyhrazené IP adresy (klasické)

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Šablona](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Classic)](virtual-networks-reserved-public-ip.md)

IP adresy v Azure lze rozdělit do dvou kategorií: dynamické a vyhrazené. Veřejné IP adresy, které spravuje Azure je dynamická ve výchozím nastavení. Aby znamená, že hello IP adresy použít pro dané cloudové služby (VIP) nebo tooaccess virtuálního počítače nebo přímo instance role (splnění) můžete změnit čas tootime, když jsou prostředky vypnout nebo zastavena (deallocated).

IP adresy tooprevent změnu, je možné rezervovat IP adresu. Vyhrazené IP adresy lze použít pouze jako virtuální IP adresu, zajistit, že hello IP adresu pro hello Cloudová služba zůstane stejné, hello to i v případě prostředky jsou vypnout nebo zastavená (deallocated). Kromě toho můžete převést existující dynamické IP adresy použít jako VIP tooa vyhrazená IP adresa.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Zjistěte, jak tooreserve statickou veřejnou IP adresu pomocí hello [modelu nasazení Resource Manager](virtual-network-ip-addresses-overview-arm.md).

toolearn Další informace o IP adresy v Azure, přečtěte si hello [IP adresy](virtual-network-ip-addresses-overview-classic.md) článku.

## <a name="when-do-i-need-a-reserved-ip"></a>Co je potřeba vyhrazená IP adresa?
* **Chcete, aby v rámci vašeho předplatného je vyhrazena tooensure, který hello IP**. Pokud chcete tooreserve IP adresu, která se neuvolní ze svého předplatného za žádných okolností, měli byste použít vyhrazený veřejnou IP adresu.  
* **Chcete vaší IP toostay s cloudovou službou i napříč zastavena nebo navrácena stavu (VM)**. Pokud chcete, aby vaše služba toobe přistupovat pomocí IP adresu, která se nezmění, i v případě, že virtuální počítače v hello cloudové služby jsou vypnout nebo zastavit (deallocated).
* **Chcete-li tooensure, odchozí provoz z Azure používá předvídatelný IP adresu**. Můžete mít místní brány firewall nakonfigurované tooallow pouze provozu z konkrétní IP adresy. Vyhrazením IP adresy, víte hello zdrojovou IP adres a nepotřebujete tooupdate pravidla brány firewall kvůli změně IP tooan.

## <a name="faq"></a>Nejčastější dotazy
1. Můžete použít vyhrazenou IP adresu pro všechny služby Azure? <br>
    Ne. Vyhrazené IP adresy lze použít pouze pro virtuální počítače a instance role cloudové služby vystavenou přes virtuální IP adresu.
2. Kolik vyhrazené IP adresy, které může mít <br>
    Podrobnosti najdete v tématu hello [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.
3. Je pro vyhrazené IP adresy zdarma? <br>
    V některých případech. Podrobnosti o cenách najdete v části hello [vyhrazené IP adresy podrobnosti o cenách](http://go.microsoft.com/fwlink/?LinkID=398482) stránky.
4. Jak rezervovat IP adresu? <br>
    Můžete použít PowerShell, hello [REST API pro správu Azure](https://msdn.microsoft.com/library/azure/dn722420.aspx), nebo hello [portál Azure](https://portal.azure.com) tooreserve IP adresu v oblasti Azure. Vyhrazená IP adresa je přidružená tooyour předplatné.
5. Můžete použít vyhrazenou IP adresu na základě skupiny vztahů sítě vnet? <br>
    Ne. Vyhrazené IP adresy jsou podporovány pouze v regionální virtuální sítě. Vyhrazené IP adresy nejsou podporovány pro virtuální sítě, které jsou přidruženy skupiny vztahů. Další informace o přiřazení virtuální síť s oblast nebo skupinu vztahů, najdete v části hello [o virtuální místní sítě a skupiny vztahů](virtual-networks-migrate-to-regional-vnet.md) článku.

## <a name="manage-reserved-vips"></a>Spravovat vyhrazené virtuální IP adresy

Ujistěte se, je nainstalován a nakonfigurován prostředí PowerShell pomocí kroků hello v hello [instalace a konfigurace prostředí PowerShell](/powershell/azure/overview) článku. 

Před použitím vyhrazené IP adresy, je třeba přidat ji tooyour předplatné. toocreate vyhrazenou IP adresu z fondu hello z veřejných IP adres k dispozici v hello *střed USA* umístění, spusťte následující příkaz hello:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

Všimněte si, ale není možné zadat co IP je vyhrazena. tooview jaké IP adresy jsou vyhrazené v rámci vašeho předplatného, spusťte následující příkaz prostředí PowerShell, hello a Všimněte si hello hodnoty pro *ReservedIPName* a *adresu*:

```powershell
Get-AzureReservedIP
```

Očekávaný výstup:

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

>[!NOTE]
>Když vytvoříte vyhrazenou IP adresu pomocí prostředí PowerShell, nelze zadat prostředku skupiny toocreate hello vyhrazené IP v. Azure místech do skupiny prostředků s názvem *výchozí sítě* automaticky. Pokud vytvoříte hello vyhrazené IP pomocí hello [portál Azure](http://portal.azure.com), můžete zadat všechny skupiny prostředků, které zvolíte. Pokud vytvoříte hello vyhrazené IP ve skupině prostředků než *výchozí sítě* však vždy, když odkazujete hello vyhrazené IP adresy s příkazy, jako `Get-AzureReservedIP` a `Remove-AzureReservedIP`, musí odkazovat hello název *Vyhrazený název skupiny prostředků ip název skupiny*.  Například pokud vytvoříte vyhrazená IP adresa s názvem *myReservedIP* ve skupině prostředků s názvem *myResourceGroup*, musí odkazovat hello název hello vyhrazená IP adresa jako *myResourceGroup skupiny myReservedIP*.   

Jakmile je vyhrazené IP adresy, zůstane předplatné přidružené tooyour, odstraňte jej. toodelete vyhrazená IP adresa, spusťte hello následující příkaz prostředí PowerShell:

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-hello-ip-address-of-an-existing-cloud-service"></a>Rezervujte hello IP adresu existující cloudové služby
IP adresa hello stávající cloudovou službu můžete vyhradit přidáním hello `-ServiceName` parametr. IP adresa hello tooreserve cloudové služby *TestService* v hello *střed USA* umístění, spusťte následující příkaz prostředí PowerShell hello:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-tooa-new-cloud-service"></a>Přidružení vyhrazené IP tooa novou cloudovou službu
Hello následující skript vytvoří nové vyhrazená IP adresa a přidruží ji tooa novou cloudovou službu s názvem *TestService*.

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> Když vytvoříte vyhrazené IP toouse ke cloudové službě, je stále odkazovat toohello virtuálních počítačů pomocí *VIP:&lt;číslo portu >* pro příchozí komunikaci. Rezervace IP neznamená, že toohello virtuální počítač může připojit přímo. Hello vyhrazená IP adresa je přiřazen toohello cloudu tohoto hello virtuálního počítače byla nasazena do služby. Pokud chcete tooconnect tooa virtuální počítač podle IP přímo, máte tooconfigure úrovni instance veřejnou IP adresu. Veřejná IP adresa úrovni instance je typ veřejné IP (nazývané splnění), který je přiřazen přímo tooyour virtuálních počítačů. Nelze rezervovat. Další informace najdete v tématu hello [úrovni Instance veřejné IP splnění](virtual-networks-instance-level-public-ip.md) článku.
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a>Odebrání spuštěného nasazení vyhrazená IP adresa
tooremove vyhrazená IP adresa byla přidána tooa novou cloudovou službu, spusťte následující příkaz prostředí PowerShell hello:

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> Odebrání vyhrazenou IP adresu z spuštěného nasazení neodebere hello rezervaci ze svého předplatného. Uvolní jednoduše toobe IP hello používán jiným prostředkem v rámci vašeho předplatného.
> 

## <a name="associate-a-reserved-ip-tooa-running-deployment"></a>Přidružení vyhrazené IP tooa běží nasazení
Hello následující příkazy vytvoření cloudové služby s názvem *TestService2* s nový virtuální počítač s názvem *TestVM2*. Hello existující vyhrazené IP adresy s názvem *MyReservedIP* pak přidružené toohello Cloudová služba.

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-tooa-cloud-service-by-using-a-service-configuration-file"></a>Pomocí konfigurační soubor služby přidružení vyhrazené IP tooa cloudové služby
Pomocí souboru konfigurace (CSCFG) služby lze přiřadit vyhrazené IP tooa cloudové služby. Hello následující ukázkový kód xml ukazuje, jak tooconfigure cloudové služby toouse vyhrazené virtuální IP adresy s názvem *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Další kroky
* Pochopit, jak [IP adresování](virtual-network-ip-addresses-overview-classic.md) funguje v modelu nasazení classic hello.
* Další informace o [vyhrazené soukromé IP adresy](virtual-networks-reserved-private-ip.md).
* Další informace o [Instance úroveň veřejné IP splnění adresy](virtual-networks-instance-level-public-ip.md).


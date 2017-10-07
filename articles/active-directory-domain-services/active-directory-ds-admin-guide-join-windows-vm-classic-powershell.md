---
title: "Azure Active Directory Domain Services: Příručka pro správu | Microsoft Docs"
description: "Připojení k doméně systému Windows virtuálního počítače tooa spravované pomocí prostředí Azure PowerShell a modelu nasazení classic hello."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9143b843-7327-43c3-baab-6e24a18db25e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 21bc5930d84c5368a120f9d81f09320e2a0168fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-windows-server-virtual-machine-tooa-managed-domain-using-powershell"></a>Připojení k systému Windows Server virtuálního počítače tooa spravované doméně pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Portál Azure classic – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell – Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení classic hello. Azure AD Domain Services hello modelu Resource Manager aktuálně nepodporuje.
>
>

Tyto kroky vám ukážou, jak toocustomize sadu Azure PowerShell příkazy, vytvoření a nastavení systému Windows Azure virtuálního počítače pomocí stavebním blokem přístup. Tyto kroky můžete vytvořit virtuální počítač systému Windows Azure a připojení k spravované doméně tooan Azure AD Domain Services.

Tyto kroky použijte přístup doplňovat-v-the-prázdné hodnoty pro vytvoření sady příkazů prostředí Azure PowerShell. Tento přístup může být užitečné, pokud jsou nové tooPowerShell nebo chcete co toospecify hodnoty pro tooknow pro úspěšné konfiguraci. Pokročilí uživatelé prostředí PowerShell můžete pořídit hello příkazy a nahraďte vlastní hodnoty pro proměnné hello (hello řádky začínající "$").

Pokud jste tak ještě neučinili, použijte pokyny hello v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) tooinstall prostředí Azure PowerShell v místním počítači. Potom otevřete příkazový řádek prostředí Windows PowerShell.

## <a name="step-1-add-your-account"></a>Krok 1: Přidání účtu
1. Zadejte v příkazovém prostředí PowerShell hello **Add-AzureAccount** a klikněte na tlačítko **Enter**.
2. Zadejte e-mailovou adresu hello spojené s předplatným Azure a klikněte na tlačítko **pokračovat**.
3. Zadejte heslo hello k vašemu účtu.
4. Klikněte na tlačítko **přihlášení**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Krok 2: Nastavení vaše předplatné a účet úložiště
Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell hello nastavte předplatné a účet úložiště. Nahraďte vše v rámci hello uvozovky, včetně hello < a > znaky, správné názvy hello.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Můžete získat název správné předplatné hello hello Název_předplatného vlastnost hello výstup hello **Get-AzureSubscription** příkaz. Název účtu hello správné úložiště můžete získat z hello vlastnosti Label hello výstup hello **Get-AzureStorageAccount** příkaz po spuštění hello **Select-AzureSubscription** příkaz.

## <a name="step-3-step-by-step-walkthrough---provision-hello-virtual-machine-and-join-it-toohello-managed-domain"></a>Krok 3: Podrobný návod - zřízení hello virtuálního počítače a připojení k spravované doméně toohello
Zde je hello odpovídající prostředí Azure PowerShell příkaz set toocreate tento virtuální počítač s prázdné řádky mezi každého bloku čitelnější.

Zadejte informace o toobe virtuálního počítače Windows hello zřízený.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Hello InstanceSize hodnoty pro D – DS- a G-series virtuálních počítačů naleznete v části [virtuálního počítače a Cloud velikost služeb pro Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Zadejte informace o hello spravované domény.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Zadejte název hello hello cloudové služby.

    $svcname="Contoso100-test"

Zadejte název hello hello virtuální sítě toowhich hello, které by měl být připojen virtuální počítač. Ujistěte se, AAD-DS spravované domény hello je k dispozici v této virtuální síti.

    $vnetname="MyPreviewVnet"

Toobe bitové kopie virtuálního počítače vyberte hello používá tooprovision hello virtuálních počítačů.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigurace virtuálního počítače – název sady virtuálních počítačů, hello instance velikost & bitové kopie toobe použít.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Získejte přihlašovací údaje místního správce pro hello virtuálních počítačů. Zvolte heslo, silné místního správce.

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

Získejte přihlašovací údaje pro účet uživatele patřící too'AAD řadič domény správci skupiny toojoin virtuálních počítačů toohello spravované domény. Nezadávejte název domény hello – pro instance v našem příkladu určíme "bob" hello uživatelské jméno.

    $domainadmincred=Get-Credential –Message "Now type hello name (DO NOT INCLUDE hello DOMAIN) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

Konfigurace hello virtuálního počítače – zadejte požadované pověření systému & požadavek na připojení k doméně.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Nastavte pro podsíť hello virtuálních počítačů.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Volitelné: Bod toohello IP adresa hello domény. Pokud nastavíte hello toobe spravované doméně služby Azure AD Domain Services hello hello IP adresy serverů DNS pro virtuální síť hello, tento krok není povinný.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Nyní hello zřídit připojené k doméně virtuálního počítače s Windows.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-tooprovision-a-windows-vm-and-automatically-join-it-tooan-aad-domain-services-managed-domain"></a>Skript tooprovision virtuální počítač s Windows a automaticky se připojí tooan služby AAD Domain Services spravované doméně
Tuto sadu příkazů prostředí PowerShell vytvoří virtuální počítač pro server obchodní, který:

* Využívá image systému Windows Server 2012 R2 Datacenter hello.
* Je velmi malé virtuální počítač.
* Obsahuje název hello Contoso100 testu.
* Je automaticky domény připojený k toohello contoso100 spravované domény.
* Přidání toohello stejné virtuální síti jako hello spravované domény.

Tady je hello úplnou ukázku najdete skript toocreate hello Windows virtuálního počítače a automaticky se připojí spravované domény toohello Azure AD Domain Services.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

    $domainadmincred=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Související obsah
* [Azure AD Domain Services – Příručka Začínáme](active-directory-ds-getting-started.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)

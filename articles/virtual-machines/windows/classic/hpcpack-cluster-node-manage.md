---
title: "výpočetní uzly clusteru HPC Pack aaaManage | Microsoft Docs"
description: "Další informace o tooadd nástroje skript prostředí PowerShell, odebrat, spuštění a zastavení výpočetní uzly clusteru HPC Pack 2012 R2 v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 5ac1142cc5da984020779434fbb7cba5ad7c14bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Správa hello číslo a dostupnosti výpočetních uzlů v clusteru služby HPC Pack v Azure
Pokud jste vytvořili clusteru HPC Pack 2012 R2 ve virtuálních počítačích Azure, můžete chtít, že výpočetní způsoby tooeasily přidat, odebrat, (zřídit) spuštění nebo zastavení (deprovision) některé virtuální počítače uzlu v clusteru. toodo tyto úlohy spouštět skripty prostředí Azure PowerShell, které jsou nainstalovány na hello hlavního uzlu virtuálního počítače. Tyto skripty pomáhá řídit hello číslo a dostupnost prostředků clusteru HPC Pack, můžete řídit náklady.

> [!IMPORTANT] 
> Tento článek se týká pouze clustery tooHPC Pack 2012 R2 v Azure vytvořit pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.
> Kromě toho nejsou k dispozici v prostředí HPC Pack 2016 hello skriptů prostředí PowerShell popsaných v tomto článku.

## <a name="prerequisites"></a>Požadavky
* **Cluster HPC Pack 2012 R2 ve virtuálních počítačích Azure**: vytvoření clusteru HPC Pack 2012 R2 v modelu nasazení classic hello. Například můžete automatizovat nasazení hello pomocí bitové kopie virtuálních počítačů HPC Pack 2012 R2 hello v hello Azure Marketplace a skript Azure PowerShell. Informace a požadavky najdete v tématu [vytvoření clusteru prostředí HPC s hello skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md).
  
    Po nasazení najít hello uzlu správy skripty v hello % CCP\_DOMOVSKÉ složky bin % hello hlavního uzlu. Spuštění každého ze hello skripty jako správce.
* **Certifikát správy nebo soubor nastavení publikování Azure**: budete potřebovat toodo jedna z následujících hello hello hlavního uzlu:
  
  * **Soubor nastavení publikování import hello Azure**. toodo se spuštění hello následující rutiny prostředí Azure PowerShell hello hlavního uzlu:
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * **Konfigurovat certifikát pro správu Azure hello hello hlavního uzlu**. Pokud máte soubor .cer hello, importujte ji do úložiště certifikátů CurrentUser\My hello a pak spusťte následující rutiny Azure Powershellu pro prostředí Azure (AzureCloud nebo AzureChinaCloud) hello:
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a>Přidat výpočetním uzlu virtuální počítače
Přidat výpočetní uzly s hello **přidat HpcIaaSNode.ps1** skriptu.

### <a name="syntax"></a>Syntaxe
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a>Parametry
* **ServiceName**: název hello Cloudová služba, která nový výpočet uzlu virtuální počítače jsou přidány do.
* **ImageName**: název bitové kopie virtuálního počítače Azure, který můžete získat prostřednictvím hello portál Azure classic nebo rutiny Azure Powershellu **Get-AzureVMImage**. Obrázek Hello musí splňovat následující požadavky hello:
  
  1. Musí být nainstalován operační systém Windows.
  2. HPC Pack musí být nainstalován v hello výpočetní uzel roli.
  3. Obrázek Hello musí být privátní bitové kopie v kategorii uživatelů hello, není veřejný image virtuálního počítače Azure.
* **Množství**: počet výpočetních uzlů virtuální počítače toobe přidat.
* **InstanceSize**: velikost hello výpočetní uzel virtuálních počítačů.
* **DomainUserName**: uživatelské jméno domény, který je použité toojoin hello nové virtuální počítače toohello domény.
* **DomainUserPassword**: heslo uživatele domény hello.
* **NodeNameSeries** (volitelné): pojmenování vzor pro hello výpočetních uzlů. musí být ve formátu Hello &lt; *kořenové\_název*&gt;&lt;*spustit\_číslo*&gt;%. Například MyCN % 10 % znamená řadu hello výpočetní uzel názvy od MyCN11. Pokud není zadaný, nakonfigurovat hello používá skript hello pojmenování řady uzlu v clusteru HPC hello.

### <a name="example"></a>Příklad
Hello následující příklad přidá 20 velikost velké výpočetním uzlu virtuální počítače v cloudové službě hello *hpcservice1*, podle image virtuálního počítače hello *hpccnimage1*.

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a>Odebrat výpočetním uzlu virtuální počítače
Odebrat výpočetní uzly s hello **odebrat HpcIaaSNode.ps1** skriptu.

### <a name="syntax"></a>Syntaxe
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a>Parametry
* **Název**: názvy toobe uzly clusteru odstraněny. Jsou podporovány zástupné znaky. Název sady parametr Hello je název. Nelze zadat obě hello **název** a **uzlu** parametry.
* **Uzel**: hello objekt HpcNode pro toobe hello uzly odebrány, který lze získat pomocí rutiny prostředí HPC PowerShell hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Název sady parametr Hello je uzel. Nelze zadat obě hello **název** a **uzlu** parametry.
* **DeleteVHD** (volitelné): nastavení toodelete hello přidružené disky pro virtuální počítače, které jsou odebrány hello.
* **Platnost** (volitelné): nastavení tooforce offline HPC uzly před jejich odebráním.
* **Potvrďte** (volitelné): výzva k potvrzení před provedením příkazu hello.
* **WhatIf**: nastavení toodescribe, co by se stalo, pokud proveden příkaz hello bez hello příkaz skutečně proveden.

### <a name="example"></a>Příklad
Hello následující příklad vynutí offline hello uzly s názvy od *HPCNode-CN -* a jejich odebere hello uzlů a jejich přidružené disky.

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a>Spuštění výpočetního uzlu virtuální počítače
Spuštění výpočetní uzly s hello **Start-HpcIaaSNode.ps1** skriptu.

### <a name="syntax"></a>Syntaxe
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a>Parametry
* **Název**: názvy toobe uzly clusteru hello spuštěna. Jsou podporovány zástupné znaky. Název sady parametr Hello je název. Nelze zadat obě hello **název** a **uzlu** parametry.
* **Uzel**-hello objekt HpcNode pro toobe uzly hello spustili, který lze získat pomocí rutiny prostředí HPC PowerShell hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Název sady parametr Hello je uzel. Nelze zadat obě hello **název** a **uzlu** parametry.

### <a name="example"></a>Příklad
Hello následující příklad spustí uzly s názvy od *HPCNode-CN -*.

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a>Zastavit výpočetním uzlu virtuální počítače
Zastavit výpočetní uzly s hello **Stop-HpcIaaSNode.ps1** skriptu.

### <a name="syntax"></a>Syntaxe
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a>Parametry
* **Název**-názvy toobe uzly clusteru hello zastavena. Jsou podporovány zástupné znaky. Název sady parametr Hello je název. Nelze zadat obě hello **název** a **uzlu** parametry.
* **Uzel**: hello objekt HpcNode pro toobe uzly hello zastavená, který lze získat pomocí rutiny prostředí HPC PowerShell hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx). Název sady parametr Hello je uzel. Nelze zadat obě hello **název** a **uzlu** parametry.
* **Platnost** (volitelné): nastavení tooforce offline HPC uzly před zastavením je.

### <a name="example"></a>Příklad
Hello následující příklad vynutí offline uzly s názvy od *HPCNode-CN -* a pak zastaví hello uzly.

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a>Další kroky
* tooautomatically zvětšit nebo zmenšit hello uzly clusteru podle aktuální zatížení hello úloh a úloh v hello clusteru najdete v tématu [automaticky zvětšovat a zmenšovat prostředky clusteru HPC Pack hello v Azure podle toohello zatížení clusteru](hpcpack-cluster-node-autogrowshrink.md).


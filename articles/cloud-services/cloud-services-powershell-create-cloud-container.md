---
title: "aaaCreate kontejner cloudové služby pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento článek vysvětluje, jak toocreate Cloudová služba kontejneru pomocí prostředí PowerShell. kontejner Hello hostuje webové a pracovní role."
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 4c47b10b5ba1f9cc39594a45cd869bf04fcaadf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-an-azure-powershell-command-toocreate-an-empty-cloud-service-container"></a>Použijte prostředí Azure PowerShell příkaz toocreate kontejner prázdný cloudové služby
Tento článek vysvětluje, jak tooquickly vytvořit kontejner cloudové služby pomocí rutin prostředí Azure PowerShell. Postupujte podle následujících kroků hello:

1. Nainstalovat rutiny Azure Powershellu Microsoft hello z hello [prostředí Azure PowerShell stáhne](http://aka.ms/webpi-azps) stránky.
2. Otevřete příkazový řádek Powershellu hello.
3. Použití hello [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) toosign v.

   > [!NOTE]
   > Další pokyny k instalaci rutiny Azure Powershellu hello a připojení tooyour předplatné Azure, najdete v části příliš[jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).
   >
   >
4. Použití hello **New-AzureService** rutiny toocreate kontejner prázdný Azure cloud service.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Použijte tuto ukázkovou rutinu tooinvoke hello:

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Další informace o vytváření hello cloudové služby Azure spusťte příkaz:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Další kroky
* toomanage hello cloudové nasazení služby, získáte toohello [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), a [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) příkazy. Můžete také použít příliš[jak tooconfigure cloudových služeb](cloud-services-how-to-configure.md) Další informace.
* toopublish cloudové služby projektu tooAzure získáte toohello **PublishCloudService.ps1** ukázka kódu z [nastavené průběžné doručování pro cloudové služby v Azure](cloud-services-dotnet-continuous-delivery.md).

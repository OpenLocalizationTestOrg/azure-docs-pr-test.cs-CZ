---
title: "Vyžadovat bezpečnému přenosu ve službě Azure Storage | Microsoft Docs"
description: "Další informace o funkci \"Vyžadovat bezpečnému přenosu\" pro Azure Storage a jak jej povolit."
services: storage
documentationcenter: na
author: fhryo-msft
manager: Jason.Hogg
editor: fhryo-msft
ms.assetid: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/20/2017
ms.author: fryu
ms.openlocfilehash: bc5b7fc79869c632db96958f17aaf953a5fd3b19
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="require-secure-transfer"></a>Vyžádání bezpečného přenosu

Možnost "Zabezpečení přenosu požadované" zvyšuje zabezpečení vašeho účtu úložiště tím, že pouze požadavky na účet úložiště z zabezpečené připojení. Například při volání rozhraní REST API pro přístup k účtu úložiště, je nutné připojit pomocí protokolu HTTPS. Všechny požadavky pomocí protokolu HTTP byly zamítnuty, pokud je povoleno "Zabezpečení přenosu požadované".

Pokud používáte službu Azure soubory, žádné připojení bez šifrování se nezdaří, pokud "Zabezpečení přenosu požadované" je povolena. To zahrnuje scénáře s využitím protokolu SMB 2.1, protokolu SMB 3.0 bez šifrování a některých typů klient Linux SMB. 

Ve výchozím nastavení je možnost "Zabezpečení přenosu požadované" zakázaná.

> [!NOTE]
> Vzhledem k tomu, že úložiště Azure nepodporuje protokol HTTPS pro vlastní názvy domén, není tato možnost použita při použití vlastního názvu domény.

## <a name="enable-secure-transfer-required-in-the-azure-portal"></a>Povolit "Zabezpečení přenosu požadované" na portálu Azure

"Zabezpečený přenos požadované" při vytvoření účtu úložiště v obě nastavení můžete povolit [portál Azure](https://portal.azure.com)a pro existující účty úložiště.

### <a name="require-secure-transfer-when-you-create-a-storage-account"></a>Po vytvoření účtu úložiště vyžadovat bezpečnému přenosu

1. Otevřete **vytvořit účet úložiště** okno na portálu Azure.
1. V části **zabezpečení přenosu požadované**, vyberte **povoleno**.

  ![snímek obrazovky](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Vyžadovat bezpečnému přenosu pro stávající účet úložiště

1. Vyberte stávající účet úložiště na portálu Azure.
1. Vyberte **konfigurace** pod **nastavení** v okně nabídce účtu úložiště.
1. V části **zabezpečení přenosu požadované**, vyberte **povoleno**.

  ![snímek obrazovky](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a>Povolit "Zabezpečení přenosu požadované" prostřednictvím kódu programu

Název nastavení je _supportsHttpsTrafficOnly_ ve vlastnosti účtu úložiště. Můžete povolit "Zabezpečený přenos požadované" nastavení pomocí rozhraní REST API, nástroje nebo knihovny:

* **Rozhraní REST API** (verze: 2016-12-01): [balíček verze](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)
* **Prostředí PowerShell** (verze: 4.1.0): [balíček verze](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)
* **Rozhraní příkazového řádku** (verze: 2.0.11): [balíček verze](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)
* **NodeJS** (verze: 1.1.0): [balíček verze](https://www.npmjs.com/package/azure-arm-storage/)
* **.NET SDK** (verze: 6.3.0): [balíček verze](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)
* **Python SDK** (verze: 1.1.0): [balíček verze](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)
* **Poznámky Ruby SDK** (verze: 0.11.0): [balíček verze](https://rubygems.org/gems/azure_mgmt_storage)

### <a name="enable-secure-transfer-required-setting-with-rest-api"></a>Povolit "Zabezpečený přenos požadované" nastavení pomocí rozhraní REST API

Pro zjednodušení testování pomocí rozhraní REST API, můžete použít [ArmClient](https://github.com/projectkudu/ARMClient) volat z příkazového řádku.

 Chcete nastavit pomocí rozhraní REST API můžete použít následující příkaz:

```
# Login Azure and proceed with your credentials
> armclient login

> armclient GET  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01
```

V odpovědi můžete najít _supportsHttpsTrafficOnly_ nastavení. Ukázka:

```Json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}",
  "kind": "Storage",
  ...
  "properties": {
    ...
    "supportsHttpsTrafficOnly": false
  },
  "type": "Microsoft.Storage/storageAccounts"
}
```

Povolit nastavení pomocí rozhraní REST API můžete použít následující příkaz:

```
# Login Azure and proceed with your credentials
> armclient login

> armclient PUT /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01 < Input.json
```
Ukázka Input.json:
```Json
{
  "location": "westus",
  "properties": {
    "supportsHttpsTrafficOnly": true
  }
}
```

## <a name="next-steps"></a>Další kroky
Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení, které společně umožňují vývojářům vytvářet aplikace zabezpečené. Další podrobnosti naleznete [Průvodce zabezpečením úložiště](storage-security-guide.md).

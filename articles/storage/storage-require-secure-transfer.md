---
title: "zabezpečení přenosu aaaRequire ve službě Azure Storage | Microsoft Docs"
description: "Další informace o funkci \"Vyžadovat bezpečnému přenosu\" hello pro Azure Storage a jak tooenable ho."
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
ms.openlocfilehash: 27f745c5e771b50213c1dbb39dee081947be1f39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="require-secure-transfer"></a><span data-ttu-id="bf276-103">Vyžádání bezpečného přenosu</span><span class="sxs-lookup"><span data-stu-id="bf276-103">Require secure transfer</span></span>

<span data-ttu-id="bf276-104">možnost Hello "zabezpečený přenos požadované" zvyšuje zabezpečení hello svého účtu úložiště tím, že pouze požadavky toohello účet úložiště z zabezpečené připojení.</span><span class="sxs-lookup"><span data-stu-id="bf276-104">hello "Secure transfer required" option enhances hello security of your storage account by only allowing requests toohello storage account from secure connections.</span></span> <span data-ttu-id="bf276-105">Například při volání rozhraní REST API tooaccess účtu úložiště, je nutné připojit pomocí protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bf276-105">For example, when calling REST APIs tooaccess your storage account, you must connect using HTTPS.</span></span> <span data-ttu-id="bf276-106">Všechny požadavky pomocí protokolu HTTP byly zamítnuty, pokud je povoleno "Zabezpečení přenosu požadované".</span><span class="sxs-lookup"><span data-stu-id="bf276-106">Any requests using HTTP are rejected when "Secure transfer required" is enabled.</span></span>

<span data-ttu-id="bf276-107">Pokud používáte službu Azure souborů hello, žádné připojení bez šifrování se nezdaří, pokud "Zabezpečení přenosu požadované" je povolena.</span><span class="sxs-lookup"><span data-stu-id="bf276-107">When you are using hello Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="bf276-108">To zahrnuje scénáře s využitím protokolu SMB 2.1, protokolu SMB 3.0 bez šifrování a některých typů hello klient Linux SMB.</span><span class="sxs-lookup"><span data-stu-id="bf276-108">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of hello Linux SMB client.</span></span> 

<span data-ttu-id="bf276-109">Ve výchozím nastavení hello "zabezpečený přenos požadované" možnost je vypnuta.</span><span class="sxs-lookup"><span data-stu-id="bf276-109">By default, hello "Secure transfer required" option is disabled.</span></span>

> [!NOTE]
> <span data-ttu-id="bf276-110">Vzhledem k tomu, že úložiště Azure nepodporuje protokol HTTPS pro vlastní názvy domén, není tato možnost použita při použití vlastního názvu domény.</span><span class="sxs-lookup"><span data-stu-id="bf276-110">Because Azure Storage doesn't support HTTPS for custom domain names, this option is not applied when using a custom domain name.</span></span>

## <a name="enable-secure-transfer-required-in-hello-azure-portal"></a><span data-ttu-id="bf276-111">Povolit "Požadované bezpečnému přenosu" v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bf276-111">Enable "Secure transfer required" in hello Azure portal</span></span>

<span data-ttu-id="bf276-112">Můžete povolit hello "zabezpečený přenos požadované" obě nastavení při vytváření účtu úložiště v hello [portál Azure](https://portal.azure.com)a pro existující účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf276-112">You can enable hello "Secure transfer required" setting both when you create a storage account in hello [Azure portal](https://portal.azure.com), and for existing storage accounts.</span></span>

### <a name="require-secure-transfer-when-you-create-a-storage-account"></a><span data-ttu-id="bf276-113">Po vytvoření účtu úložiště vyžadovat bezpečnému přenosu</span><span class="sxs-lookup"><span data-stu-id="bf276-113">Require secure transfer when you create a storage account</span></span>

1. <span data-ttu-id="bf276-114">Otevřete hello **vytvořit účet úložiště** okno v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bf276-114">Open hello **Create storage account** blade in hello Azure portal.</span></span>
1. <span data-ttu-id="bf276-115">V části **zabezpečení přenosu požadované**, vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="bf276-115">Under **Secure transfer required**, select **Enabled**.</span></span>

  ![snímek obrazovky](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a><span data-ttu-id="bf276-117">Vyžadovat bezpečnému přenosu pro stávající účet úložiště</span><span class="sxs-lookup"><span data-stu-id="bf276-117">Require secure transfer for an existing storage account</span></span>

1. <span data-ttu-id="bf276-118">Vyberte stávající účet úložiště v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bf276-118">Select an existing storage account in hello Azure portal.</span></span>
1. <span data-ttu-id="bf276-119">Vyberte **konfigurace** pod **nastavení** v okně nabídce účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="bf276-119">Select **Configuration** under **SETTINGS** in hello storage account menu blade.</span></span>
1. <span data-ttu-id="bf276-120">V části **zabezpečení přenosu požadované**, vyberte **povoleno**.</span><span class="sxs-lookup"><span data-stu-id="bf276-120">Under **Secure transfer required**, select **Enabled**.</span></span>

  ![snímek obrazovky](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="enable-secure-transfer-required-programmatically"></a><span data-ttu-id="bf276-122">Povolit "Zabezpečení přenosu požadované" prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="bf276-122">Enable "Secure transfer required" programmatically</span></span>

<span data-ttu-id="bf276-123">Název nastavení Hello je _supportsHttpsTrafficOnly_ ve vlastnosti účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="bf276-123">hello setting name is _supportsHttpsTrafficOnly_ in storage account properties.</span></span> <span data-ttu-id="bf276-124">Můžete povolit "Zabezpečený přenos požadované" nastavení pomocí rozhraní REST API, nástroje nebo knihovny:</span><span class="sxs-lookup"><span data-stu-id="bf276-124">You can enable "Secure transfer required" setting with REST API, tools or libraries:</span></span>

* <span data-ttu-id="bf276-125">**Rozhraní REST API** (verze: 2016-12-01): [balíček verze](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)</span><span class="sxs-lookup"><span data-stu-id="bf276-125">**REST API** (Version: 2016-12-01): [release package](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts)</span></span>
* <span data-ttu-id="bf276-126">**Prostředí PowerShell** (verze: 4.1.0): [balíček verze](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)</span><span class="sxs-lookup"><span data-stu-id="bf276-126">**PowerShell** (Version: 4.1.0): [release package](https://docs.microsoft.com/en-us/powershell/module/azurerm.storage/set-azurermstorageaccount?view=azurermps-4.1.0)</span></span>
* <span data-ttu-id="bf276-127">**Rozhraní příkazového řádku** (verze: 2.0.11): [balíček verze](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)</span><span class="sxs-lookup"><span data-stu-id="bf276-127">**CLI** (Version: 2.0.11): [release package](https://pypi.python.org/pypi/azure-cli-storage/2.0.11)</span></span>
* <span data-ttu-id="bf276-128">**NodeJS** (verze: 1.1.0): [balíček verze](https://www.npmjs.com/package/azure-arm-storage/)</span><span class="sxs-lookup"><span data-stu-id="bf276-128">**NodeJS** (Version: 1.1.0): [release package](https://www.npmjs.com/package/azure-arm-storage/)</span></span>
* <span data-ttu-id="bf276-129">**.NET SDK** (verze: 6.3.0): [balíček verze](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)</span><span class="sxs-lookup"><span data-stu-id="bf276-129">**.NET SDK** (Version: 6.3.0): [release package](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/6.3.0-preview)</span></span>
* <span data-ttu-id="bf276-130">**Python SDK** (verze: 1.1.0): [balíček verze](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)</span><span class="sxs-lookup"><span data-stu-id="bf276-130">**Python SDK** (Version: 1.1.0): [release package](https://pypi.python.org/pypi/azure-mgmt-storage/1.1.0)</span></span>
* <span data-ttu-id="bf276-131">**Poznámky Ruby SDK** (verze: 0.11.0): [balíček verze](https://rubygems.org/gems/azure_mgmt_storage)</span><span class="sxs-lookup"><span data-stu-id="bf276-131">**Ruby SDK** (Version: 0.11.0): [release package](https://rubygems.org/gems/azure_mgmt_storage)</span></span>

### <a name="enable-secure-transfer-required-setting-with-rest-api"></a><span data-ttu-id="bf276-132">Povolit "Zabezpečený přenos požadované" nastavení pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="bf276-132">Enable "Secure transfer required" setting with REST API</span></span>

<span data-ttu-id="bf276-133">toosimplify testování pomocí rozhraní REST API, můžete použít [ArmClient](https://github.com/projectkudu/ARMClient) toocall z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="bf276-133">toosimplify testing with REST API, you can use [ArmClient](https://github.com/projectkudu/ARMClient) toocall from command line.</span></span>

 <span data-ttu-id="bf276-134">Můžete použít následující nastavení hello toocheck příkazového řádku s hello REST API:</span><span class="sxs-lookup"><span data-stu-id="bf276-134">You can use below command line toocheck hello setting with hello REST API:</span></span>

```
# Login Azure and proceed with your credentials
> armclient login

> armclient GET  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01
```

<span data-ttu-id="bf276-135">V hello odpovědi můžete najít _supportsHttpsTrafficOnly_ nastavení.</span><span class="sxs-lookup"><span data-stu-id="bf276-135">In hello response, you can find _supportsHttpsTrafficOnly_ setting.</span></span> <span data-ttu-id="bf276-136">Ukázka:</span><span class="sxs-lookup"><span data-stu-id="bf276-136">Sample:</span></span>

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

<span data-ttu-id="bf276-137">Můžete použít následující nastavení hello tooenable příkazového řádku s hello REST API:</span><span class="sxs-lookup"><span data-stu-id="bf276-137">You can use below command line tooenable hello setting with hello REST API:</span></span>

```
# Login Azure and proceed with your credentials
> armclient login

> armclient PUT /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}?api-version=2016-12-01 < Input.json
```
<span data-ttu-id="bf276-138">Ukázka Input.json:</span><span class="sxs-lookup"><span data-stu-id="bf276-138">Sample of Input.json:</span></span>
```Json
{
  "location": "westus",
  "properties": {
    "supportsHttpsTrafficOnly": true
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="bf276-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf276-139">Next steps</span></span>
<span data-ttu-id="bf276-140">Úložiště Azure poskytuje komplexní sadu funkcí zabezpečení, které společně umožňují vývojářům toobuild zabezpečených aplikací.</span><span class="sxs-lookup"><span data-stu-id="bf276-140">Azure Storage provides a comprehensive set of security capabilities, which together enable developers toobuild secure applications.</span></span> <span data-ttu-id="bf276-141">Další podrobnosti najdete v článku hello [Průvodce zabezpečením úložiště](storage-security-guide.md).</span><span class="sxs-lookup"><span data-stu-id="bf276-141">For more details, visit hello [Storage Security Guide](storage-security-guide.md).</span></span>

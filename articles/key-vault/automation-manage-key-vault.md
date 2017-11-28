---
title: "aaaManage Azure Key Vault pomocí Azure Automation | Microsoft Docs"
description: "Informace o tom, jak hello služby Azure Automation lze použít toomanage Azure Key Vault."
services: Key-Vault, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 4e780762-19b6-4ca6-b894-ebb44c538f35
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte;csand
ms.openlocfilehash: 7f46ecc1206a96e8aeb1d086285461cb5b205472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a><span data-ttu-id="82acb-103">Správa Azure Key Vault pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="82acb-103">Managing Azure Key Vault using Azure Automation</span></span>
<span data-ttu-id="82acb-104">Tento průvodce vás seznámí toohello služba Azure Automation a jak může být použité toosimplify správy klíčů a tajných klíčů v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="82acb-104">This guide will introduce you toohello Azure Automation service and how it can be used toosimplify management of your keys and secrets in Azure Key Vault.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="82acb-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="82acb-105">What is Azure Automation?</span></span>
<span data-ttu-id="82acb-106">[Služby Azure Automation](../automation/automation-intro.md) je služba Azure pro zjednodušenou správu cloudu pomocí automatizace procesů a konfigurace požadovaného stavu.</span><span class="sxs-lookup"><span data-stu-id="82acb-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation and desired state configuration.</span></span> <span data-ttu-id="82acb-107">Pomocí Azure Automation, může být ruční, opakované, dlouhotrvajících a náchylné úlohy automatizované tooincrease spolehlivost, efektivitu a toovalue čas pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="82acb-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="82acb-108">Azure Automation nabízí modulu provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, která je škálovatelná toomeet vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="82acb-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="82acb-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="82acb-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="82acb-110">Snížit provozní režie a uvolněte IT a spustit automaticky DevOps zaměstnanci toofocus na práci, kterou přidá obchodní hodnotu přesunutím vaší toobe úlohy správy cloudu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="82acb-110">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a><span data-ttu-id="82acb-111">Jak Azure Automation pomoci spravovat Azure Key Vault?</span><span class="sxs-lookup"><span data-stu-id="82acb-111">How can Azure Automation help manage Azure Key Vault?</span></span>
<span data-ttu-id="82acb-112">Key Vault můžete spravovat ve službě Azure Automation pomocí hello [rutin AzureRM Key Vault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) a [rutin Azure Classic Key Vault](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span><span class="sxs-lookup"><span data-stu-id="82acb-112">Key Vault can be managed in Azure Automation by using hello [AzureRM Key Vault cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) and [Azure Classic Key Vault cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span></span> <span data-ttu-id="82acb-113">Hello Azure modul pro správu classic Key Vault je dostupný automaticky ve službě Azure Automation a můžete importovat hello [AzureRM KeyVault modulu](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) do Azure Automation, aby mohli provést řadu vaší správy Key Vault úlohy v rámci služby hello.</span><span class="sxs-lookup"><span data-stu-id="82acb-113">hello Azure module for managing classic Key Vault is available automatically in Azure Automation, and you can import hello [AzureRM-KeyVault module](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) into Azure Automation, so that you can perform many of your Key Vault management tasks within hello service.</span></span> <span data-ttu-id="82acb-114">Tyto rutiny ve službě Azure Automation pomocí hello rutin pro ostatní služby Azure, tooautomate složité úlohy může také párovat napříč službami Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="82acb-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="82acb-115">Pomocí rutiny Azure Key Vault hello můžete provádět tyto úlohy mimo jiné:</span><span class="sxs-lookup"><span data-stu-id="82acb-115">With hello Azure Key Vault cmdlets you can perform these tasks among others:</span></span> 

* <span data-ttu-id="82acb-116">Vytvoření a konfigurace trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="82acb-116">Create and configure a key vault</span></span>
* <span data-ttu-id="82acb-117">Vytvoření nebo import klíče</span><span class="sxs-lookup"><span data-stu-id="82acb-117">Create or import a key</span></span>
* <span data-ttu-id="82acb-118">Vytvořit nebo aktualizovat tajný klíč</span><span class="sxs-lookup"><span data-stu-id="82acb-118">Create or update a secret</span></span>
* <span data-ttu-id="82acb-119">Aktualizovat atributy klíče</span><span class="sxs-lookup"><span data-stu-id="82acb-119">Update attributes of a key</span></span>
* <span data-ttu-id="82acb-120">Získat klíč nebo tajný klíč</span><span class="sxs-lookup"><span data-stu-id="82acb-120">Get a key or secret</span></span>
* <span data-ttu-id="82acb-121">Odstranění klíče nebo tajného klíče</span><span class="sxs-lookup"><span data-stu-id="82acb-121">Delete a key or secret</span></span>

<span data-ttu-id="82acb-122">Zde je několik příkladů použití prostředí PowerShell toomanage Key Vault:</span><span class="sxs-lookup"><span data-stu-id="82acb-122">Here are some examples of using PowerShell toomanage Key Vault:</span></span>  

* [<span data-ttu-id="82acb-123">Azure Key Vault - krok za krokem</span><span class="sxs-lookup"><span data-stu-id="82acb-123">Azure Key Vault - Step by Step</span></span>](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [<span data-ttu-id="82acb-124">Nastavení a konfiguraci Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="82acb-124">Setting Up and Configuring an Azure Key Vault</span></span>](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a><span data-ttu-id="82acb-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="82acb-125">Next steps</span></span>
<span data-ttu-id="82acb-126">Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage Azure Key Vault, postupujte podle těchto odkazů toolearn Další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="82acb-126">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure Key Vault, follow these links toolearn more about Azure Automation.</span></span>

* <span data-ttu-id="82acb-127">V tématu hello Azure Automation [kurzu Začínáme](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="82acb-127">See hello Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md).</span></span>
* <span data-ttu-id="82acb-128">V tématu hello [Azure Key Vault PowerShell skripty](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span><span class="sxs-lookup"><span data-stu-id="82acb-128">See hello [Azure Key Vault PowerShell scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span></span>


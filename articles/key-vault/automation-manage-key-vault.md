---
title: "Správa Azure Key Vault pomocí Azure Automation | Microsoft Docs"
description: "Další informace o používání služby Azure Automation pro správu Azure Key Vault."
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
ms.openlocfilehash: dee39662472fe54776b591977f2b1ecb39d15b00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a><span data-ttu-id="85d1d-103">Správa Azure Key Vault pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="85d1d-103">Managing Azure Key Vault using Azure Automation</span></span>
<span data-ttu-id="85d1d-104">Tento průvodce vás seznámí s služba Azure Automation a jak může sloužit ke zjednodušení správy klíčů a tajných klíčů v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="85d1d-104">This guide will introduce you to the Azure Automation service and how it can be used to simplify management of your keys and secrets in Azure Key Vault.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="85d1d-105">Co je Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="85d1d-105">What is Azure Automation?</span></span>
<span data-ttu-id="85d1d-106">[Služby Azure Automation](../automation/automation-intro.md) je služba Azure pro zjednodušenou správu cloudu pomocí automatizace procesů a konfigurace požadovaného stavu.</span><span class="sxs-lookup"><span data-stu-id="85d1d-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation and desired state configuration.</span></span> <span data-ttu-id="85d1d-107">Pomocí Azure Automation, ruční, opakované, dlouhotrvajících a k chybám úloh je možné automatizovat zvýšit spolehlivost, efektivitu a času na hodnotu pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="85d1d-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="85d1d-108">Azure Automation nabízí modul provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, který rozšiřuje podle vašich potřeb.</span><span class="sxs-lookup"><span data-stu-id="85d1d-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="85d1d-109">Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="85d1d-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="85d1d-110">Snížit provozní režie a uvolněte IT a zaměstnanci DevOps a zaměřit se na práci, kterou přidá obchodní hodnotu přesunutím vašeho cloudu spuštění úloh správy se automaticky automatizace Azure.</span><span class="sxs-lookup"><span data-stu-id="85d1d-110">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a><span data-ttu-id="85d1d-111">Jak Azure Automation pomoci spravovat Azure Key Vault?</span><span class="sxs-lookup"><span data-stu-id="85d1d-111">How can Azure Automation help manage Azure Key Vault?</span></span>
<span data-ttu-id="85d1d-112">Key Vault můžete spravovat ve službě Azure Automation pomocí [rutin AzureRM Key Vault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) a [rutin Azure Classic Key Vault](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span><span class="sxs-lookup"><span data-stu-id="85d1d-112">Key Vault can be managed in Azure Automation by using the [AzureRM Key Vault cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) and [Azure Classic Key Vault cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).</span></span> <span data-ttu-id="85d1d-113">Modul Azure pro správu classic Key Vault je k dispozici automaticky ve službě Azure Automation a můžete importovat [AzureRM KeyVault modulu](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) do Azure Automation, tak, aby bylo možné provádět mnoho úloh správy vaší Key Vault v rámci služby.</span><span class="sxs-lookup"><span data-stu-id="85d1d-113">The Azure module for managing classic Key Vault is available automatically in Azure Automation, and you can import the [AzureRM-KeyVault module](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) into Azure Automation, so that you can perform many of your Key Vault management tasks within the service.</span></span> <span data-ttu-id="85d1d-114">Může také párovat tyto rutiny ve službě Azure Automation pomocí rutin pro jinými službami Azure, na automatizují komplexní úlohy v služeb Azure a systémech 3. stran.</span><span class="sxs-lookup"><span data-stu-id="85d1d-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="85d1d-115">Pomocí rutiny Azure Key Vault můžete provádět tyto úlohy mimo jiné:</span><span class="sxs-lookup"><span data-stu-id="85d1d-115">With the Azure Key Vault cmdlets you can perform these tasks among others:</span></span> 

* <span data-ttu-id="85d1d-116">Vytvoření a konfigurace trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="85d1d-116">Create and configure a key vault</span></span>
* <span data-ttu-id="85d1d-117">Vytvoření nebo import klíče</span><span class="sxs-lookup"><span data-stu-id="85d1d-117">Create or import a key</span></span>
* <span data-ttu-id="85d1d-118">Vytvořit nebo aktualizovat tajný klíč</span><span class="sxs-lookup"><span data-stu-id="85d1d-118">Create or update a secret</span></span>
* <span data-ttu-id="85d1d-119">Aktualizovat atributy klíče</span><span class="sxs-lookup"><span data-stu-id="85d1d-119">Update attributes of a key</span></span>
* <span data-ttu-id="85d1d-120">Získat klíč nebo tajný klíč</span><span class="sxs-lookup"><span data-stu-id="85d1d-120">Get a key or secret</span></span>
* <span data-ttu-id="85d1d-121">Odstranění klíče nebo tajného klíče</span><span class="sxs-lookup"><span data-stu-id="85d1d-121">Delete a key or secret</span></span>

<span data-ttu-id="85d1d-122">Tady jsou některé příklady použití Powershellu ke správě Key Vault:</span><span class="sxs-lookup"><span data-stu-id="85d1d-122">Here are some examples of using PowerShell to manage Key Vault:</span></span>  

* [<span data-ttu-id="85d1d-123">Azure Key Vault - krok za krokem</span><span class="sxs-lookup"><span data-stu-id="85d1d-123">Azure Key Vault - Step by Step</span></span>](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [<span data-ttu-id="85d1d-124">Nastavení a konfiguraci Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="85d1d-124">Setting Up and Configuring an Azure Key Vault</span></span>](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a><span data-ttu-id="85d1d-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="85d1d-125">Next steps</span></span>
<span data-ttu-id="85d1d-126">Teď, když jste se naučili základy Azure Automation a jak může sloužit ke správě Azure Key Vault, postupujte podle následujících odkazech na další informace o Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="85d1d-126">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure Key Vault, follow these links to learn more about Azure Automation.</span></span>

* <span data-ttu-id="85d1d-127">Najdete v části služby Azure Automation [kurz Začínáme](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="85d1d-127">See the Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md).</span></span>
* <span data-ttu-id="85d1d-128">Najdete v článku [Azure Key Vault PowerShell skripty](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span><span class="sxs-lookup"><span data-stu-id="85d1d-128">See the [Azure Key Vault PowerShell scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).</span></span>


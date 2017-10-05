---
title: "Nastavení prostředí PowerShell pro vytvoření virtuálního počítače pro Marketplace | Microsoft Docs"
description: "Pokyny pro nastavení prostředí Azure PowerShell a jeho použití jako volitelný proces toku k vytvoření Image virtuálních počítačů k nasazení a prodeje na Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e19d6cda-76df-4e42-be84-c9fe47a636db
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2016
ms.author: hascipio
ms.openlocfilehash: bbcce5093d2bbd5326523063db7d0e565fe4de6d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a><span data-ttu-id="ab4c6-103">Nastavení prostředí Azure PowerShell k vytvoření nabídky pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="ab4c6-103">Set up Azure PowerShell to create an offer for the Azure Marketplace</span></span>
<span data-ttu-id="ab4c6-104">Podrobné informace o tom, jak nastavit prostředí PowerShell v Azure najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ab4c6-104">For detailed information on how to set up PowerShell in Azure, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="ab4c6-105">Jednoduché možných přístupů je použít metody certifikátu, která se stáhne a importuje certifikát potřebný pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="ab4c6-105">A simple approach is to use the certificate method, which downloads and imports a certificate needed for authentication.</span></span> <span data-ttu-id="ab4c6-106">Chcete-li získat potřebný certifikát, použijte **Get-AzurePublishSettingsFile** rutiny.</span><span class="sxs-lookup"><span data-stu-id="ab4c6-106">To obtain the needed certificate, use the **Get-AzurePublishSettingsFile** cmdlet.</span></span> <span data-ttu-id="ab4c6-107">Když se zobrazí výzva, uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="ab4c6-107">Save the file when you're prompted.</span></span> <span data-ttu-id="ab4c6-108">Chcete-li import certifikátu do relace prostředí PowerShell, použijte **Import AzurePublishSettingsFile** rutiny.</span><span class="sxs-lookup"><span data-stu-id="ab4c6-108">To import the certificate into a PowerShell session, use the **Import-AzurePublishSettingsFile** cmdlet.</span></span>

<span data-ttu-id="ab4c6-109">Chcete-li konfigurace a uložení běžných nastavení odběru Microsoft Azure pro relace prostředí PowerShell, použijte **Set-AzureSubscription** a **Select-AzureSubscription** rutiny:</span><span class="sxs-lookup"><span data-stu-id="ab4c6-109">To configure and store the common Microsoft Azure subscription settings for the PowerShell session, use the **Set-AzureSubscription** and **Select-AzureSubscription** cmdlets:</span></span>

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

<span data-ttu-id="ab4c6-110">První příkaz přidruží výchozí účet úložiště s předplatným (vyžadována pro některé operace zřizování virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="ab4c6-110">The first command associates a default storage account with the subscription (needed for some VM provisioning operations).</span></span>  <span data-ttu-id="ab4c6-111">Druhá je předplatné stávající (rozpoznán jinými rutinami).</span><span class="sxs-lookup"><span data-stu-id="ab4c6-111">The second makes the subscription the current one (recognized by other cmdlets).</span></span>

## <a name="see-also"></a><span data-ttu-id="ab4c6-112">Viz také</span><span class="sxs-lookup"><span data-stu-id="ab4c6-112">See also</span></span>
* [<span data-ttu-id="ab4c6-113">Začínáme: postup publikování nabídky pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="ab4c6-113">Getting started: How to publish an offer to the Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)
* [<span data-ttu-id="ab4c6-114">Vytváření bitové kopie virtuálního počítače pro Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ab4c6-114">Creating a virtual machine image for the Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)


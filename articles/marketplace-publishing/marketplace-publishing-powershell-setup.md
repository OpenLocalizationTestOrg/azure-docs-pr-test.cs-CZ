---
title: "aaaSet až toocreate prostředí PowerShell virtuálního počítače pro hello Marketplace | Microsoft Docs"
description: "Pokyny pro nastavení prostředí Azure PowerShell a jeho použití jako volitelný proces toku toodeploy bitové kopie toocreate virtuálních počítačů a prodeje na, hello Azure Marketplace"
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
ms.openlocfilehash: cd2ebad7472248b8f921706e1a8c82d41f33b9cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-powershell-toocreate-an-offer-for-hello-azure-marketplace"></a><span data-ttu-id="0b28a-103">Nastavení prostředí Azure PowerShell toocreate nabídku hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="0b28a-103">Set up Azure PowerShell toocreate an offer for hello Azure Marketplace</span></span>
<span data-ttu-id="0b28a-104">Podrobné informace o tom, tooset až prostředí PowerShell v Azure, najdete v části [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0b28a-104">For detailed information on how tooset up PowerShell in Azure, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="0b28a-105">Jednoduchý přístup je toouse hello certifikát metodu, která se stáhne a importuje certifikát potřebný pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="0b28a-105">A simple approach is toouse hello certificate method, which downloads and imports a certificate needed for authentication.</span></span> <span data-ttu-id="0b28a-106">tooobtain hello potřeby certifikát, použijte hello **Get-AzurePublishSettingsFile** rutiny.</span><span class="sxs-lookup"><span data-stu-id="0b28a-106">tooobtain hello needed certificate, use hello **Get-AzurePublishSettingsFile** cmdlet.</span></span> <span data-ttu-id="0b28a-107">Když se zobrazí výzva, uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="0b28a-107">Save hello file when you're prompted.</span></span> <span data-ttu-id="0b28a-108">certifikát hello tooimport do relace prostředí PowerShell, použijte hello **Import AzurePublishSettingsFile** rutiny.</span><span class="sxs-lookup"><span data-stu-id="0b28a-108">tooimport hello certificate into a PowerShell session, use hello **Import-AzurePublishSettingsFile** cmdlet.</span></span>

<span data-ttu-id="0b28a-109">tooconfigure a úložiště hello běžné Microsoft Azure předplatné nastavení pro relaci prostředí PowerShell hello používat hello **Set-AzureSubscription** a **Select-AzureSubscription** rutiny:</span><span class="sxs-lookup"><span data-stu-id="0b28a-109">tooconfigure and store hello common Microsoft Azure subscription settings for hello PowerShell session, use hello **Set-AzureSubscription** and **Select-AzureSubscription** cmdlets:</span></span>

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

<span data-ttu-id="0b28a-110">První příkaz Hello přidruží výchozí účet úložiště s předplatným hello (vyžadována pro některé operace zřizování virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="0b28a-110">hello first command associates a default storage account with hello subscription (needed for some VM provisioning operations).</span></span>  <span data-ttu-id="0b28a-111">Hello druhý díky hello předplatné hello stávající (rozpoznán jinými rutinami).</span><span class="sxs-lookup"><span data-stu-id="0b28a-111">hello second makes hello subscription hello current one (recognized by other cmdlets).</span></span>

## <a name="see-also"></a><span data-ttu-id="0b28a-112">Viz také</span><span class="sxs-lookup"><span data-stu-id="0b28a-112">See also</span></span>
* [<span data-ttu-id="0b28a-113">Začínáme: jak toopublish toohello nabídka Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="0b28a-113">Getting started: How toopublish an offer toohello Azure Marketplace</span></span>](marketplace-publishing-getting-started.md)
* [<span data-ttu-id="0b28a-114">Vytváření bitové kopie virtuálního počítače pro hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0b28a-114">Creating a virtual machine image for hello Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)


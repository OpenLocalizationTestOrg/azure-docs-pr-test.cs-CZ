---
title: "Nainstalovat bránu dat místní | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat bránu místní data."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/22/2017
ms.author: owend
ms.openlocfilehash: 6ef296fb98478be9240f0231c8ad39cd2a0af995
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a><span data-ttu-id="b76b5-103">Nainstalujte a nakonfigurujte bránu místní data</span><span class="sxs-lookup"><span data-stu-id="b76b5-103">Install and configure an on-premises data gateway</span></span>
<span data-ttu-id="b76b5-104">Místní brána dat je potřeba při jeden nebo více serverů ve stejné oblasti Azure Analysis Services připojení ke zdrojům dat v místě.</span><span class="sxs-lookup"><span data-stu-id="b76b5-104">An on-premises data gateway is required when one or more Azure Analysis Services servers in the same region connect to on-premises data sources.</span></span> <span data-ttu-id="b76b5-105">Další informace o bráně najdete v tématu [místní brána dat](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="b76b5-105">To learn more about the gateway, see [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b76b5-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b76b5-106">Prerequisites</span></span>
<span data-ttu-id="b76b5-107">**Minimální požadavky:**</span><span class="sxs-lookup"><span data-stu-id="b76b5-107">**Minimum Requirements:**</span></span>

* <span data-ttu-id="b76b5-108">Rozhraní .NET 4.5 framework</span><span class="sxs-lookup"><span data-stu-id="b76b5-108">.NET 4.5 Framework</span></span>
* <span data-ttu-id="b76b5-109">64bitová verze systému Windows 7 nebo Windows Server 2008 R2 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="b76b5-109">64-bit version of Windows 7 / Windows Server 2008 R2 (or later)</span></span>

<span data-ttu-id="b76b5-110">**Doporučujeme:**</span><span class="sxs-lookup"><span data-stu-id="b76b5-110">**Recommended:**</span></span>

* <span data-ttu-id="b76b5-111">8 jader procesoru</span><span class="sxs-lookup"><span data-stu-id="b76b5-111">8 Core CPU</span></span>
* <span data-ttu-id="b76b5-112">8 GB paměti</span><span class="sxs-lookup"><span data-stu-id="b76b5-112">8 GB Memory</span></span>
* <span data-ttu-id="b76b5-113">64bitová verze systému Windows 2012 R2 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="b76b5-113">64-bit version of Windows 2012 R2 (or later)</span></span>

<span data-ttu-id="b76b5-114">**Důležité informace:**</span><span class="sxs-lookup"><span data-stu-id="b76b5-114">**Important considerations:**</span></span>

* <span data-ttu-id="b76b5-115">Během instalace, při registraci brány na Azure je vybrán výchozí oblasti pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="b76b5-115">During setup, when registering your gateway with Azure, the default region for your subscription is selected.</span></span> <span data-ttu-id="b76b5-116">Můžete použít v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="b76b5-116">You can choose a different region.</span></span> <span data-ttu-id="b76b5-117">Pokud máte servery ve více než jedné oblasti, je nutné nainstalovat bránu pro každou oblast.</span><span class="sxs-lookup"><span data-stu-id="b76b5-117">If you have servers in more than one region, you must install a gateway for each region.</span></span> 
* <span data-ttu-id="b76b5-118">Brána nemůže být nainstalována na řadiči domény.</span><span class="sxs-lookup"><span data-stu-id="b76b5-118">The gateway cannot be installed on a domain controller.</span></span>
* <span data-ttu-id="b76b5-119">V jednom počítači lze nainstalovat pouze jedna brána.</span><span class="sxs-lookup"><span data-stu-id="b76b5-119">Only one gateway can be installed on a single computer.</span></span>
* <span data-ttu-id="b76b5-120">Bránu nainstalujte na počítač, který zůstává na a není přejít do režimu spánku.</span><span class="sxs-lookup"><span data-stu-id="b76b5-120">Install the gateway on a computer that remains on and does not go to sleep.</span></span>
* <span data-ttu-id="b76b5-121">Neinstalujte bránu v počítači se bezdrátově připojený k síti.</span><span class="sxs-lookup"><span data-stu-id="b76b5-121">Do not install the gateway on a computer wirelessly connected to your network.</span></span> <span data-ttu-id="b76b5-122">Výkon může být snížena.</span><span class="sxs-lookup"><span data-stu-id="b76b5-122">Performance can be diminished.</span></span>


## <span data-ttu-id="b76b5-123"><a name="download"></a>Stahování</span><span class="sxs-lookup"><span data-stu-id="b76b5-123"><a name="download"></a>Download</span></span>
 [<span data-ttu-id="b76b5-124">Stáhněte bránu</span><span class="sxs-lookup"><span data-stu-id="b76b5-124">Download the gateway</span></span>](https://aka.ms/azureasgateway)

## <span data-ttu-id="b76b5-125"><a name="install"></a>Instalace</span><span class="sxs-lookup"><span data-stu-id="b76b5-125"><a name="install"></a>Install</span></span>

1. <span data-ttu-id="b76b5-126">Spusťte instalační program.</span><span class="sxs-lookup"><span data-stu-id="b76b5-126">Run setup.</span></span>

2. <span data-ttu-id="b76b5-127">Vyberte umístění, přijměte podmínky a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="b76b5-127">Select a location, accept the terms, and then click **Install**.</span></span>

   ![Nainstalujte umístění a licenční podmínky](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. <span data-ttu-id="b76b5-129">Vyberte **místní brána dat (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="b76b5-129">Select **On-premises data gateway (recommended)**.</span></span> <span data-ttu-id="b76b5-130">Azure Analysis Services nepodporuje osobní režimu.</span><span class="sxs-lookup"><span data-stu-id="b76b5-130">Azure Analysis Services does not support personal mode.</span></span>

   ![Vyberte typ brány](media/analysis-services-gateway-install/aas-gateway-installer-shared.png)

4. <span data-ttu-id="b76b5-132">Zadejte účet pro přihlášení k Azure.</span><span class="sxs-lookup"><span data-stu-id="b76b5-132">Enter an account to sign in to Azure.</span></span> <span data-ttu-id="b76b5-133">Účet musí být v vašeho klienta Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b76b5-133">The account must be in your tenant's Azure Active Directory.</span></span> <span data-ttu-id="b76b5-134">Tento účet slouží pro správce brány.</span><span class="sxs-lookup"><span data-stu-id="b76b5-134">This account is used for the gateway administrator.</span></span> 

   ![Zadejte účet pro přihlášení k Azure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > <span data-ttu-id="b76b5-136">Pokud se přihlásíte pomocí účtu domény, ji budou mapována na váš účet organizace v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b76b5-136">If you sign in with a domain account, it will be mapped to your organizational account in Azure AD.</span></span> <span data-ttu-id="b76b5-137">Váš účet organizace se použije jako správce brány.</span><span class="sxs-lookup"><span data-stu-id="b76b5-137">Your organizational account will be used as the the gateway administrator.</span></span>

## <span data-ttu-id="b76b5-138"><a name="register"></a>Registrace</span><span class="sxs-lookup"><span data-stu-id="b76b5-138"><a name="register"></a>Register</span></span>
<span data-ttu-id="b76b5-139">Chcete-li vytvořit bránu prostředků v Azure, je nutné zaregistrovat místní instance, kterou jste nainstalovali s cloudové službě brány.</span><span class="sxs-lookup"><span data-stu-id="b76b5-139">In order to create a gateway resource in Azure, you must register the local instance you installed with the Gateway Cloud Service.</span></span> 

1.  <span data-ttu-id="b76b5-140">Vyberte **registrace nové brány na tomto počítači**.</span><span class="sxs-lookup"><span data-stu-id="b76b5-140">Select **Register a new gateway on this computer**.</span></span>

    ![Registrace](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. <span data-ttu-id="b76b5-142">Zadejte název a obnovení klíče pro bránu.</span><span class="sxs-lookup"><span data-stu-id="b76b5-142">Type a name and recovery key for your gateway.</span></span> <span data-ttu-id="b76b5-143">Ve výchozím nastavení brána používá výchozí oblasti vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="b76b5-143">By default, the gateway uses your subscription's default region.</span></span> <span data-ttu-id="b76b5-144">Pokud je nutné vybrat jiné oblasti, vyberte **změnu oblast**.</span><span class="sxs-lookup"><span data-stu-id="b76b5-144">If you need to select a different region, select **Change Region**.</span></span>

   ![Registrace](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <span data-ttu-id="b76b5-146"><a name="create-resource"></a>Vytvořte prostředek Azure brány</span><span class="sxs-lookup"><span data-stu-id="b76b5-146"><a name="create-resource"></a>Create an Azure gateway resource</span></span>
<span data-ttu-id="b76b5-147">Po instalaci a zaregistrovat bránu, musíte vytvořit prostředek brány ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="b76b5-147">After you've installed and registered your gateway, you need to create a gateway resource in your Azure subscription.</span></span> <span data-ttu-id="b76b5-148">Přihlaste se k Azure pomocí stejného účtu, který jste použili při registraci brány.</span><span class="sxs-lookup"><span data-stu-id="b76b5-148">Sign in to Azure with the same account you used when registering the gateway.</span></span>

1. <span data-ttu-id="b76b5-149">Na portálu Azure, klikněte na tlačítko **vytvoření nové služby** > **Enterprise integrace** > **místní brána dat** > **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b76b5-149">In Azure portal, click **Create a new service** > **Enterprise Integration** > **On-premises data gateway** > **Create**.</span></span>

   ![Vytvořte prostředek brány](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. <span data-ttu-id="b76b5-151">V **vytvořit připojení bránu**, zadejte tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="b76b5-151">In **Create connection gateway**, enter these settings:</span></span>

    * <span data-ttu-id="b76b5-152">**Název**: Zadejte název pro prostředek brány.</span><span class="sxs-lookup"><span data-stu-id="b76b5-152">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="b76b5-153">**Předplatné**: Vyberte předplatné Azure, které chcete přidružit k prostředku brány.</span><span class="sxs-lookup"><span data-stu-id="b76b5-153">**Subscription**: Select the Azure subscription to associate with your gateway resource.</span></span> 
    <span data-ttu-id="b76b5-154">Tento odběr musí být stejné předplatné, které jsou vaše servery v.</span><span class="sxs-lookup"><span data-stu-id="b76b5-154">This subscription should be the same subscription your servers are in.</span></span>
   
      <span data-ttu-id="b76b5-155">Výchozí předplatné je založena na účet Azure, který jste použili k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b76b5-155">The default subscription is based on the Azure account that you used to sign in.</span></span>

    * <span data-ttu-id="b76b5-156">**Skupina prostředků**: Vytvořte skupinu prostředků, nebo vyberte existující.</span><span class="sxs-lookup"><span data-stu-id="b76b5-156">**Resource group**: Create a resource group or select an existing resource group.</span></span>

    * <span data-ttu-id="b76b5-157">**Umístění**: Vyberte oblast zaregistrovat bránu v.</span><span class="sxs-lookup"><span data-stu-id="b76b5-157">**Location**: Select the region you registered your gateway in.</span></span>

    * <span data-ttu-id="b76b5-158">**Název instalace**: Pokud vaše instalace brány již není vybrána, vyberte bránu zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="b76b5-158">**Installation Name**: If your gateway installation isn't already selected, select the gateway registered.</span></span> 

    <span data-ttu-id="b76b5-159">Když jste hotovi, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b76b5-159">When you're done, click **Create**.</span></span>

## <span data-ttu-id="b76b5-160"><a name="connect-servers"></a>Připojte servery k prostředku brány</span><span class="sxs-lookup"><span data-stu-id="b76b5-160"><a name="connect-servers"></a>Connect servers to the gateway resource</span></span>

1. <span data-ttu-id="b76b5-161">V přehledu vaší služby Azure Analysis Services serveru, klikněte na tlačítko **místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="b76b5-161">In your Azure Analysis Services server overview, click **On-Premises Data Gateway**.</span></span>

   ![Připojení serveru k bráně](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. <span data-ttu-id="b76b5-163">V **vyberte bránu místní Data připojit**, vyberte prostředek vaší brány a pak klikněte na tlačítko **připojit vybrané brány**.</span><span class="sxs-lookup"><span data-stu-id="b76b5-163">In **Pick an On-Premises Data Gateway to connect**, select your gateway resource, and then click **Connect selected gateway**.</span></span>

   ![Připojení serveru k prostředku brány](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > <span data-ttu-id="b76b5-165">Pokud vaše brána v seznamu nezobrazí, je váš server pravděpodobně že není ve stejné oblasti jako oblast jste zadali při registraci brány.</span><span class="sxs-lookup"><span data-stu-id="b76b5-165">If your gateway does not appear in the list, your server is likely not in the same region as the region you specified when registering the gateway.</span></span> 

<span data-ttu-id="b76b5-166">A je to.</span><span class="sxs-lookup"><span data-stu-id="b76b5-166">That's it.</span></span> <span data-ttu-id="b76b5-167">Pokud potřebujete otevřít porty nebo provést žádné řešení potíží, nezapomeňte se podívat [místní brána dat](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="b76b5-167">If you need to open ports or do any troubleshooting, be sure to check out [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b76b5-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b76b5-168">Next steps</span></span>
* [<span data-ttu-id="b76b5-169">Správa služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="b76b5-169">Manage Analysis Services</span></span>](analysis-services-manage.md)   
* [<span data-ttu-id="b76b5-170">Získání dat z Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="b76b5-170">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)

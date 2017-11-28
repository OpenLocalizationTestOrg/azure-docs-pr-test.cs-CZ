---
title: "Brána dat místní aaaInstall | Microsoft Docs"
description: "Zjistěte, jak tooinstall a konfigurace brány místní data."
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
ms.openlocfilehash: e2878bf765c82910d452ae2cdd9264a343ec1990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a><span data-ttu-id="24dbf-103">Nainstalujte a nakonfigurujte bránu místní data</span><span class="sxs-lookup"><span data-stu-id="24dbf-103">Install and configure an on-premises data gateway</span></span>
<span data-ttu-id="24dbf-104">Místní brána dat je potřeba při jeden nebo více serverů Azure Analysis Services v hello stejné oblasti připojení zdroje dat tooon místní.</span><span class="sxs-lookup"><span data-stu-id="24dbf-104">An on-premises data gateway is required when one or more Azure Analysis Services servers in hello same region connect tooon-premises data sources.</span></span> <span data-ttu-id="24dbf-105">toolearn Další informace o bráně hello, najdete v části [místní brána dat](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="24dbf-105">toolearn more about hello gateway, see [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24dbf-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="24dbf-106">Prerequisites</span></span>
<span data-ttu-id="24dbf-107">**Minimální požadavky:**</span><span class="sxs-lookup"><span data-stu-id="24dbf-107">**Minimum Requirements:**</span></span>

* <span data-ttu-id="24dbf-108">Rozhraní .NET 4.5 framework</span><span class="sxs-lookup"><span data-stu-id="24dbf-108">.NET 4.5 Framework</span></span>
* <span data-ttu-id="24dbf-109">64bitová verze systému Windows 7 nebo Windows Server 2008 R2 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="24dbf-109">64-bit version of Windows 7 / Windows Server 2008 R2 (or later)</span></span>

<span data-ttu-id="24dbf-110">**Doporučujeme:**</span><span class="sxs-lookup"><span data-stu-id="24dbf-110">**Recommended:**</span></span>

* <span data-ttu-id="24dbf-111">8 jader procesoru</span><span class="sxs-lookup"><span data-stu-id="24dbf-111">8 Core CPU</span></span>
* <span data-ttu-id="24dbf-112">8 GB paměti</span><span class="sxs-lookup"><span data-stu-id="24dbf-112">8 GB Memory</span></span>
* <span data-ttu-id="24dbf-113">64bitová verze systému Windows 2012 R2 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="24dbf-113">64-bit version of Windows 2012 R2 (or later)</span></span>

<span data-ttu-id="24dbf-114">**Důležité informace:**</span><span class="sxs-lookup"><span data-stu-id="24dbf-114">**Important considerations:**</span></span>

* <span data-ttu-id="24dbf-115">Během instalace, při registraci brány na Azure je vybrat hello výchozí oblast pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="24dbf-115">During setup, when registering your gateway with Azure, hello default region for your subscription is selected.</span></span> <span data-ttu-id="24dbf-116">Můžete použít v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="24dbf-116">You can choose a different region.</span></span> <span data-ttu-id="24dbf-117">Pokud máte servery ve více než jedné oblasti, je nutné nainstalovat bránu pro každou oblast.</span><span class="sxs-lookup"><span data-stu-id="24dbf-117">If you have servers in more than one region, you must install a gateway for each region.</span></span> 
* <span data-ttu-id="24dbf-118">Brána Hello nemůže být nainstalována na řadiči domény.</span><span class="sxs-lookup"><span data-stu-id="24dbf-118">hello gateway cannot be installed on a domain controller.</span></span>
* <span data-ttu-id="24dbf-119">V jednom počítači lze nainstalovat pouze jedna brána.</span><span class="sxs-lookup"><span data-stu-id="24dbf-119">Only one gateway can be installed on a single computer.</span></span>
* <span data-ttu-id="24dbf-120">Hello bránu nainstalujte na počítač, který zůstává na a nepřekračuje toosleep.</span><span class="sxs-lookup"><span data-stu-id="24dbf-120">Install hello gateway on a computer that remains on and does not go toosleep.</span></span>
* <span data-ttu-id="24dbf-121">Neinstalujte hello brány v síti bezdrátově připojených tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="24dbf-121">Do not install hello gateway on a computer wirelessly connected tooyour network.</span></span> <span data-ttu-id="24dbf-122">Výkon může být snížena.</span><span class="sxs-lookup"><span data-stu-id="24dbf-122">Performance can be diminished.</span></span>


## <span data-ttu-id="24dbf-123"><a name="download"></a>Stahování</span><span class="sxs-lookup"><span data-stu-id="24dbf-123"><a name="download"></a>Download</span></span>
 [<span data-ttu-id="24dbf-124">Stáhnout hello brány</span><span class="sxs-lookup"><span data-stu-id="24dbf-124">Download hello gateway</span></span>](https://aka.ms/azureasgateway)

## <span data-ttu-id="24dbf-125"><a name="install"></a>Instalace</span><span class="sxs-lookup"><span data-stu-id="24dbf-125"><a name="install"></a>Install</span></span>

1. <span data-ttu-id="24dbf-126">Spusťte instalační program.</span><span class="sxs-lookup"><span data-stu-id="24dbf-126">Run setup.</span></span>

2. <span data-ttu-id="24dbf-127">Vyberte umístění, přijměte podmínky hello a pak klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="24dbf-127">Select a location, accept hello terms, and then click **Install**.</span></span>

   ![Nainstalujte umístění a licenční podmínky](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. <span data-ttu-id="24dbf-129">Vyberte **místní brána dat (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="24dbf-129">Select **On-premises data gateway (recommended)**.</span></span> <span data-ttu-id="24dbf-130">Azure Analysis Services nepodporuje osobní režimu.</span><span class="sxs-lookup"><span data-stu-id="24dbf-130">Azure Analysis Services does not support personal mode.</span></span>

   ![Vyberte typ brány](media/analysis-services-gateway-install/aas-gateway-installer-shared.png)

4. <span data-ttu-id="24dbf-132">Zadejte účtu toosign tooAzure.</span><span class="sxs-lookup"><span data-stu-id="24dbf-132">Enter an account toosign in tooAzure.</span></span> <span data-ttu-id="24dbf-133">Hello účet musí být v vašeho klienta Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="24dbf-133">hello account must be in your tenant's Azure Active Directory.</span></span> <span data-ttu-id="24dbf-134">Tento účet slouží pro hello Správce brány.</span><span class="sxs-lookup"><span data-stu-id="24dbf-134">This account is used for hello gateway administrator.</span></span> 

   ![Zadejte účtu toosign v tooAzure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > <span data-ttu-id="24dbf-136">Pokud se přihlásíte pomocí účtu domény, bude namapované tooyour účet organizace v Azure AD.</span><span class="sxs-lookup"><span data-stu-id="24dbf-136">If you sign in with a domain account, it will be mapped tooyour organizational account in Azure AD.</span></span> <span data-ttu-id="24dbf-137">Váš účet organizace se použije jako správce brány hello hello.</span><span class="sxs-lookup"><span data-stu-id="24dbf-137">Your organizational account will be used as hello hello gateway administrator.</span></span>

## <span data-ttu-id="24dbf-138"><a name="register"></a>Registrace</span><span class="sxs-lookup"><span data-stu-id="24dbf-138"><a name="register"></a>Register</span></span>
<span data-ttu-id="24dbf-139">V pořadí toocreate brány prostředků v Azure je nutné zaregistrovat hello místní instance, které jste nainstalovali s hello cloudové službě brány.</span><span class="sxs-lookup"><span data-stu-id="24dbf-139">In order toocreate a gateway resource in Azure, you must register hello local instance you installed with hello Gateway Cloud Service.</span></span> 

1.  <span data-ttu-id="24dbf-140">Vyberte **registrace nové brány na tomto počítači**.</span><span class="sxs-lookup"><span data-stu-id="24dbf-140">Select **Register a new gateway on this computer**.</span></span>

    ![Registrace](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. <span data-ttu-id="24dbf-142">Zadejte název a obnovení klíče pro bránu.</span><span class="sxs-lookup"><span data-stu-id="24dbf-142">Type a name and recovery key for your gateway.</span></span> <span data-ttu-id="24dbf-143">Ve výchozím nastavení brána hello používá výchozí oblasti vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="24dbf-143">By default, hello gateway uses your subscription's default region.</span></span> <span data-ttu-id="24dbf-144">Pokud potřebujete tooselect v jiné oblasti, vyberte **změnu oblast**.</span><span class="sxs-lookup"><span data-stu-id="24dbf-144">If you need tooselect a different region, select **Change Region**.</span></span>

   ![Registrace](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <span data-ttu-id="24dbf-146"><a name="create-resource"></a>Vytvořte prostředek Azure brány</span><span class="sxs-lookup"><span data-stu-id="24dbf-146"><a name="create-resource"></a>Create an Azure gateway resource</span></span>
<span data-ttu-id="24dbf-147">Po instalaci a zaregistrovat bránu, musíte toocreate brány prostředků ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="24dbf-147">After you've installed and registered your gateway, you need toocreate a gateway resource in your Azure subscription.</span></span> <span data-ttu-id="24dbf-148">Přihlášení tooAzure s hello stejný účet, které jste použili při registraci brány hello.</span><span class="sxs-lookup"><span data-stu-id="24dbf-148">Sign in tooAzure with hello same account you used when registering hello gateway.</span></span>

1. <span data-ttu-id="24dbf-149">Na portálu Azure, klikněte na tlačítko **vytvoření nové služby** > **Enterprise integrace** > **místní brána dat** > **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="24dbf-149">In Azure portal, click **Create a new service** > **Enterprise Integration** > **On-premises data gateway** > **Create**.</span></span>

   ![Vytvořte prostředek brány](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. <span data-ttu-id="24dbf-151">V **vytvořit připojení bránu**, zadejte tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="24dbf-151">In **Create connection gateway**, enter these settings:</span></span>

    * <span data-ttu-id="24dbf-152">**Název**: Zadejte název pro prostředek brány.</span><span class="sxs-lookup"><span data-stu-id="24dbf-152">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="24dbf-153">**Předplatné**: Vyberte hello tooassociate předplatného Azure s vaší brány prostředků.</span><span class="sxs-lookup"><span data-stu-id="24dbf-153">**Subscription**: Select hello Azure subscription tooassociate with your gateway resource.</span></span> 
    <span data-ttu-id="24dbf-154">Tento odběr musí být hello jsou vaše servery v rámci stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="24dbf-154">This subscription should be hello same subscription your servers are in.</span></span>
   
      <span data-ttu-id="24dbf-155">výchozí předplatné Hello je založena na hello účet Azure, který jste použili toosign v.</span><span class="sxs-lookup"><span data-stu-id="24dbf-155">hello default subscription is based on hello Azure account that you used toosign in.</span></span>

    * <span data-ttu-id="24dbf-156">**Skupina prostředků**: Vytvořte skupinu prostředků, nebo vyberte existující.</span><span class="sxs-lookup"><span data-stu-id="24dbf-156">**Resource group**: Create a resource group or select an existing resource group.</span></span>

    * <span data-ttu-id="24dbf-157">**Umístění**: zaregistrovat bránu v oblasti vyberte hello.</span><span class="sxs-lookup"><span data-stu-id="24dbf-157">**Location**: Select hello region you registered your gateway in.</span></span>

    * <span data-ttu-id="24dbf-158">**Název instalace**: Pokud vaše instalace brány již není vybrána, vyberte hello brána registrovaná.</span><span class="sxs-lookup"><span data-stu-id="24dbf-158">**Installation Name**: If your gateway installation isn't already selected, select hello gateway registered.</span></span> 

    <span data-ttu-id="24dbf-159">Když jste hotovi, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="24dbf-159">When you're done, click **Create**.</span></span>

## <span data-ttu-id="24dbf-160"><a name="connect-servers"></a>Připojte prostředek brány toohello servery</span><span class="sxs-lookup"><span data-stu-id="24dbf-160"><a name="connect-servers"></a>Connect servers toohello gateway resource</span></span>

1. <span data-ttu-id="24dbf-161">V přehledu vaší služby Azure Analysis Services serveru, klikněte na tlačítko **místní brána dat**.</span><span class="sxs-lookup"><span data-stu-id="24dbf-161">In your Azure Analysis Services server overview, click **On-Premises Data Gateway**.</span></span>

   ![Připojit server toogateway](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. <span data-ttu-id="24dbf-163">V **vyberte místní brána dat tooconnect**, vyberte prostředek vaší brány a pak klikněte na tlačítko **připojit vybrané brány**.</span><span class="sxs-lookup"><span data-stu-id="24dbf-163">In **Pick an On-Premises Data Gateway tooconnect**, select your gateway resource, and then click **Connect selected gateway**.</span></span>

   ![Připojte server toogateway prostředek](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > <span data-ttu-id="24dbf-165">Pokud vaše brána není uvedené v seznamu hello, váš server není pravděpodobné ve stejné oblasti jako hello oblasti, které jste zadali při registraci brány hello hello.</span><span class="sxs-lookup"><span data-stu-id="24dbf-165">If your gateway does not appear in hello list, your server is likely not in hello same region as hello region you specified when registering hello gateway.</span></span> 

<span data-ttu-id="24dbf-166">A je to.</span><span class="sxs-lookup"><span data-stu-id="24dbf-166">That's it.</span></span> <span data-ttu-id="24dbf-167">Pokud třeba tooopen porty nebo provést žádné řešení potíží s, nebude se toocheck se [místní brána dat](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="24dbf-167">If you need tooopen ports or do any troubleshooting, be sure toocheck out [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="24dbf-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="24dbf-168">Next steps</span></span>
* [<span data-ttu-id="24dbf-169">Správa služby Analysis Services</span><span class="sxs-lookup"><span data-stu-id="24dbf-169">Manage Analysis Services</span></span>](analysis-services-manage.md)   
* [<span data-ttu-id="24dbf-170">Získání dat z Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="24dbf-170">Get data from Azure Analysis Services</span></span>](analysis-services-connect.md)

---
title: "konektory portálu Proxy aplikace Azure AD aaaClassic | Microsoft Docs"
description: "Zahrnuje jak toocreate a spravovat skupiny konektorů v Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 43559b0f4ffc3c7dbbf00901e89ac276d01796e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a><span data-ttu-id="515a7-103">Publikování aplikací na samostatných sítí a umístění pomocí konektoru skupin</span><span class="sxs-lookup"><span data-stu-id="515a7-103">Publish applications on separate networks and locations using connector groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="515a7-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="515a7-104">Azure portal</span></span>](active-directory-application-proxy-connectors-azure-portal.md)
> * [<span data-ttu-id="515a7-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="515a7-105">Azure classic portal</span></span>](active-directory-application-proxy-connectors.md)
>
>

<span data-ttu-id="515a7-106">Konektor skupiny jsou užitečné pro různé scénáře, včetně:</span><span class="sxs-lookup"><span data-stu-id="515a7-106">Connector groups are useful for various scenarios, including:</span></span>

* <span data-ttu-id="515a7-107">Weby s několik datových center vzájemně propojena.</span><span class="sxs-lookup"><span data-stu-id="515a7-107">Sites with multiple interconnected datacenters.</span></span> <span data-ttu-id="515a7-108">V takovém případě chcete tookeep tolik provoz v rámci datového centra hello nejdříve protože datacenter křížové odkazy jsou nákladné a pomalé.</span><span class="sxs-lookup"><span data-stu-id="515a7-108">In this case, you want tookeep as much traffic within hello datacenter as possible because cross-datacenter links are expensive and slow.</span></span> <span data-ttu-id="515a7-109">Můžete nasadit konektorů v každé datacenter tooserve pouze hello aplikace, které se nacházejí v rámci datového centra hello.</span><span class="sxs-lookup"><span data-stu-id="515a7-109">You can deploy connectors in each datacenter tooserve only hello applications that reside within hello datacenter.</span></span> <span data-ttu-id="515a7-110">Tento postup minimalizuje datacenter křížové odkazy a poskytuje zcela transparentní tooyour uživatele.</span><span class="sxs-lookup"><span data-stu-id="515a7-110">This approach minimizes cross-datacenter links and provides an entirely transparent experience tooyour users.</span></span>
* <span data-ttu-id="515a7-111">Správa aplikace nainstalované v izolovaných sítích, které nejsou součástí hello hlavní podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="515a7-111">Managing applications installed on isolated networks that are not part of hello main corporate network.</span></span> <span data-ttu-id="515a7-112">V izolovaných sítích tooalso izolovat aplikace toohello síti můžete použít konektory tooinstall vyhrazené skupiny konektor.</span><span class="sxs-lookup"><span data-stu-id="515a7-112">You can use connector groups tooinstall dedicated connectors on isolated networks tooalso isolate applications toohello network.</span></span>
* <span data-ttu-id="515a7-113">Pro aplikace nainstalované v IaaS pro přístup do cloudu konektor skupiny poskytují běžné služby toosecure hello přístup tooall hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="515a7-113">For applications installed on IaaS for cloud access, connector groups provide a common service toosecure hello access tooall hello apps.</span></span> <span data-ttu-id="515a7-114">Konektor skupiny nemáte vytvoření další závislosti na vaší podnikové síti nebo fragmentu hello aplikační prostředí.</span><span class="sxs-lookup"><span data-stu-id="515a7-114">Connector groups don't create additional dependency on your corporate network, or fragment hello app experience.</span></span> <span data-ttu-id="515a7-115">Konektory lze nainstalovat na každé cloudové datacentrum a sloužit pouze aplikace, které se nacházejí v této síti.</span><span class="sxs-lookup"><span data-stu-id="515a7-115">Connectors can be installed on every cloud datacenter and serve only applications that reside in this network.</span></span> <span data-ttu-id="515a7-116">Můžete nainstalovat několik konektorů tooachieve vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="515a7-116">You can install several connectors tooachieve high availability.</span></span>
* <span data-ttu-id="515a7-117">Podpora pro prostředí s více doménovými strukturami ve kterých můžete nasadit pro každou doménovou strukturu konkrétní konektory a nastavit tooserve konkrétní aplikace.</span><span class="sxs-lookup"><span data-stu-id="515a7-117">Support for multi-forest environments in which specific connectors can be deployed per forest and set tooserve specific applications.</span></span>
* <span data-ttu-id="515a7-118">Konektor skupiny lze použít v lokalitách zotavení po havárii tooeither zjistit převzetí služeb při selhání nebo zálohu pro hello hlavní lokalitu.</span><span class="sxs-lookup"><span data-stu-id="515a7-118">Connector groups can be used in Disaster Recovery sites tooeither detect failover or as backup for hello main site.</span></span>
* <span data-ttu-id="515a7-119">Konektor skupiny můžete také být použité tooserve více společností z jednoho klienta.</span><span class="sxs-lookup"><span data-stu-id="515a7-119">Connector groups can also be used tooserve multiple companies from a single tenant.</span></span>

## <a name="prerequisite-create-your-connectors"></a><span data-ttu-id="515a7-120">Předpoklad: Vytvoření vaší konektory</span><span class="sxs-lookup"><span data-stu-id="515a7-120">Prerequisite: Create your connectors</span></span>
<span data-ttu-id="515a7-121">toogroup konektory, [nainstalovat více konektorů](active-directory-application-proxy-enable.md), potom zadejte název a seskupovat je.</span><span class="sxs-lookup"><span data-stu-id="515a7-121">toogroup your connectors, [install multiple connectors](active-directory-application-proxy-enable.md), then name and group them.</span></span> <span data-ttu-id="515a7-122">Nakonec máte tooassign je toospecific aplikace.</span><span class="sxs-lookup"><span data-stu-id="515a7-122">Finally you have tooassign them toospecific apps.</span></span>

## <a name="step-1-create-connector-groups"></a><span data-ttu-id="515a7-123">Krok 1: Vytvoření skupiny pro konektor</span><span class="sxs-lookup"><span data-stu-id="515a7-123">Step 1: Create connector groups</span></span>
<span data-ttu-id="515a7-124">Můžete vytvořit libovolný počet skupin konektor.</span><span class="sxs-lookup"><span data-stu-id="515a7-124">You can create as many connector groups as you want.</span></span> <span data-ttu-id="515a7-125">Vytvoření konektoru skupiny se provádí v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="515a7-125">Connector group creation is accomplished in hello Azure classic portal.</span></span>

1. <span data-ttu-id="515a7-126">Vyberte adresář a klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="515a7-126">Select your directory and click **Configure**.</span></span>  
    <span data-ttu-id="515a7-127">![Proxy aplikace nakonfigurovat snímek – kliknutím na Spravovat skupiny konektoru](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span><span class="sxs-lookup"><span data-stu-id="515a7-127">![Application proxy, configure screenshot - click manage connector groups](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span></span>
2. <span data-ttu-id="515a7-128">V části Proxy aplikace, klikněte na **spravovat skupiny konektor** a vytvořte skupinu konektor tím, že název skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="515a7-128">Under Application Proxy, click **Manage Connector Groups** and create a connector group by giving hello group a name.</span></span>  
    <span data-ttu-id="515a7-129">![Snímek obrazovky skupin konektoru proxy aplikace – název nové skupiny](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span><span class="sxs-lookup"><span data-stu-id="515a7-129">![Application proxy connector groups screenshot - name new group](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span></span>

## <a name="step-2-assign-connectors-tooyour-groups"></a><span data-ttu-id="515a7-130">Krok 2: Přiřazení konektory tooyour skupiny</span><span class="sxs-lookup"><span data-stu-id="515a7-130">Step 2: Assign connectors tooyour groups</span></span>
<span data-ttu-id="515a7-131">Po vytvoření skupiny pro konektor hello přesunete hello konektory toohello příslušné skupiny.</span><span class="sxs-lookup"><span data-stu-id="515a7-131">Once hello connector groups are created, move hello connectors toohello appropriate group.</span></span>

1. <span data-ttu-id="515a7-132">V části **Proxy aplikace**, klikněte na tlačítko **spravovat konektory**.</span><span class="sxs-lookup"><span data-stu-id="515a7-132">Under **Application Proxy**, click **Manage Connectors**.</span></span>
2. <span data-ttu-id="515a7-133">V části **skupiny**vyberte hello skupiny, které chcete použít pro každý konektor.</span><span class="sxs-lookup"><span data-stu-id="515a7-133">Under **Group**, select hello group you want for each connector.</span></span> <span data-ttu-id="515a7-134">Může trvat hello konektory až too10 minut toobecome active hello nové skupiny.</span><span class="sxs-lookup"><span data-stu-id="515a7-134">It might take hello connectors up too10 minutes toobecome active in hello new group.</span></span>  
    <span data-ttu-id="515a7-135">![Snímek obrazovky konektory proxy aplikace – vyberte skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span><span class="sxs-lookup"><span data-stu-id="515a7-135">![Application proxy connectors screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span></span>

## <a name="step-3-assign-applications-tooyour-connector-groups"></a><span data-ttu-id="515a7-136">Krok 3: Přiřaďte aplikace tooyour konektor skupiny</span><span class="sxs-lookup"><span data-stu-id="515a7-136">Step 3: Assign applications tooyour connector groups</span></span>
<span data-ttu-id="515a7-137">poslední krok Hello je tooset každou skupinu konektor toohello aplikací, který ji obsluhuje.</span><span class="sxs-lookup"><span data-stu-id="515a7-137">hello last step is tooset each application toohello connector group that serves it.</span></span>

1. <span data-ttu-id="515a7-138">V hello portál Azure classic, v adresáři, vyberte hello aplikace tooassign toohello skupiny a klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="515a7-138">In hello Azure classic portal, in your directory, select hello Application you want tooassign toohello group and click **Configure**.</span></span>
2. <span data-ttu-id="515a7-139">V části **konektor skupiny**, vyberte skupinu hello chcete hello toouse aplikace.</span><span class="sxs-lookup"><span data-stu-id="515a7-139">Under **Connector group**, select hello group you want hello application toouse.</span></span> <span data-ttu-id="515a7-140">Tato změna se použije okamžitě.</span><span class="sxs-lookup"><span data-stu-id="515a7-140">This change is immediately applied.</span></span>  
    <span data-ttu-id="515a7-141">![Snímek obrazovky skupiny konektoru proxy aplikace – vyberte skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span><span class="sxs-lookup"><span data-stu-id="515a7-141">![Application proxy connector group screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span></span>

## <a name="see-also"></a><span data-ttu-id="515a7-142">Viz také</span><span class="sxs-lookup"><span data-stu-id="515a7-142">See also</span></span>
* [<span data-ttu-id="515a7-143">Povolení Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="515a7-143">Enable Application Proxy</span></span>](active-directory-application-proxy-enable.md)
* [<span data-ttu-id="515a7-144">Povolení jednoduchého přihlášení</span><span class="sxs-lookup"><span data-stu-id="515a7-144">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="515a7-145">Povolení podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="515a7-145">Enable conditional access</span></span>](active-directory-application-proxy-conditional-access.md)
* [<span data-ttu-id="515a7-146">Řešení problémů, které máte s pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="515a7-146">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)

<span data-ttu-id="515a7-147">Hello nejnovější novinky a aktualizace, najdete na naší hello [blogu Proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)</span><span class="sxs-lookup"><span data-stu-id="515a7-147">For hello latest news and updates, check out hello [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>

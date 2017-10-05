---
title: "Klasického portálu konektory Proxy aplikace Azure AD | Microsoft Docs"
description: "Popisuje postup vytvoření a Správa skupin konektorů v Azure AD Application Proxy."
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
ms.openlocfilehash: fc65c4053c45d9c16c62ee0fe65924133a4bb94a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a><span data-ttu-id="d1c28-103">Publikování aplikací na samostatných sítí a umístění pomocí konektoru skupin</span><span class="sxs-lookup"><span data-stu-id="d1c28-103">Publish applications on separate networks and locations using connector groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d1c28-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d1c28-104">Azure portal</span></span>](active-directory-application-proxy-connectors-azure-portal.md)
> * [<span data-ttu-id="d1c28-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="d1c28-105">Azure classic portal</span></span>](active-directory-application-proxy-connectors.md)
>
>

<span data-ttu-id="d1c28-106">Konektor skupiny jsou užitečné pro různé scénáře, včetně:</span><span class="sxs-lookup"><span data-stu-id="d1c28-106">Connector groups are useful for various scenarios, including:</span></span>

* <span data-ttu-id="d1c28-107">Weby s několik datových center vzájemně propojena.</span><span class="sxs-lookup"><span data-stu-id="d1c28-107">Sites with multiple interconnected datacenters.</span></span> <span data-ttu-id="d1c28-108">V takovém případě chcete zachovat tolik provoz v rámci datového centra nejdříve, protože datacenter křížové odkazy jsou nákladné a pomalé.</span><span class="sxs-lookup"><span data-stu-id="d1c28-108">In this case, you want to keep as much traffic within the datacenter as possible because cross-datacenter links are expensive and slow.</span></span> <span data-ttu-id="d1c28-109">Můžete nasadit konektorů v každé datové centrum, která bude sloužit pouze aplikace, které se nacházejí v datovém centru.</span><span class="sxs-lookup"><span data-stu-id="d1c28-109">You can deploy connectors in each datacenter to serve only the applications that reside within the datacenter.</span></span> <span data-ttu-id="d1c28-110">Tento postup minimalizuje datacenter křížové odkazy a poskytuje zcela transparentní pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="d1c28-110">This approach minimizes cross-datacenter links and provides an entirely transparent experience to your users.</span></span>
* <span data-ttu-id="d1c28-111">Správa aplikace nainstalované v izolovaných sítích, které nejsou součástí hlavní podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="d1c28-111">Managing applications installed on isolated networks that are not part of the main corporate network.</span></span> <span data-ttu-id="d1c28-112">Konektor skupiny můžete použít k instalaci vyhrazené konektory v izolovaných sítích také izolovat aplikace do sítě.</span><span class="sxs-lookup"><span data-stu-id="d1c28-112">You can use connector groups to install dedicated connectors on isolated networks to also isolate applications to the network.</span></span>
* <span data-ttu-id="d1c28-113">Pro aplikace nainstalované v IaaS pro přístup do cloudu konektor skupiny poskytují běžné služby zabezpečený přístup ke všem aplikacím.</span><span class="sxs-lookup"><span data-stu-id="d1c28-113">For applications installed on IaaS for cloud access, connector groups provide a common service to secure the access to all the apps.</span></span> <span data-ttu-id="d1c28-114">Konektor skupiny nemáte vytvoření další závislosti na vaší podnikové síti nebo fragmentu aplikační prostředí.</span><span class="sxs-lookup"><span data-stu-id="d1c28-114">Connector groups don't create additional dependency on your corporate network, or fragment the app experience.</span></span> <span data-ttu-id="d1c28-115">Konektory lze nainstalovat na každé cloudové datacentrum a sloužit pouze aplikace, které se nacházejí v této síti.</span><span class="sxs-lookup"><span data-stu-id="d1c28-115">Connectors can be installed on every cloud datacenter and serve only applications that reside in this network.</span></span> <span data-ttu-id="d1c28-116">Můžete nainstalovat několik konektorů k dosažení vysoké dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="d1c28-116">You can install several connectors to achieve high availability.</span></span>
* <span data-ttu-id="d1c28-117">Podpora pro prostředí s více doménovými strukturami ve kterých konkrétní konektory nasazení pro každou doménovou strukturu a nastavit poskytovat konkrétní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1c28-117">Support for multi-forest environments in which specific connectors can be deployed per forest and set to serve specific applications.</span></span>
* <span data-ttu-id="d1c28-118">Konektor skupiny můžete používat v lokalitách zotavení po havárii a buď zjistit převzetí služeb při selhání nebo zálohu pro hlavní lokalitu.</span><span class="sxs-lookup"><span data-stu-id="d1c28-118">Connector groups can be used in Disaster Recovery sites to either detect failover or as backup for the main site.</span></span>
* <span data-ttu-id="d1c28-119">Konektor skupiny lze také sloužit více společností z jednoho klienta.</span><span class="sxs-lookup"><span data-stu-id="d1c28-119">Connector groups can also be used to serve multiple companies from a single tenant.</span></span>

## <a name="prerequisite-create-your-connectors"></a><span data-ttu-id="d1c28-120">Předpoklad: Vytvoření vaší konektory</span><span class="sxs-lookup"><span data-stu-id="d1c28-120">Prerequisite: Create your connectors</span></span>
<span data-ttu-id="d1c28-121">K seskupení konektory, [nainstalovat více konektorů](active-directory-application-proxy-enable.md), potom zadejte název a seskupovat je.</span><span class="sxs-lookup"><span data-stu-id="d1c28-121">To group your connectors, [install multiple connectors](active-directory-application-proxy-enable.md), then name and group them.</span></span> <span data-ttu-id="d1c28-122">Nakonec budete muset přiřadit konkrétní aplikace.</span><span class="sxs-lookup"><span data-stu-id="d1c28-122">Finally you have to assign them to specific apps.</span></span>

## <a name="step-1-create-connector-groups"></a><span data-ttu-id="d1c28-123">Krok 1: Vytvoření skupiny pro konektor</span><span class="sxs-lookup"><span data-stu-id="d1c28-123">Step 1: Create connector groups</span></span>
<span data-ttu-id="d1c28-124">Můžete vytvořit libovolný počet skupin konektor.</span><span class="sxs-lookup"><span data-stu-id="d1c28-124">You can create as many connector groups as you want.</span></span> <span data-ttu-id="d1c28-125">Vytvoření konektoru skupiny se provádí na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="d1c28-125">Connector group creation is accomplished in the Azure classic portal.</span></span>

1. <span data-ttu-id="d1c28-126">Vyberte adresář a klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="d1c28-126">Select your directory and click **Configure**.</span></span>  
    <span data-ttu-id="d1c28-127">![Proxy aplikace nakonfigurovat snímek – kliknutím na Spravovat skupiny konektoru](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span><span class="sxs-lookup"><span data-stu-id="d1c28-127">![Application proxy, configure screenshot - click manage connector groups](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span></span>
2. <span data-ttu-id="d1c28-128">V části Proxy aplikace, klikněte na **spravovat skupiny konektor** a vytvořte skupinu konektor tím, že název skupiny.</span><span class="sxs-lookup"><span data-stu-id="d1c28-128">Under Application Proxy, click **Manage Connector Groups** and create a connector group by giving the group a name.</span></span>  
    <span data-ttu-id="d1c28-129">![Snímek obrazovky skupin konektoru proxy aplikace – název nové skupiny](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span><span class="sxs-lookup"><span data-stu-id="d1c28-129">![Application proxy connector groups screenshot - name new group](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span></span>

## <a name="step-2-assign-connectors-to-your-groups"></a><span data-ttu-id="d1c28-130">Krok 2: Přiřazení do skupin konektory</span><span class="sxs-lookup"><span data-stu-id="d1c28-130">Step 2: Assign connectors to your groups</span></span>
<span data-ttu-id="d1c28-131">Po vytvoření skupiny konektor přesunete konektory do příslušné skupiny.</span><span class="sxs-lookup"><span data-stu-id="d1c28-131">Once the connector groups are created, move the connectors to the appropriate group.</span></span>

1. <span data-ttu-id="d1c28-132">V části **Proxy aplikace**, klikněte na tlačítko **spravovat konektory**.</span><span class="sxs-lookup"><span data-stu-id="d1c28-132">Under **Application Proxy**, click **Manage Connectors**.</span></span>
2. <span data-ttu-id="d1c28-133">V části **skupiny**, vyberte skupinu, do které chcete použít pro každý konektor.</span><span class="sxs-lookup"><span data-stu-id="d1c28-133">Under **Group**, select the group you want for each connector.</span></span> <span data-ttu-id="d1c28-134">Konektory může trvat až 10 minut, než se stane aktivní do nové skupiny.</span><span class="sxs-lookup"><span data-stu-id="d1c28-134">It might take the connectors up to 10 minutes to become active in the new group.</span></span>  
    <span data-ttu-id="d1c28-135">![Snímek obrazovky konektory proxy aplikace – vyberte skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span><span class="sxs-lookup"><span data-stu-id="d1c28-135">![Application proxy connectors screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span></span>

## <a name="step-3-assign-applications-to-your-connector-groups"></a><span data-ttu-id="d1c28-136">Krok 3: Přiřaďte aplikací do skupin konektoru</span><span class="sxs-lookup"><span data-stu-id="d1c28-136">Step 3: Assign applications to your connector groups</span></span>
<span data-ttu-id="d1c28-137">Posledním krokem je nastavení každou aplikaci, aby skupině konektor, který ji obsluhuje.</span><span class="sxs-lookup"><span data-stu-id="d1c28-137">The last step is to set each application to the connector group that serves it.</span></span>

1. <span data-ttu-id="d1c28-138">V portálu Azure classic, v adresáři, vyberte aplikaci, kterou chcete přiřadit ke skupině a klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="d1c28-138">In the Azure classic portal, in your directory, select the Application you want to assign to the group and click **Configure**.</span></span>
2. <span data-ttu-id="d1c28-139">V části **konektor skupiny**, vyberte skupinu, kterou má aplikace použít.</span><span class="sxs-lookup"><span data-stu-id="d1c28-139">Under **Connector group**, select the group you want the application to use.</span></span> <span data-ttu-id="d1c28-140">Tato změna se použije okamžitě.</span><span class="sxs-lookup"><span data-stu-id="d1c28-140">This change is immediately applied.</span></span>  
    <span data-ttu-id="d1c28-141">![Snímek obrazovky skupiny konektoru proxy aplikace – vyberte skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span><span class="sxs-lookup"><span data-stu-id="d1c28-141">![Application proxy connector group screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span></span>

## <a name="see-also"></a><span data-ttu-id="d1c28-142">Viz také</span><span class="sxs-lookup"><span data-stu-id="d1c28-142">See also</span></span>
* [<span data-ttu-id="d1c28-143">Povolení Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="d1c28-143">Enable Application Proxy</span></span>](active-directory-application-proxy-enable.md)
* [<span data-ttu-id="d1c28-144">Povolení jednoduchého přihlášení</span><span class="sxs-lookup"><span data-stu-id="d1c28-144">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="d1c28-145">Povolení podmíněného přístupu</span><span class="sxs-lookup"><span data-stu-id="d1c28-145">Enable conditional access</span></span>](active-directory-application-proxy-conditional-access.md)
* [<span data-ttu-id="d1c28-146">Řešení problémů, které máte s pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="d1c28-146">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)

<span data-ttu-id="d1c28-147">Nejnovější novinky a aktualizace naleznete na [blogu proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)</span><span class="sxs-lookup"><span data-stu-id="d1c28-147">For the latest news and updates, check out the [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>

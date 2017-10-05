---
title: "Jak otevřít porty brány firewall, které jsou potřebné pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Zjistěte, jaké porty otevřít pro Azure AD Application Proxy fungovala správně"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 8ecd6d7e666d362194126a4abba7a65f2c7b8b6b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a><span data-ttu-id="3cd57-103">Jak otevřít porty brány firewall, které jsou potřebné pro aplikaci Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="3cd57-103">How to open the firewall ports required for an Application Proxy application</span></span>

<span data-ttu-id="3cd57-104">Pokud chcete zobrazit úplný seznam požadované porty a funkci každý port, najdete v části Požadavky [Proxy aplikace dokumentaci](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span><span class="sxs-lookup"><span data-stu-id="3cd57-104">To see a full list of the required ports and the function of each port, see the prerequisites section of the [Application Proxy documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span></span> <span data-ttu-id="3cd57-105">Všimněte si, že Proxy aplikace používá jenom Odchozí porty.</span><span class="sxs-lookup"><span data-stu-id="3cd57-105">note that Application Proxy only uses outbound ports.</span></span>

<span data-ttu-id="3cd57-106">Můžete také zkontrolovat, jestli máte všechny požadované porty otevřete tak, že otevřete [nástroj pro testování porty konektor](https://aadap-portcheck.connectorporttest.msappproxy.net/) z vaší místní sítě.</span><span class="sxs-lookup"><span data-stu-id="3cd57-106">You can also check whether you have all the required ports open by opening the [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) from your on-prem network.</span></span> <span data-ttu-id="3cd57-107">Další zelené značky zaškrtnutí znamená větší odolnost proti chybám.</span><span class="sxs-lookup"><span data-stu-id="3cd57-107">More green checkmarks means greater resiliency.</span></span> 

## <a name="app-proxy-regions"></a><span data-ttu-id="3cd57-108">Oblasti Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="3cd57-108">App Proxy regions</span></span>

<span data-ttu-id="3cd57-109">Pracujeme na způsob, jak umožňují vědět, která z těchto oblastí musí být pro vás.</span><span class="sxs-lookup"><span data-stu-id="3cd57-109">We are working on a way to let you know which of these regions needs to be green for you.</span></span> <span data-ttu-id="3cd57-110">Teď Ujistěte se, že všechny jsou.</span><span class="sxs-lookup"><span data-stu-id="3cd57-110">For now, make sure they all are.</span></span> <span data-ttu-id="3cd57-111">Také je vyžadována bez ohledu na to, které oblasti jsou ve střed USA.</span><span class="sxs-lookup"><span data-stu-id="3cd57-111">Central US is also required regardless of which region you are in.</span></span>

<span data-ttu-id="3cd57-112">Pokud chcete mít jistotu, že tento nástroj vám dává správné výsledky, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="3cd57-112">To make sure the tool gives you the right results, be sure to:</span></span>

-   <span data-ttu-id="3cd57-113">Otevřete nástroj v prohlížeči ze serveru, kam jste nainstalovali konektor.</span><span class="sxs-lookup"><span data-stu-id="3cd57-113">Open the tool on a browser from the server where you have installed the Connector.</span></span>

-   <span data-ttu-id="3cd57-114">Zajistěte, aby všechny proxy nebo brány firewall pro vaše konektor jsou také aplikováno na tuto stránku.</span><span class="sxs-lookup"><span data-stu-id="3cd57-114">Ensure that any proxies or firewalls applicable to your Connector are also applied to this page.</span></span> <span data-ttu-id="3cd57-115">To lze provést v aplikaci Internet Explorer tak, že přejdete do **nastavení**  - &gt; **Možnosti Internetu**  - &gt; **připojení**  - &gt; **nastavení místní sítě**.</span><span class="sxs-lookup"><span data-stu-id="3cd57-115">This can be done in Internet Explorer by going to **Settings** -&gt; **Internet Options** -&gt; **Connections** -&gt; **Lan Settings**.</span></span> <span data-ttu-id="3cd57-116">Na této stránce se zobrazí pole "Použití Proxy serveru pro vaše místní sítě".</span><span class="sxs-lookup"><span data-stu-id="3cd57-116">On this page, you see the field “Use a Proxy Server for your LAN”.</span></span> <span data-ttu-id="3cd57-117">Zaškrtněte toto políčko a uveďte adresu proxy serveru do pole "Adresa".</span><span class="sxs-lookup"><span data-stu-id="3cd57-117">Select this box, and put the proxy address into the “Address” field.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cd57-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3cd57-118">Next steps</span></span>
[<span data-ttu-id="3cd57-119">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3cd57-119">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)

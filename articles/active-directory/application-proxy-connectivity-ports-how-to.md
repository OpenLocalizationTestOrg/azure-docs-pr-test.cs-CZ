---
title: "aaaHow tooopen hello porty brány firewall potřebné pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Zjistěte, jaké porty tooopen pro hello Azure AD Application Proxy toowork správně"
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
ms.openlocfilehash: cdc7badb7c15591689a3bfd6bb26da182b00fb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-hello-firewall-ports-required-for-an-application-proxy-application"></a><span data-ttu-id="0d7c7-103">Jak tooopen hello porty brány firewall, které jsou potřebné pro aplikaci Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="0d7c7-103">How tooopen hello firewall ports required for an Application Proxy application</span></span>

<span data-ttu-id="0d7c7-104">toosee úplný seznam hello požadované porty a hello funkci každý port, najdete v části požadavky hello hello [Proxy aplikace dokumentaci](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span><span class="sxs-lookup"><span data-stu-id="0d7c7-104">toosee a full list of hello required ports and hello function of each port, see hello prerequisites section of hello [Application Proxy documentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable).</span></span> <span data-ttu-id="0d7c7-105">Všimněte si, že Proxy aplikace používá jenom Odchozí porty.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-105">note that Application Proxy only uses outbound ports.</span></span>

<span data-ttu-id="0d7c7-106">Můžete také zkontrolovat, jestli máte všechny otevřené porty hello požadované podle otevírání hello [nástroj pro testování porty konektor](https://aadap-portcheck.connectorporttest.msappproxy.net/) z vaší místní sítě.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-106">You can also check whether you have all hello required ports open by opening hello [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) from your on-prem network.</span></span> <span data-ttu-id="0d7c7-107">Další zelené značky zaškrtnutí znamená větší odolnost proti chybám.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-107">More green checkmarks means greater resiliency.</span></span> 

## <a name="app-proxy-regions"></a><span data-ttu-id="0d7c7-108">Oblasti Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="0d7c7-108">App Proxy regions</span></span>

<span data-ttu-id="0d7c7-109">Pracujeme na způsob, jak toolet víte, které z těchto oblastí musí toobe zelená za vás.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-109">We are working on a way toolet you know which of these regions needs toobe green for you.</span></span> <span data-ttu-id="0d7c7-110">Teď Ujistěte se, že všechny jsou.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-110">For now, make sure they all are.</span></span> <span data-ttu-id="0d7c7-111">Také je vyžadována bez ohledu na to, které oblasti jsou ve střed USA.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-111">Central US is also required regardless of which region you are in.</span></span>

<span data-ttu-id="0d7c7-112">toomake zda hello nástroj poskytuje hello správné výsledky, nezapomeňte:</span><span class="sxs-lookup"><span data-stu-id="0d7c7-112">toomake sure hello tool gives you hello right results, be sure to:</span></span>

-   <span data-ttu-id="0d7c7-113">Nástroj hello v prohlížeči otevřít z hello serveru s nainstalovanou hello konektor.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-113">Open hello tool on a browser from hello server where you have installed hello Connector.</span></span>

-   <span data-ttu-id="0d7c7-114">Zajistěte, aby všechny proxy nebo brány firewall použít tooyour konektor jsou také aplikováno toothis stránky.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-114">Ensure that any proxies or firewalls applicable tooyour Connector are also applied toothis page.</span></span> <span data-ttu-id="0d7c7-115">To lze provést v aplikaci Internet Explorer tak, že přejdete příliš**nastavení**  - &gt; **Možnosti Internetu**  - &gt; **připojení**  - &gt; **Nastavení místní sítě**.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-115">This can be done in Internet Explorer by going too**Settings** -&gt; **Internet Options** -&gt; **Connections** -&gt; **Lan Settings**.</span></span> <span data-ttu-id="0d7c7-116">Na této stránce najdete v části hello pole "Použití Proxy serveru pro vaše místní sítě".</span><span class="sxs-lookup"><span data-stu-id="0d7c7-116">On this page, you see hello field “Use a Proxy Server for your LAN”.</span></span> <span data-ttu-id="0d7c7-117">Zaškrtněte toto políčko a uveďte adresu proxy serveru hello do pole "Address" hello.</span><span class="sxs-lookup"><span data-stu-id="0d7c7-117">Select this box, and put hello proxy address into hello “Address” field.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d7c7-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d7c7-118">Next steps</span></span>
[<span data-ttu-id="0d7c7-119">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0d7c7-119">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)

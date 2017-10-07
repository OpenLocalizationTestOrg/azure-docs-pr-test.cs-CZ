---
title: aaaOverview Azure DNS | Microsoft Docs
description: "Přehled systému DNS, který je hostitelem služby v Microsoft Azure. Hostitel doménu v Microsoft Azure."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: gwallace
ms.openlocfilehash: a10f87c488356469e9c04aabde31129049563891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-overview"></a><span data-ttu-id="1c86d-104">Přehled Azure DNS</span><span class="sxs-lookup"><span data-stu-id="1c86d-104">Azure DNS overview</span></span>

<span data-ttu-id="1c86d-105">Hello systému názvů domény nebo DNS, zodpovídá za překladu (nebo vyřešení) k webu nebo službě název tooits IP adresu.</span><span class="sxs-lookup"><span data-stu-id="1c86d-105">hello Domain Name System, or DNS, is responsible for translating (or resolving) a website or service name tooits IP address.</span></span> <span data-ttu-id="1c86d-106">Azure DNS je hostitelská služba domén DNS poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="1c86d-106">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span> <span data-ttu-id="1c86d-107">Hostování domény do Azure, můžete spravovat DNS záznamů pomocí hello stejné přihlašovací údaje, rozhraní API, nástroje a fakturace jako jinými službami Azure.</span><span class="sxs-lookup"><span data-stu-id="1c86d-107">By hosting your domains in Azure, you can manage your DNS records using hello same credentials, APIs, tools, and billing as your other Azure services.</span></span>

![Přehled systému DNS](./media/dns-overview/scenario.png)

## <a name="features"></a><span data-ttu-id="1c86d-109">Funkce</span><span class="sxs-lookup"><span data-stu-id="1c86d-109">Features</span></span>

* <span data-ttu-id="1c86d-110">**Spolehlivost a výkon** -domén DNS v Azure DNS jsou hostované v Azure globální síti názvových serverů DNS.</span><span class="sxs-lookup"><span data-stu-id="1c86d-110">**Reliability and performance** - DNS domains in Azure DNS are hosted on Azure's global network of DNS name servers.</span></span> <span data-ttu-id="1c86d-111">Používáme všesměrového vysílání sítě tak, aby každý dotaz DNS je zodpovězen pomocí serveru DNS nejbližší dostupné hello.</span><span class="sxs-lookup"><span data-stu-id="1c86d-111">We use Anycast networking so that each DNS query is answered by hello closest available DNS server.</span></span> <span data-ttu-id="1c86d-112">To poskytuje vysoký výkon a vysokou dostupnost vaší domény.</span><span class="sxs-lookup"><span data-stu-id="1c86d-112">This provides both fast performance and high availability for your domain.</span></span>

* <span data-ttu-id="1c86d-113">**Bezproblémová integrace** – služba Azure DNS hello lze použít toomanage záznamy DNS pro služeb Azure a může být použité tooprovide DNS pro vaše externí prostředky.</span><span class="sxs-lookup"><span data-stu-id="1c86d-113">**Seamless integration** - hello Azure DNS service can be used toomanage DNS records for your Azure services and can be used tooprovide DNS for your external resources as well.</span></span> <span data-ttu-id="1c86d-114">Azure DNS je integrován v hello portál Azure a používá hello stejné přihlašovací údaje, fakturaci a podporu kontrakt jako jinými službami Azure.</span><span class="sxs-lookup"><span data-stu-id="1c86d-114">Azure DNS is integrated in hello Azure portal and uses hello same credentials, billing and support contract as your other Azure services.</span></span>

* <span data-ttu-id="1c86d-115">**Zabezpečení** -hello služba Azure DNS je založena na Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1c86d-115">**Security** - hello Azure DNS service is based on Azure Resource Manager.</span></span> <span data-ttu-id="1c86d-116">Jako takový výhody z funkce služby Správce prostředků například řízení přístupu na základě rolí, protokoly auditu a uzamčení prostředků.</span><span class="sxs-lookup"><span data-stu-id="1c86d-116">As such, it benefits from Resource Manager features such as role-based access control, audit logs, and resource locking.</span></span> <span data-ttu-id="1c86d-117">Doménách a záznamech můžete spravovat prostřednictvím hello portál Azure, rutin prostředí Azure PowerShell a hello rozhraní příkazového řádku Azure napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="1c86d-117">Your domains and records can be managed via hello Azure portal, Azure PowerShell cmdlets, and hello cross-platform Azure CLI.</span></span> <span data-ttu-id="1c86d-118">Aplikace, které potřebují Automatická správa DNS může integrovat hello service pomocí hello REST API a sady SDK.</span><span class="sxs-lookup"><span data-stu-id="1c86d-118">Applications requiring automatic DNS management can integrate with hello service via hello REST API and SDKs.</span></span>

<span data-ttu-id="1c86d-119">Azure DNS aktuálně nepodporuje nákup názvů domén.</span><span class="sxs-lookup"><span data-stu-id="1c86d-119">Azure DNS does not currently support purchasing of domain names.</span></span> <span data-ttu-id="1c86d-120">Pokud chcete, aby toopurchase domény, je třeba toouse doménového registrátora názvu domény třetí strany.</span><span class="sxs-lookup"><span data-stu-id="1c86d-120">If you want toopurchase domains, you need toouse a third-party domain name registrar.</span></span> <span data-ttu-id="1c86d-121">Hello Registrátor obvykle účtuje malý roční poplatek.</span><span class="sxs-lookup"><span data-stu-id="1c86d-121">hello registrar typically charges a small annual fee.</span></span> <span data-ttu-id="1c86d-122">pro správu záznamů DNS, může být Hello domény pak hostovaný v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="1c86d-122">hello domains can then be hosted in Azure DNS for management of DNS records.</span></span> <span data-ttu-id="1c86d-123">V tématu [delegovat tooAzure domény DNS](dns-domain-delegation.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1c86d-123">See [Delegate a Domain tooAzure DNS](dns-domain-delegation.md) for details.</span></span>

## <a name="pricing"></a><span data-ttu-id="1c86d-124">Ceny</span><span class="sxs-lookup"><span data-stu-id="1c86d-124">Pricing</span></span>

<span data-ttu-id="1c86d-125">Fakturace DNS je založena na hello počet zóny DNS hostované v Azure a číslem hello dotazů DNS.</span><span class="sxs-lookup"><span data-stu-id="1c86d-125">DNS billing is based on hello number of DNS zones hosted in Azure and by hello number of DNS queries.</span></span> <span data-ttu-id="1c86d-126">Další informace o cenách najdete na toolearn [Azure DNS ceny](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="1c86d-126">toolearn more about pricing visit [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>

## <a name="faq"></a><span data-ttu-id="1c86d-127">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="1c86d-127">FAQ</span></span>

<span data-ttu-id="1c86d-128">Nejčastější dotazy k Azure DNS, najdete v části hello [Azure DNS – nejčastější dotazy](dns-faq.md).</span><span class="sxs-lookup"><span data-stu-id="1c86d-128">For frequently asked questions about Azure DNS, see hello [Azure DNS FAQ](dns-faq.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1c86d-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1c86d-129">Next steps</span></span>

<span data-ttu-id="1c86d-130">Další informace o zóny DNS a záznamy, navštivte stránky: [DNS zóny a zaznamenává přehled](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="1c86d-130">Learn about DNS zones and records by visiting: [DNS zones and records overview](dns-zones-records.md).</span></span>

<span data-ttu-id="1c86d-131">Zjistěte, jak příliš[vytvořit zónu DNS](./dns-getstarted-create-dnszone-portal.md) v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="1c86d-131">Learn how too[create a DNS zone](./dns-getstarted-create-dnszone-portal.md) in Azure DNS.</span></span>

<span data-ttu-id="1c86d-132">Další informace o některých hello Další klíč [sítě možnosti](../networking/networking-overview.md) Azure.</span><span class="sxs-lookup"><span data-stu-id="1c86d-132">Learn about some of hello other key [networking capabilities](../networking/networking-overview.md) of Azure.</span></span>


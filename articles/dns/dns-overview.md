---
title: "Přehled Azure DNS | Microsoft Docs"
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
ms.openlocfilehash: 3705457e4c90f8869496f7f5177531bd128d1057
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-dns-overview"></a><span data-ttu-id="ad30a-104">Přehled Azure DNS</span><span class="sxs-lookup"><span data-stu-id="ad30a-104">Azure DNS overview</span></span>

<span data-ttu-id="ad30a-105">Systému názvů domény nebo DNS, zodpovídá za překladu (nebo vyřešení) název webu nebo služby na jeho IP adresu.</span><span class="sxs-lookup"><span data-stu-id="ad30a-105">The Domain Name System, or DNS, is responsible for translating (or resolving) a website or service name to its IP address.</span></span> <span data-ttu-id="ad30a-106">Azure DNS je hostitelská služba domén DNS poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ad30a-106">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span> <span data-ttu-id="ad30a-107">Pokud svoje domény hostujete v Azure, můžete spravovat svoje DNS záznamy pomocí stejných přihlašovacích údajů, rozhraní API a nástrojů a za stejných fakturačních podmínek jako u ostatních služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="ad30a-107">By hosting your domains in Azure, you can manage your DNS records using the same credentials, APIs, tools, and billing as your other Azure services.</span></span>

![Přehled systému DNS](./media/dns-overview/scenario.png)

## <a name="features"></a><span data-ttu-id="ad30a-109">Funkce</span><span class="sxs-lookup"><span data-stu-id="ad30a-109">Features</span></span>

* <span data-ttu-id="ad30a-110">**Spolehlivost a výkon** -domén DNS v Azure DNS jsou hostované v Azure globální síti názvových serverů DNS.</span><span class="sxs-lookup"><span data-stu-id="ad30a-110">**Reliability and performance** - DNS domains in Azure DNS are hosted on Azure's global network of DNS name servers.</span></span> <span data-ttu-id="ad30a-111">Používáme všesměrového vysílání sítě tak, aby každý dotaz DNS je zodpovězen nejbližší server DNS k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ad30a-111">We use Anycast networking so that each DNS query is answered by the closest available DNS server.</span></span> <span data-ttu-id="ad30a-112">To poskytuje vysoký výkon a vysokou dostupnost vaší domény.</span><span class="sxs-lookup"><span data-stu-id="ad30a-112">This provides both fast performance and high availability for your domain.</span></span>

* <span data-ttu-id="ad30a-113">**Bezproblémová integrace** – služba Azure DNS umožňuje spravovat záznamy DNS pro služeb Azure a slouží k poskytování DNS pro vaše externí prostředky.</span><span class="sxs-lookup"><span data-stu-id="ad30a-113">**Seamless integration** - The Azure DNS service can be used to manage DNS records for your Azure services and can be used to provide DNS for your external resources as well.</span></span> <span data-ttu-id="ad30a-114">Azure DNS je integrovaná v portálu Azure a používá stejné přihlašovací údaje, fakturaci a podporu kontrakt jako jinými službami Azure.</span><span class="sxs-lookup"><span data-stu-id="ad30a-114">Azure DNS is integrated in the Azure portal and uses the same credentials, billing and support contract as your other Azure services.</span></span>

* <span data-ttu-id="ad30a-115">**Zabezpečení** – služba Azure DNS je založena na Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ad30a-115">**Security** - The Azure DNS service is based on Azure Resource Manager.</span></span> <span data-ttu-id="ad30a-116">Jako takový výhody z funkce služby Správce prostředků například řízení přístupu na základě rolí, protokoly auditu a uzamčení prostředků.</span><span class="sxs-lookup"><span data-stu-id="ad30a-116">As such, it benefits from Resource Manager features such as role-based access control, audit logs, and resource locking.</span></span> <span data-ttu-id="ad30a-117">Doménách a záznamech lze spravovat prostřednictvím portálu Azure, rutin prostředí Azure PowerShell a rozhraní příkazového řádku Azure napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="ad30a-117">Your domains and records can be managed via the Azure portal, Azure PowerShell cmdlets, and the cross-platform Azure CLI.</span></span> <span data-ttu-id="ad30a-118">Aplikace, které potřebují Automatická správa DNS může integrovat službu pomocí REST API a sady SDK.</span><span class="sxs-lookup"><span data-stu-id="ad30a-118">Applications requiring automatic DNS management can integrate with the service via the REST API and SDKs.</span></span>

<span data-ttu-id="ad30a-119">Azure DNS aktuálně nepodporuje nákup názvů domén.</span><span class="sxs-lookup"><span data-stu-id="ad30a-119">Azure DNS does not currently support purchasing of domain names.</span></span> <span data-ttu-id="ad30a-120">Pokud chcete zakoupit domén, budete muset použít doménového registrátora názvu domény třetí strany.</span><span class="sxs-lookup"><span data-stu-id="ad30a-120">If you want to purchase domains, you need to use a third-party domain name registrar.</span></span> <span data-ttu-id="ad30a-121">Registrátor obvykle účtuje malý roční poplatek.</span><span class="sxs-lookup"><span data-stu-id="ad30a-121">The registrar typically charges a small annual fee.</span></span> <span data-ttu-id="ad30a-122">Pro správu záznamů DNS, může být domény pak hostovaný v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="ad30a-122">The domains can then be hosted in Azure DNS for management of DNS records.</span></span> <span data-ttu-id="ad30a-123">V tématu [delegování domény do Azure DNS](dns-domain-delegation.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ad30a-123">See [Delegate a Domain to Azure DNS](dns-domain-delegation.md) for details.</span></span>

## <a name="pricing"></a><span data-ttu-id="ad30a-124">Ceny</span><span class="sxs-lookup"><span data-stu-id="ad30a-124">Pricing</span></span>

<span data-ttu-id="ad30a-125">Fakturace DNS je založena na počtu zónách DNS hostovaných v Azure a počet dotazů DNS.</span><span class="sxs-lookup"><span data-stu-id="ad30a-125">DNS billing is based on the number of DNS zones hosted in Azure and by the number of DNS queries.</span></span> <span data-ttu-id="ad30a-126">Další informace o cenách najdete na [Azure DNS ceny](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="ad30a-126">To learn more about pricing visit [Azure DNS Pricing](https://azure.microsoft.com/pricing/details/dns/).</span></span>

## <a name="faq"></a><span data-ttu-id="ad30a-127">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="ad30a-127">FAQ</span></span>

<span data-ttu-id="ad30a-128">Nejčastější dotazy k Azure DNS, najdete v článku [Azure DNS – nejčastější dotazy](dns-faq.md).</span><span class="sxs-lookup"><span data-stu-id="ad30a-128">For frequently asked questions about Azure DNS, see the [Azure DNS FAQ](dns-faq.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad30a-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad30a-129">Next steps</span></span>

<span data-ttu-id="ad30a-130">Další informace o zóny DNS a záznamy, navštivte stránky: [DNS zóny a zaznamenává přehled](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="ad30a-130">Learn about DNS zones and records by visiting: [DNS zones and records overview](dns-zones-records.md).</span></span>

<span data-ttu-id="ad30a-131">Zjistěte, jak [vytvořit zónu DNS](./dns-getstarted-create-dnszone-portal.md) v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="ad30a-131">Learn how to [create a DNS zone](./dns-getstarted-create-dnszone-portal.md) in Azure DNS.</span></span>

<span data-ttu-id="ad30a-132">Informace o některých dalších klíčových [možnostech sítě](../networking/networking-overview.md) v Azure.</span><span class="sxs-lookup"><span data-stu-id="ad30a-132">Learn about some of the other key [networking capabilities](../networking/networking-overview.md) of Azure.</span></span>


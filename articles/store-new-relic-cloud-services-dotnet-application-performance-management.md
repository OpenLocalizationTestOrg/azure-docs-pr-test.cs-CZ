---
title: aaaUsing New Relic s Azure | Microsoft Docs
description: "Zjistěte, jak toouse hello toomanage služby New Relic a sledovat vaše aplikace Azure."
services: 
documentationcenter: .net
author: nickfloyd
manager: timlt
editor: 
ms.assetid: b01011be-c344-4e33-987d-c93dac1971fb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2016
ms.author: nickfloyd@newrelic.com
ms.openlocfilehash: 81e5f1b694264e3586ac75d40edd8afe185493d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="new-relic-application-performance-management-on-azure"></a><span data-ttu-id="dcbd3-103">Nové Správa výkonu aplikací New Relic v Azure</span><span class="sxs-lookup"><span data-stu-id="dcbd3-103">New Relic Application Performance Management on Azure</span></span>
<span data-ttu-id="dcbd3-104">Můžete přidat New Relic špičkových výkonu monitorování tooyour Azure hostované aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-104">You can add New Relic's world-class performance monitoring tooyour Azure hosted applications.</span></span> <span data-ttu-id="dcbd3-105">Spolu s komplexní funkcemi pro monitorování, řešení potíží a ladění aplikace Azure máte také nárok na sníženou cenu produktů New Relic pomocí služby Azure.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-105">Along with comprehensive capabilities for monitoring, troubleshooting, and tuning your Azure apps, you are also eligible for a discounted price for New Relic products by using Azure.</span></span>

## <a name="what-is-new-relic"></a><span data-ttu-id="dcbd3-106">Co je to New Relic?</span><span class="sxs-lookup"><span data-stu-id="dcbd3-106">What is New Relic?</span></span>
<span data-ttu-id="dcbd3-107">S [New Relic produkty](https://newrelic.com/products), můžete vyřešit chyby aplikace, předcházení potenciálních problémů a pochopit výkon hello celé prostředí.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-107">With [New Relic products](https://newrelic.com/products), you can solve app errors, stay ahead of potential issues, and understand hello performance of your entire environment.</span></span> <span data-ttu-id="dcbd3-108">Je navrženou toosave, které jste čas při identifikaci a diagnostice problémů s výkonem a uloží je hello informace, které toosolve potřebujete tyto problémy na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-108">It is designed toosave you time when identifying and diagnosing performance issues, and it puts hello information you need toosolve these issues at your fingertips.</span></span>

<span data-ttu-id="dcbd3-109">Nové hello sleduje Relic načíst čas a propustnost pro webové transakce, jak z hello serveru a do prohlížečů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-109">New Relic tracks hello load time and throughput for your web transactions, both from hello server and your users' browsers.</span></span> <span data-ttu-id="dcbd3-110">Zobrazuje, kolik času stráveného v databázi hello analyzuje pomalé dotazy a webových požadavků, poskytuje provozu monitorování a výstrahy, sleduje výjimky aplikací a mnoho další.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-110">It shows how much time you spend in hello database, analyzes slow queries and web requests, provides uptime monitoring and alerting, tracks application exceptions, and a whole lot more.</span></span> 

## <a name="special-pricing"></a><span data-ttu-id="dcbd3-111">Speciální ceny</span><span class="sxs-lookup"><span data-stu-id="dcbd3-111">Special pricing</span></span>
<span data-ttu-id="dcbd3-112">Nové standardní Relic je volné tooAzure uživatele.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-112">New Relic Standard is free tooAzure users.</span></span> <span data-ttu-id="dcbd3-113">Pro nové New Relic se nabízí podle velikost instance pro cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-113">New Relic Pro is offered based on instance size for Azure Cloud Services.</span></span> <span data-ttu-id="dcbd3-114">Informace o cenách naleznete v tématu hello [New Relic stránky](https://azure.microsoft.com/marketplace/partners/newrelic/newrelic/) v hello Azure Marketplace nebo se obraťte na New Relic (sales@newrelic.com) pro cenové úrovni celého podniku.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-114">For pricing information, see hello [New Relic page](https://azure.microsoft.com/marketplace/partners/newrelic/newrelic/) in hello Azure Marketplace or contact New Relic (sales@newrelic.com) for enterprise-level pricing.</span></span>

<span data-ttu-id="dcbd3-115">Azure zákazníci obdrží dvě týden zkušební předplatné Pro nový New Relic, když nasadí agent New Relic hello.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-115">Azure customers receive a two week trial subscription of New Relic Pro when they deploy hello New Relic agent.</span></span>

## <a name="sign-up-for-new-relic-using-hello-azure-store"></a><span data-ttu-id="dcbd3-116">Zaregistrujte si používání úložiště Azure hello New Relic.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-116">Sign up for New Relic using hello Azure Store</span></span>
<span data-ttu-id="dcbd3-117">New Relic se bezproblémově integruje se webové role Azure a pracovní role.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-117">New Relic integrates seamlessly with Azure Web Roles and Worker roles.</span></span> <span data-ttu-id="dcbd3-118">Můžete rychle a snadno zaregistrovat New Relic přímo z hello úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-118">You can quickly and easily sign up for New Relic directly from hello Azure Store.</span></span> <span data-ttu-id="dcbd3-119">Pokyny najdete v tématu hello [Azure ukládání pokyny pro přihlášení](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services#signup) z New Relic.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-119">For instructions, see hello [Azure store sign-up instructions](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services#signup) from New Relic.</span></span>

## <a name="view-your-data"></a><span data-ttu-id="dcbd3-120">Zobrazení dat</span><span class="sxs-lookup"><span data-stu-id="dcbd3-120">View your data</span></span>
<span data-ttu-id="dcbd3-121">Jakmile jste zaregistrovali, můžete využít výhod úžasné monitorování aplikací a datové analýzy New Relic.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-121">Once you've signed up, you can take advantage of New Relic's amazing application monitoring and data-driven analysis.</span></span> <span data-ttu-id="dcbd3-122">toocheck výkon vaší aplikace v New Relic:</span><span class="sxs-lookup"><span data-stu-id="dcbd3-122">toocheck your app's performance in New Relic:</span></span>

1. <span data-ttu-id="dcbd3-123">Hello portálu Azure vyberte spravovat.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-123">From hello Azure Portal, select Manage.</span></span>
2. <span data-ttu-id="dcbd3-124">Přihlaste se pomocí vaší e-mailu účet New Relic a heslo.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-124">Sign in with your New Relic account email and password.</span></span>
3. <span data-ttu-id="dcbd3-125">Vyberte svou aplikaci z tooview index aplikace hello data všechny aplikace v hello [stránku přehled APM](https://docs.newrelic.com/docs/apm/applications-menu/monitoring/apm-overview-page).</span><span class="sxs-lookup"><span data-stu-id="dcbd3-125">Select your app from hello Application index tooview all your app's data in hello [APM overview page](https://docs.newrelic.com/docs/apm/applications-menu/monitoring/apm-overview-page).</span></span>

## <a name="using-new-relic-with-azure"></a><span data-ttu-id="dcbd3-126">Pomocí Azure New Relic.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-126">Using New Relic with Azure</span></span>
<span data-ttu-id="dcbd3-127">Další informace o používání New Relic a Azure najdete v tématu [web dokumentace New Relic](https://docs.newrelic.com/docs/agents/net-agent/azure-installation), včetně:</span><span class="sxs-lookup"><span data-stu-id="dcbd3-127">For more information about using New Relic and Azure, see [New Relic's Documentation site](https://docs.newrelic.com/docs/agents/net-agent/azure-installation), including:</span></span> 

* [<span data-ttu-id="dcbd3-128">New Relic, pro .NET</span><span class="sxs-lookup"><span data-stu-id="dcbd3-128">New Relic for .NET</span></span>](https://docs.newrelic.com/docs/agents/net-agent/getting-started/new-relic-net)
* [<span data-ttu-id="dcbd3-129">Instrumentace pro roli rozhraní .NET pracovního procesu Azure Cloud service</span><span class="sxs-lookup"><span data-stu-id="dcbd3-129">Instrument a .NET Worker Role on Azure Cloud service</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/instrument-net-worker-role-azure-cloud-service)
* [<span data-ttu-id="dcbd3-130">Pro platformu Microsoft Azure Cloud hello New Relic.</span><span class="sxs-lookup"><span data-stu-id="dcbd3-130">New Relic for hello Microsoft Azure Cloud platform</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services)
* [<span data-ttu-id="dcbd3-131">New Relic pro hello Microsoft Azure App Services</span><span class="sxs-lookup"><span data-stu-id="dcbd3-131">New Relic for hello Microsoft Azure App Services</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-portal)
* [<span data-ttu-id="dcbd3-132">Nové řešení potíží s Relic nebo Azure</span><span class="sxs-lookup"><span data-stu-id="dcbd3-132">New Relic/Azure troubleshooting</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-troubleshooting)


---
title: "Pomocí Azure New Relic. | Microsoft Docs"
description: "Další informace o použití služby New Relic ke správě a sledování aplikace Azure."
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
ms.openlocfilehash: f4f13c909a6ff640d403f5264004176c087925dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="new-relic-application-performance-management-on-azure"></a><span data-ttu-id="a3d84-103">Nové Správa výkonu aplikací New Relic v Azure</span><span class="sxs-lookup"><span data-stu-id="a3d84-103">New Relic Application Performance Management on Azure</span></span>
<span data-ttu-id="a3d84-104">New Relic špičkových výkonu můžete přidat monitorování do vaší Azure hostované aplikace.</span><span class="sxs-lookup"><span data-stu-id="a3d84-104">You can add New Relic's world-class performance monitoring to your Azure hosted applications.</span></span> <span data-ttu-id="a3d84-105">Spolu s komplexní funkcemi pro monitorování, řešení potíží a ladění aplikace Azure máte také nárok na sníženou cenu produktů New Relic pomocí služby Azure.</span><span class="sxs-lookup"><span data-stu-id="a3d84-105">Along with comprehensive capabilities for monitoring, troubleshooting, and tuning your Azure apps, you are also eligible for a discounted price for New Relic products by using Azure.</span></span>

## <a name="what-is-new-relic"></a><span data-ttu-id="a3d84-106">Co je to New Relic?</span><span class="sxs-lookup"><span data-stu-id="a3d84-106">What is New Relic?</span></span>
<span data-ttu-id="a3d84-107">S [New Relic produkty](https://newrelic.com/products), můžete vyřešit chyby aplikace, předcházení potenciální problémy a pochopit výkon celé prostředí.</span><span class="sxs-lookup"><span data-stu-id="a3d84-107">With [New Relic products](https://newrelic.com/products), you can solve app errors, stay ahead of potential issues, and understand the performance of your entire environment.</span></span> <span data-ttu-id="a3d84-108">Je navržený tak, aby ušetřil čas při identifikaci a diagnostikování potíží s výkonem a abyste měli informace, které k jejich řešení potřebujete, jednoduše na dosah ruky.</span><span class="sxs-lookup"><span data-stu-id="a3d84-108">It is designed to save you time when identifying and diagnosing performance issues, and it puts the information you need to solve these issues at your fingertips.</span></span>

<span data-ttu-id="a3d84-109">New Relic sleduje doba načítání a propustnost pro webové transakce, jak ze serveru a do prohlížečů uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a3d84-109">New Relic tracks the load time and throughput for your web transactions, both from the server and your users' browsers.</span></span> <span data-ttu-id="a3d84-110">Zobrazuje, kolik času stráveného v databázi, analyzuje pomalé dotazy a webových požadavků, poskytuje provozu monitorování a výstrahy, sleduje výjimky aplikací a mnoho další.</span><span class="sxs-lookup"><span data-stu-id="a3d84-110">It shows how much time you spend in the database, analyzes slow queries and web requests, provides uptime monitoring and alerting, tracks application exceptions, and a whole lot more.</span></span> 

## <a name="special-pricing"></a><span data-ttu-id="a3d84-111">Speciální ceny</span><span class="sxs-lookup"><span data-stu-id="a3d84-111">Special pricing</span></span>
<span data-ttu-id="a3d84-112">Nové standardní Relic je zdarma uživatelům Azure.</span><span class="sxs-lookup"><span data-stu-id="a3d84-112">New Relic Standard is free to Azure users.</span></span> <span data-ttu-id="a3d84-113">Pro nové New Relic se nabízí podle velikost instance pro cloudové služby Azure.</span><span class="sxs-lookup"><span data-stu-id="a3d84-113">New Relic Pro is offered based on instance size for Azure Cloud Services.</span></span> <span data-ttu-id="a3d84-114">Informace o cenách najdete v článku [New Relic stránky](https://azure.microsoft.com/marketplace/partners/newrelic/newrelic/) v Azure Marketplace nebo kontaktujte New Relic (sales@newrelic.com) pro cenové úrovni celého podniku.</span><span class="sxs-lookup"><span data-stu-id="a3d84-114">For pricing information, see the [New Relic page](https://azure.microsoft.com/marketplace/partners/newrelic/newrelic/) in the Azure Marketplace or contact New Relic (sales@newrelic.com) for enterprise-level pricing.</span></span>

<span data-ttu-id="a3d84-115">Azure zákazníci obdrží dvě týden zkušební předplatné Pro nový Relic, když nasadí agent New Relic.</span><span class="sxs-lookup"><span data-stu-id="a3d84-115">Azure customers receive a two week trial subscription of New Relic Pro when they deploy the New Relic agent.</span></span>

## <a name="sign-up-for-new-relic-using-the-azure-store"></a><span data-ttu-id="a3d84-116">Zaregistrujte si New Relic pomocí úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="a3d84-116">Sign up for New Relic using the Azure Store</span></span>
<span data-ttu-id="a3d84-117">New Relic se bezproblémově integruje se webové role Azure a pracovní role.</span><span class="sxs-lookup"><span data-stu-id="a3d84-117">New Relic integrates seamlessly with Azure Web Roles and Worker roles.</span></span> <span data-ttu-id="a3d84-118">Můžete rychle a snadno zaregistrovat New Relic přímo z úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a3d84-118">You can quickly and easily sign up for New Relic directly from the Azure Store.</span></span> <span data-ttu-id="a3d84-119">Pokyny najdete v tématu [Azure ukládání pokyny pro přihlášení](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services#signup) z New Relic.</span><span class="sxs-lookup"><span data-stu-id="a3d84-119">For instructions, see the [Azure store sign-up instructions](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services#signup) from New Relic.</span></span>

## <a name="view-your-data"></a><span data-ttu-id="a3d84-120">Zobrazení dat</span><span class="sxs-lookup"><span data-stu-id="a3d84-120">View your data</span></span>
<span data-ttu-id="a3d84-121">Jakmile jste zaregistrovali, můžete využít výhod úžasné monitorování aplikací a datové analýzy New Relic.</span><span class="sxs-lookup"><span data-stu-id="a3d84-121">Once you've signed up, you can take advantage of New Relic's amazing application monitoring and data-driven analysis.</span></span> <span data-ttu-id="a3d84-122">Chcete-li zkontrolovat výkon vaší aplikace v New Relic:</span><span class="sxs-lookup"><span data-stu-id="a3d84-122">To check your app's performance in New Relic:</span></span>

1. <span data-ttu-id="a3d84-123">Z portálu Azure vyberte spravovat.</span><span class="sxs-lookup"><span data-stu-id="a3d84-123">From the Azure Portal, select Manage.</span></span>
2. <span data-ttu-id="a3d84-124">Přihlaste se pomocí vaší e-mailu účet New Relic a heslo.</span><span class="sxs-lookup"><span data-stu-id="a3d84-124">Sign in with your New Relic account email and password.</span></span>
3. <span data-ttu-id="a3d84-125">Vyberte svou aplikaci z aplikace index pro zobrazení dat všechny aplikace v [stránku přehled APM](https://docs.newrelic.com/docs/apm/applications-menu/monitoring/apm-overview-page).</span><span class="sxs-lookup"><span data-stu-id="a3d84-125">Select your app from the Application index to view all your app's data in the [APM overview page](https://docs.newrelic.com/docs/apm/applications-menu/monitoring/apm-overview-page).</span></span>

## <a name="using-new-relic-with-azure"></a><span data-ttu-id="a3d84-126">Pomocí Azure New Relic.</span><span class="sxs-lookup"><span data-stu-id="a3d84-126">Using New Relic with Azure</span></span>
<span data-ttu-id="a3d84-127">Další informace o používání New Relic a Azure najdete v tématu [web dokumentace New Relic](https://docs.newrelic.com/docs/agents/net-agent/azure-installation), včetně:</span><span class="sxs-lookup"><span data-stu-id="a3d84-127">For more information about using New Relic and Azure, see [New Relic's Documentation site](https://docs.newrelic.com/docs/agents/net-agent/azure-installation), including:</span></span> 

* [<span data-ttu-id="a3d84-128">New Relic, pro .NET</span><span class="sxs-lookup"><span data-stu-id="a3d84-128">New Relic for .NET</span></span>](https://docs.newrelic.com/docs/agents/net-agent/getting-started/new-relic-net)
* [<span data-ttu-id="a3d84-129">Instrumentace pro roli rozhraní .NET pracovního procesu Azure Cloud service</span><span class="sxs-lookup"><span data-stu-id="a3d84-129">Instrument a .NET Worker Role on Azure Cloud service</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/instrument-net-worker-role-azure-cloud-service)
* [<span data-ttu-id="a3d84-130">New Relic pro platformu Microsoft Azure Cloud</span><span class="sxs-lookup"><span data-stu-id="a3d84-130">New Relic for the Microsoft Azure Cloud platform</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services)
* [<span data-ttu-id="a3d84-131">Služby New Relic, pro aplikaci Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="a3d84-131">New Relic for the Microsoft Azure App Services</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-portal)
* [<span data-ttu-id="a3d84-132">Nové řešení potíží s Relic nebo Azure</span><span class="sxs-lookup"><span data-stu-id="a3d84-132">New Relic/Azure troubleshooting</span></span>](https://docs.newrelic.com/docs/agents/net-agent/azure-troubleshooting)


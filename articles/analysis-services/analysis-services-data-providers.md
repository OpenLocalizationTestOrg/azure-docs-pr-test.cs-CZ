---
title: "Klient knihovny potřebné pro připojení k Azure Analysis Services | Microsoft Docs"
description: "Popisuje klientské knihovny potřebné pro klientské aplikace a nástroje pro připojení služby Azure Analysis Services"
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
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: a96e7fe671dc051da35168fa7b49ecba53b4c4a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a><span data-ttu-id="754e9-103">Klientské knihovny pro připojení k Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="754e9-103">Client libraries for connecting to Azure Analysis Services</span></span>

<span data-ttu-id="754e9-104">Knihovny klienta jsou nezbytné pro klientské aplikace a nástroje pro připojení k serverům služby Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="754e9-104">Client libraries are necessary for client applications and tools to connect to Analysis Services servers.</span></span> 

<span data-ttu-id="754e9-105">Služba Analysis Services, používají tři knihovny klienta.</span><span class="sxs-lookup"><span data-stu-id="754e9-105">Analysis Services utilize three client libraries.</span></span> <span data-ttu-id="754e9-106">ADOMD.NET a Analysis Services Management Objects (AMO), jsou spravované klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="754e9-106">ADOMD.NET and Analysis Services Management Objects (AMO), are managed client libraries.</span></span> <span data-ttu-id="754e9-107">Zprostředkovatel OLE DB služby Analysis Services (MSOLAP DLL) je nativní Klientská knihovna.</span><span class="sxs-lookup"><span data-stu-id="754e9-107">The Analysis Services OLE DB provider (MSOLAP DLL) is a native client library.</span></span> <span data-ttu-id="754e9-108">Obvykle jsou všechny tři nainstalovány ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="754e9-108">Typically, all three are installed at the same time.</span></span> <span data-ttu-id="754e9-109">Azure Analysis Services vyžaduje nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="754e9-109">Azure Analysis Services requires the latest versions.</span></span> 

<span data-ttu-id="754e9-110">Microsoft klientské aplikace, třeba Power BI Desktop a Excel nainstalovat všechny tři klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="754e9-110">Microsoft client applications such as Power BI Desktop and Excel install all three client libraries.</span></span> <span data-ttu-id="754e9-111">V závislosti na verzi nebo četnost aktualizací, ale klientské knihovny nemusí být nejnovější verze vyžaduje Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="754e9-111">However, depending on the version or frequency of updates, client libraries may not be the latest versions required by Azure Analysis Services.</span></span> <span data-ttu-id="754e9-112">To samé platí pro vlastní aplikace a další rozhraní, jako jsou AsCmd, Tom nebo ADOMD.NET.</span><span class="sxs-lookup"><span data-stu-id="754e9-112">The same applies to custom applications or other interfaces such as AsCmd, TOM, ADOMD.NET.</span></span> <span data-ttu-id="754e9-113">Tyto aplikace vyžadují ruční instalaci knihovny.</span><span class="sxs-lookup"><span data-stu-id="754e9-113">These applications require manually installing the libraries.</span></span> <span data-ttu-id="754e9-114">Klientské knihovny pro ruční instalaci jsou zahrnuty v balíčcích funkcí systému SQL Server jako distribuovatelného balíčky.</span><span class="sxs-lookup"><span data-stu-id="754e9-114">The client libraries for manual installation are included in SQL Server feature packs as distributable packages.</span></span> <span data-ttu-id="754e9-115">Ale tyto knihovny klienta, jsou svázané s verze systému SQL Server a nemusí být nejnovější.</span><span class="sxs-lookup"><span data-stu-id="754e9-115">However, these client libraries are tied to the SQL Server version and may not be the latest.</span></span>  

<span data-ttu-id="754e9-116">Klientské knihovny pro připojení klientů se liší od zprostředkovatelů dat. požadované pro připojení ke zdroji dat ze serveru Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="754e9-116">Client libraries for client connections are different from data providers required to connect from an Azure Analysis Services server to a data source.</span></span> <span data-ttu-id="754e9-117">Další informace o připojení zdroje dat naleznete v tématu [připojení zdroje dat](analysis-services-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="754e9-117">To learn more about datasource connections, see [Datasource connections](analysis-services-datasource.md).</span></span>

## <a name="download-the-latest-client-libraries"></a><span data-ttu-id="754e9-118">Stáhněte si nejnovější knihovny klienta</span><span class="sxs-lookup"><span data-stu-id="754e9-118">Download the latest client libraries</span></span>  
<span data-ttu-id="754e9-119">Pomocí následujících klientských knihoven, pokud jsou v produkčním prostředí a vyžadovat plně vydaná a podporované verze.</span><span class="sxs-lookup"><span data-stu-id="754e9-119">Use the following client libraries if you are in a production environment and require fully released and supported versions.</span></span>

[<span data-ttu-id="754e9-120">MSOLAP (amd64)</span><span class="sxs-lookup"><span data-stu-id="754e9-120">MSOLAP (amd64)</span></span>](https://go.microsoft.com/fwlink/?linkid=829576)</br><span data-ttu-id="754e9-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span><span class="sxs-lookup"><span data-stu-id="754e9-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span></span></br><span data-ttu-id="754e9-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span><span class="sxs-lookup"><span data-stu-id="754e9-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span></span></br><span data-ttu-id="754e9-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span><span class="sxs-lookup"><span data-stu-id="754e9-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="754e9-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="754e9-124">Next steps</span></span>
<span data-ttu-id="754e9-125">[Připojit pomocí aplikace Excel](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="754e9-125">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
[<span data-ttu-id="754e9-126">Propojení s Power BI</span><span class="sxs-lookup"><span data-stu-id="754e9-126">Connect with Power BI</span></span>](analysis-services-connect-pbi.md)

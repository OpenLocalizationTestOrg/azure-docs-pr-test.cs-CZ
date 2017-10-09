---
title: "aaaClient knihovny potřebné pro připojení služby Analysis Services tooAzure | Microsoft Docs"
description: "Popisuje klienta knihovny potřebné pro klientské aplikace a nástroje tooconnect Azure Analysis Services"
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
ms.openlocfilehash: 74ba5c05ba76c6587c5aed38f200a1ba469aa4f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="client-libraries-for-connecting-tooazure-analysis-services"></a><span data-ttu-id="8762a-103">Klientské knihovny pro připojení tooAzure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="8762a-103">Client libraries for connecting tooAzure Analysis Services</span></span>

<span data-ttu-id="8762a-104">Knihovny klienta jsou nezbytné pro nástroje tooconnect tooAnalysis služby servery a klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="8762a-104">Client libraries are necessary for client applications and tools tooconnect tooAnalysis Services servers.</span></span> 

<span data-ttu-id="8762a-105">Služba Analysis Services, používají tři knihovny klienta.</span><span class="sxs-lookup"><span data-stu-id="8762a-105">Analysis Services utilize three client libraries.</span></span> <span data-ttu-id="8762a-106">ADOMD.NET a Analysis Services Management Objects (AMO), jsou spravované klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="8762a-106">ADOMD.NET and Analysis Services Management Objects (AMO), are managed client libraries.</span></span> <span data-ttu-id="8762a-107">Zprostředkovatel OLE DB služby Analysis Services Hello (MSOLAP DLL) je nativní klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="8762a-107">hello Analysis Services OLE DB provider (MSOLAP DLL) is a native client library.</span></span> <span data-ttu-id="8762a-108">Obvykle jsou všechny tři nainstalovány na hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="8762a-108">Typically, all three are installed at hello same time.</span></span> <span data-ttu-id="8762a-109">Azure Analysis Services vyžaduje hello nejnovější verze.</span><span class="sxs-lookup"><span data-stu-id="8762a-109">Azure Analysis Services requires hello latest versions.</span></span> 

<span data-ttu-id="8762a-110">Microsoft klientské aplikace, třeba Power BI Desktop a Excel nainstalovat všechny tři klientské knihovny.</span><span class="sxs-lookup"><span data-stu-id="8762a-110">Microsoft client applications such as Power BI Desktop and Excel install all three client libraries.</span></span> <span data-ttu-id="8762a-111">V závislosti na verzi hello nebo četnost aktualizací, ale klientské knihovny nemusí být hello nejnovější verze vyžadované službou Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="8762a-111">However, depending on hello version or frequency of updates, client libraries may not be hello latest versions required by Azure Analysis Services.</span></span> <span data-ttu-id="8762a-112">Hello totéž platí i toocustom aplikace nebo dalších rozhraní, jako je například AsCmd, TNÍ, ADOMD.NET.</span><span class="sxs-lookup"><span data-stu-id="8762a-112">hello same applies toocustom applications or other interfaces such as AsCmd, TOM, ADOMD.NET.</span></span> <span data-ttu-id="8762a-113">Tyto aplikace vyžadují ruční instalaci hello knihovny.</span><span class="sxs-lookup"><span data-stu-id="8762a-113">These applications require manually installing hello libraries.</span></span> <span data-ttu-id="8762a-114">Hello klientské knihovny pro ruční instalaci jsou zahrnuty v balíčcích funkcí systému SQL Server jako distribuovatelného balíčky.</span><span class="sxs-lookup"><span data-stu-id="8762a-114">hello client libraries for manual installation are included in SQL Server feature packs as distributable packages.</span></span> <span data-ttu-id="8762a-115">Tyto knihovny klienta však jsou vázané toohello verze systému SQL Server a nemusí být hello nejnovější.</span><span class="sxs-lookup"><span data-stu-id="8762a-115">However, these client libraries are tied toohello SQL Server version and may not be hello latest.</span></span>  

<span data-ttu-id="8762a-116">Klientské knihovny pro připojení klientů se liší od požadované tooconnect zprostředkovatelé dat ze zdroje dat tooa serveru Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="8762a-116">Client libraries for client connections are different from data providers required tooconnect from an Azure Analysis Services server tooa data source.</span></span> <span data-ttu-id="8762a-117">toolearn Další informace o připojení zdroje dat, najdete v části [připojení zdroje dat](analysis-services-datasource.md).</span><span class="sxs-lookup"><span data-stu-id="8762a-117">toolearn more about datasource connections, see [Datasource connections](analysis-services-datasource.md).</span></span>

## <a name="download-hello-latest-client-libraries"></a><span data-ttu-id="8762a-118">Stáhněte si nejnovější knihovny klienta hello</span><span class="sxs-lookup"><span data-stu-id="8762a-118">Download hello latest client libraries</span></span>  
<span data-ttu-id="8762a-119">Použijte následující klientské knihovny, pokud jsou v produkčním prostředí a nevyžadují plně vydaná a podporovaných verzích hello.</span><span class="sxs-lookup"><span data-stu-id="8762a-119">Use hello following client libraries if you are in a production environment and require fully released and supported versions.</span></span>

[<span data-ttu-id="8762a-120">MSOLAP (amd64)</span><span class="sxs-lookup"><span data-stu-id="8762a-120">MSOLAP (amd64)</span></span>](https://go.microsoft.com/fwlink/?linkid=829576)</br><span data-ttu-id="8762a-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span><span class="sxs-lookup"><span data-stu-id="8762a-121">
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</span></span></br><span data-ttu-id="8762a-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span><span class="sxs-lookup"><span data-stu-id="8762a-122">
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</span></span></br><span data-ttu-id="8762a-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span><span class="sxs-lookup"><span data-stu-id="8762a-123">
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</span></span></br>

## <a name="next-steps"></a><span data-ttu-id="8762a-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8762a-124">Next steps</span></span>
<span data-ttu-id="8762a-125">[Připojit pomocí aplikace Excel](analysis-services-connect-excel.md)  </span><span class="sxs-lookup"><span data-stu-id="8762a-125">[Connect with Excel](analysis-services-connect-excel.md)  </span></span>  
[<span data-ttu-id="8762a-126">Propojení s Power BI</span><span class="sxs-lookup"><span data-stu-id="8762a-126">Connect with Power BI</span></span>](analysis-services-connect-pbi.md)

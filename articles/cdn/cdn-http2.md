---
title: Podpora protokolu HTTP nebo 2 v Azure CDN | Microsoft Docs
description: "Další informace o podpoře protokolu HTTP/2 a CDN."
services: cdn
documentationcenter: 
author: lichard
manager: erikre
editor: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: 4f8dd685c3ae89535217d7a17a01c5129ca7e6e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="http2-support-in-azure-cdn"></a><span data-ttu-id="bff99-103">Podpora protokolu HTTP nebo 2 v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="bff99-103">HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="bff99-104">HTTP/2 je hlavní revizi k HTTP/1.1\.</span><span class="sxs-lookup"><span data-stu-id="bff99-104">HTTP/2 is a major revision to HTTP/1.1\.</span></span> <span data-ttu-id="bff99-105">Nabízí rychlejší webové výkon, sníženou odezvu a vylepšené uživatelské prostředí, při zachování známé metody HTTP, stavové kódy a sémantiku.</span><span class="sxs-lookup"><span data-stu-id="bff99-105">It provides faster web performance, reduced response time, and improved user experience, while maintaining the familiar HTTP methods, status codes, and semantics.</span></span> <span data-ttu-id="bff99-106">I když HTTP/2 je navržený na práci s protokoly HTTP a HTTPS, mnoha webových prohlížečích klienta přes protokol TLS podporují pouze HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="bff99-106">Though HTTP/2 is designed to work with HTTP and HTTPS, many client web browsers only support HTTP/2 over TLS.</span></span>

###<a name="http2-benefits"></a><span data-ttu-id="bff99-107">Výhody HTTP/2</span><span class="sxs-lookup"><span data-stu-id="bff99-107">HTTP/2 Benefits</span></span>

<span data-ttu-id="bff99-108">Příklady výhod HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="bff99-108">The benefits of HTTP/2 include:</span></span>

*   <span data-ttu-id="bff99-109">**Multiplexní a souběžnost**</span><span class="sxs-lookup"><span data-stu-id="bff99-109">**Multiplexing and concurrency**</span></span>

    <span data-ttu-id="bff99-110">Pomocí protokolu HTTP 1.1, více provádění více požadavků prostředků vyžaduje více připojení TCP a každé připojení má zatížení s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="bff99-110">Using HTTP 1.1, multiple making multiple resource requests requires multiple TCP connections, and each connection has performance overhead associated with it.</span></span> <span data-ttu-id="bff99-111">HTTP/2 umožňuje více prostředky vyžadované na jednoho připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="bff99-111">HTTP/2 allows multiple resources to be requested on a single TCP connection.</span></span>

*   <span data-ttu-id="bff99-112">**Komprese záhlaví**</span><span class="sxs-lookup"><span data-stu-id="bff99-112">**Header compression**</span></span>

    <span data-ttu-id="bff99-113">Komprimací hlavičky protokolu HTTP pro obsloužit prostředky, se výrazně snižuje čas v drátové síti.</span><span class="sxs-lookup"><span data-stu-id="bff99-113">By compressing the HTTP headers for served resources, time on the wire is reduced significantly.</span></span>

*   <span data-ttu-id="bff99-114">**Datový proud závislosti**</span><span class="sxs-lookup"><span data-stu-id="bff99-114">**Stream dependencies**</span></span>

    <span data-ttu-id="bff99-115">Datový proud závislosti umožňují klientu označení serveru, který prostředků mají prioritu.</span><span class="sxs-lookup"><span data-stu-id="bff99-115">Stream dependencies allow the client to indicate to the server which of resources have priority.</span></span>


##<a name="http2-browser-support"></a><span data-ttu-id="bff99-116">Podpora prohlížeče HTTP/2</span><span class="sxs-lookup"><span data-stu-id="bff99-116">HTTP/2 Browser Support</span></span>

<span data-ttu-id="bff99-117">Všechny hlavní prohlížeče v jejich aktuální verze implementovat podporu protokolu HTTP nebo 2.</span><span class="sxs-lookup"><span data-stu-id="bff99-117">All of the major browsers have implemented HTTP/2 support in their current versions.</span></span> <span data-ttu-id="bff99-118">Nepodporovaná prohlížečů bude automaticky záložnímu HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="bff99-118">Non-supported browsers will automatically fallback to HTTP/1.1.</span></span>

|<span data-ttu-id="bff99-119">Prohlížeč</span><span class="sxs-lookup"><span data-stu-id="bff99-119">Browser</span></span>|<span data-ttu-id="bff99-120">Minimální verze</span><span class="sxs-lookup"><span data-stu-id="bff99-120">Minimum Version</span></span>|
|-------------|------------|
|<span data-ttu-id="bff99-121">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="bff99-121">Microsoft Edge</span></span>| <span data-ttu-id="bff99-122">12</span><span class="sxs-lookup"><span data-stu-id="bff99-122">12</span></span>|
|<span data-ttu-id="bff99-123">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="bff99-123">Google Chrome</span></span>| <span data-ttu-id="bff99-124">43</span><span class="sxs-lookup"><span data-stu-id="bff99-124">43</span></span>|
|<span data-ttu-id="bff99-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="bff99-125">Mozilla Firefox</span></span>| <span data-ttu-id="bff99-126">38</span><span class="sxs-lookup"><span data-stu-id="bff99-126">38</span></span>|
|<span data-ttu-id="bff99-127">Opera</span><span class="sxs-lookup"><span data-stu-id="bff99-127">Opera</span></span>| <span data-ttu-id="bff99-128">32</span><span class="sxs-lookup"><span data-stu-id="bff99-128">32</span></span>|
|<span data-ttu-id="bff99-129">Safari</span><span class="sxs-lookup"><span data-stu-id="bff99-129">Safari</span></span>| <span data-ttu-id="bff99-130">9</span><span class="sxs-lookup"><span data-stu-id="bff99-130">9</span></span>|

##<a name="enabling-http2-support-in-azure-cdn"></a><span data-ttu-id="bff99-131">Povolení podpory protokolu HTTP nebo 2 v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="bff99-131">Enabling HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="bff99-132">Aktuálně je aktivní pro podporu protokolu HTTP/2 **Azure CDN společnosti Akamai** a **Azure CDN společnosti Verizon** profily.</span><span class="sxs-lookup"><span data-stu-id="bff99-132">Currently HTTP/2 support is active for **Azure CDN from Akamai** and **Azure CDN from Verizon** profiles.</span></span> <span data-ttu-id="bff99-133">Žádná další akce je vyžadována od zákazníků.</span><span class="sxs-lookup"><span data-stu-id="bff99-133">No further action is required from customers.</span></span>

##<a name="next-steps"></a><span data-ttu-id="bff99-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bff99-134">Next Steps</span></span>

<span data-ttu-id="bff99-135">Výhody HTTP/2 v praxi, najdete v sekci [tuto ukázku společnosti Akamai](https://http2.akamai.com/demo).</span><span class="sxs-lookup"><span data-stu-id="bff99-135">To see the benefits of HTTP/2 in action, see [this demo from Akamai](https://http2.akamai.com/demo).</span></span>

<span data-ttu-id="bff99-136">Další informace o protokolu HTTP nebo 2, najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="bff99-136">To learn more about HTTP/2, visit the following resources:</span></span>

*   [<span data-ttu-id="bff99-137">Specifikace protokolu HTTP/2 domovské stránky</span><span class="sxs-lookup"><span data-stu-id="bff99-137">HTTP/2 specification homepage</span></span>](https://http2.github.io/)
*   [<span data-ttu-id="bff99-138">Oficiální HTTP/2 – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="bff99-138">Official HTTP/2 FAQ</span></span>](https://http2.github.io/faq/)
*   [<span data-ttu-id="bff99-139">Informace o Akamai HTTP/2</span><span class="sxs-lookup"><span data-stu-id="bff99-139">Akamai HTTP/2 information</span></span>](https://http2.akamai.com/)

<span data-ttu-id="bff99-140">Další informace o dostupných funkcí Azure CDN najdete v tématu [přehled CDN Azure](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span><span class="sxs-lookup"><span data-stu-id="bff99-140">To learn more about Azure CDN's available features, see the [Azure CDN Overview](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span></span>
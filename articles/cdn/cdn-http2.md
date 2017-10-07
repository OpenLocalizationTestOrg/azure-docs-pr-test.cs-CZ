---
title: Podpora aaaHTTP nebo 2 v Azure CDN | Microsoft Docs
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
ms.openlocfilehash: 2e5e5345e8cf5c40e080ebf18b4f13a239a5aac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="http2-support-in-azure-cdn"></a><span data-ttu-id="c534a-103">Podpora protokolu HTTP nebo 2 v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="c534a-103">HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="c534a-104">HTTP/2 je tooHTTP/1.1\ hlavní revize.</span><span class="sxs-lookup"><span data-stu-id="c534a-104">HTTP/2 is a major revision tooHTTP/1.1\.</span></span> <span data-ttu-id="c534a-105">Nabízí rychlejší webové výkon, sníženou odezvu a vylepšené uživatelské prostředí, při zachování hello známé metody HTTP, stavové kódy a sémantiku.</span><span class="sxs-lookup"><span data-stu-id="c534a-105">It provides faster web performance, reduced response time, and improved user experience, while maintaining hello familiar HTTP methods, status codes, and semantics.</span></span> <span data-ttu-id="c534a-106">I když HTTP/2 je navrženou toowork přes protokol HTTP a HTTPS, mnoha webových prohlížečích klienta přes protokol TLS podporují pouze HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="c534a-106">Though HTTP/2 is designed toowork with HTTP and HTTPS, many client web browsers only support HTTP/2 over TLS.</span></span>

###<a name="http2-benefits"></a><span data-ttu-id="c534a-107">Výhody HTTP/2</span><span class="sxs-lookup"><span data-stu-id="c534a-107">HTTP/2 Benefits</span></span>

<span data-ttu-id="c534a-108">Příklady výhod Hello HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="c534a-108">hello benefits of HTTP/2 include:</span></span>

*   <span data-ttu-id="c534a-109">**Multiplexní a souběžnost**</span><span class="sxs-lookup"><span data-stu-id="c534a-109">**Multiplexing and concurrency**</span></span>

    <span data-ttu-id="c534a-110">Pomocí protokolu HTTP 1.1, více provádění více požadavků prostředků vyžaduje více připojení TCP a každé připojení má zatížení s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="c534a-110">Using HTTP 1.1, multiple making multiple resource requests requires multiple TCP connections, and each connection has performance overhead associated with it.</span></span> <span data-ttu-id="c534a-111">HTTP/2 umožňuje více prostředků toobe požadovaný jednoho připojení TCP.</span><span class="sxs-lookup"><span data-stu-id="c534a-111">HTTP/2 allows multiple resources toobe requested on a single TCP connection.</span></span>

*   <span data-ttu-id="c534a-112">**Komprese záhlaví**</span><span class="sxs-lookup"><span data-stu-id="c534a-112">**Header compression**</span></span>

    <span data-ttu-id="c534a-113">Komprimací hello HTTP hlavičky pro obsloužit prostředky se výrazně snižuje času na hello přenosová.</span><span class="sxs-lookup"><span data-stu-id="c534a-113">By compressing hello HTTP headers for served resources, time on hello wire is reduced significantly.</span></span>

*   <span data-ttu-id="c534a-114">**Datový proud závislosti**</span><span class="sxs-lookup"><span data-stu-id="c534a-114">**Stream dependencies**</span></span>

    <span data-ttu-id="c534a-115">Datový proud závislosti povolit hello klienta tooindicate toohello server, který prostředků prioritou.</span><span class="sxs-lookup"><span data-stu-id="c534a-115">Stream dependencies allow hello client tooindicate toohello server which of resources have priority.</span></span>


##<a name="http2-browser-support"></a><span data-ttu-id="c534a-116">Podpora prohlížeče HTTP/2</span><span class="sxs-lookup"><span data-stu-id="c534a-116">HTTP/2 Browser Support</span></span>

<span data-ttu-id="c534a-117">Všechny hlavní prohlížeče hello v jejich aktuální verze implementovat podporu protokolu HTTP nebo 2.</span><span class="sxs-lookup"><span data-stu-id="c534a-117">All of hello major browsers have implemented HTTP/2 support in their current versions.</span></span> <span data-ttu-id="c534a-118">Nepodporovaná prohlížečů bude automaticky záložní tooHTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="c534a-118">Non-supported browsers will automatically fallback tooHTTP/1.1.</span></span>

|<span data-ttu-id="c534a-119">Prohlížeč</span><span class="sxs-lookup"><span data-stu-id="c534a-119">Browser</span></span>|<span data-ttu-id="c534a-120">Minimální verze</span><span class="sxs-lookup"><span data-stu-id="c534a-120">Minimum Version</span></span>|
|-------------|------------|
|<span data-ttu-id="c534a-121">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="c534a-121">Microsoft Edge</span></span>| <span data-ttu-id="c534a-122">12</span><span class="sxs-lookup"><span data-stu-id="c534a-122">12</span></span>|
|<span data-ttu-id="c534a-123">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="c534a-123">Google Chrome</span></span>| <span data-ttu-id="c534a-124">43</span><span class="sxs-lookup"><span data-stu-id="c534a-124">43</span></span>|
|<span data-ttu-id="c534a-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="c534a-125">Mozilla Firefox</span></span>| <span data-ttu-id="c534a-126">38</span><span class="sxs-lookup"><span data-stu-id="c534a-126">38</span></span>|
|<span data-ttu-id="c534a-127">Opera</span><span class="sxs-lookup"><span data-stu-id="c534a-127">Opera</span></span>| <span data-ttu-id="c534a-128">32</span><span class="sxs-lookup"><span data-stu-id="c534a-128">32</span></span>|
|<span data-ttu-id="c534a-129">Safari</span><span class="sxs-lookup"><span data-stu-id="c534a-129">Safari</span></span>| <span data-ttu-id="c534a-130">9</span><span class="sxs-lookup"><span data-stu-id="c534a-130">9</span></span>|

##<a name="enabling-http2-support-in-azure-cdn"></a><span data-ttu-id="c534a-131">Povolení podpory protokolu HTTP nebo 2 v Azure CDN</span><span class="sxs-lookup"><span data-stu-id="c534a-131">Enabling HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="c534a-132">Aktuálně je aktivní pro podporu protokolu HTTP/2 **Azure CDN společnosti Akamai** a **Azure CDN společnosti Verizon** profily.</span><span class="sxs-lookup"><span data-stu-id="c534a-132">Currently HTTP/2 support is active for **Azure CDN from Akamai** and **Azure CDN from Verizon** profiles.</span></span> <span data-ttu-id="c534a-133">Žádná další akce je vyžadována od zákazníků.</span><span class="sxs-lookup"><span data-stu-id="c534a-133">No further action is required from customers.</span></span>

##<a name="next-steps"></a><span data-ttu-id="c534a-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c534a-134">Next Steps</span></span>

<span data-ttu-id="c534a-135">toosee hello výhody HTTP/2 v praxi, najdete v části [tuto ukázku společnosti Akamai](https://http2.akamai.com/demo).</span><span class="sxs-lookup"><span data-stu-id="c534a-135">toosee hello benefits of HTTP/2 in action, see [this demo from Akamai](https://http2.akamai.com/demo).</span></span>

<span data-ttu-id="c534a-136">Další informace o protokolu HTTP nebo 2, toolearn najdete na adrese hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="c534a-136">toolearn more about HTTP/2, visit hello following resources:</span></span>

*   [<span data-ttu-id="c534a-137">Specifikace protokolu HTTP/2 domovské stránky</span><span class="sxs-lookup"><span data-stu-id="c534a-137">HTTP/2 specification homepage</span></span>](https://http2.github.io/)
*   [<span data-ttu-id="c534a-138">Oficiální HTTP/2 – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="c534a-138">Official HTTP/2 FAQ</span></span>](https://http2.github.io/faq/)
*   [<span data-ttu-id="c534a-139">Informace o Akamai HTTP/2</span><span class="sxs-lookup"><span data-stu-id="c534a-139">Akamai HTTP/2 information</span></span>](https://http2.akamai.com/)

<span data-ttu-id="c534a-140">toolearn Další informace o dostupných funkcí Azure CDN, najdete v části hello [přehled CDN Azure](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span><span class="sxs-lookup"><span data-stu-id="c534a-140">toolearn more about Azure CDN's available features, see hello [Azure CDN Overview](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span></span>

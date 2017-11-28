---
title: aaaEnabling end tooend protokolu SSL na Azure Application Gateway | Microsoft Docs
description: "Tato stránka obsahuje přehled hello Application Gateway end tooend podporu protokolu SSL."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: amsriva
ms.openlocfilehash: c5cb398a1e7d9a08662a3120baad98edb5575917
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-end-tooend-ssl-with-application-gateway"></a><span data-ttu-id="55a3a-103">Přehled end tooend SSL s aplikační brány</span><span class="sxs-lookup"><span data-stu-id="55a3a-103">Overview of end tooend SSL with Application Gateway</span></span>

<span data-ttu-id="55a3a-104">Aplikační brána podporuje ukončení protokolu SSL na hello brány, po které obvykle přenosy dat bez šifrování toohello back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="55a3a-104">Application gateway supports SSL termination at hello gateway, after which traffic typically flows unencrypted toohello backend servers.</span></span> <span data-ttu-id="55a3a-105">Tato funkce umožňuje webové servery toobe unburdened z nákladná režii šifrování a dešifrování.</span><span class="sxs-lookup"><span data-stu-id="55a3a-105">This feature allows web servers toobe unburdened from costly encryption and decryption overhead.</span></span> <span data-ttu-id="55a3a-106">Ale někteří zákazníci nešifrovaná komunikace toohello back-end serverech není přijatelné možnost.</span><span class="sxs-lookup"><span data-stu-id="55a3a-106">However for some customers unencrypted communication toohello backend servers is not an acceptable option.</span></span> <span data-ttu-id="55a3a-107">Tato nešifrovaná komunikace může být z důvodu toosecurity požadavky, požadavky na dodržování předpisů, nebo hello aplikace přijímá pouze zabezpečené připojení.</span><span class="sxs-lookup"><span data-stu-id="55a3a-107">This unencrypted communication could be due toosecurity requirements, compliance requirements, or hello application may only accept a secure connection.</span></span> <span data-ttu-id="55a3a-108">Pro tyto aplikace, aplikační brána podporuje end tooend SSL šifrování.</span><span class="sxs-lookup"><span data-stu-id="55a3a-108">For such applications, application gateway supports end tooend SSL encryption.</span></span>

## <a name="overview"></a><span data-ttu-id="55a3a-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="55a3a-109">Overview</span></span>

<span data-ttu-id="55a3a-110">End tooend SSL můžete toosecurely přenášet back-end toohello citlivá data šifrují, když poskytuje i nadále využívat výhod hello výhod funkce Vyrovnávání zatížení vrstvy 7 které aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="55a3a-110">End tooend SSL allows you toosecurely transmit sensitive data toohello backend encrypted while still taking advantage of hello benefits of Layer 7 load balancing features which application gateway provides.</span></span> <span data-ttu-id="55a3a-111">Některé z těchto funkcí jsou spřažení na základě souboru cookie relace, na základě adresy URL směrování, podporu pro směrování na základě lokality, nebo možnost tooinject X - předávaných-* hlavičky.</span><span class="sxs-lookup"><span data-stu-id="55a3a-111">Some of these features are cookie-based session affinity, URL-based routing, support for routing based on sites, or ability tooinject X-Forwarded-* headers.</span></span>

<span data-ttu-id="55a3a-112">Jestliže nakonfigurované s režimem komunikace SSL tooend end, aplikační brána hello relací SSL na hello brány a dešifruje provozu generovaného uživateli.</span><span class="sxs-lookup"><span data-stu-id="55a3a-112">When configured with end tooend SSL communication mode, application gateway terminates hello SSL sessions at hello gateway and decrypts user traffic.</span></span> <span data-ttu-id="55a3a-113">Pak provede hello nakonfigurovaná pravidla tooselect odpovídající back-end fondu instance tooroute provoz do.</span><span class="sxs-lookup"><span data-stu-id="55a3a-113">It then applies hello configured rules tooselect an appropriate backend pool instance tooroute traffic to.</span></span> <span data-ttu-id="55a3a-114">Aplikační brána pak inicializuje nový toohello back-end server připojení SSL a znovu je zašifruje data před přenosem hello požadavek toohello back-end pomocí certifikátu veřejného klíče hello back-end serveru.</span><span class="sxs-lookup"><span data-stu-id="55a3a-114">Application gateway then initiates a new SSL connection toohello backend server and re-encrypts data using hello backend server's public key certificate before transmitting hello request toohello backend.</span></span> <span data-ttu-id="55a3a-115">End tooend, který je povolen protokol SSL nastavením nastavení protokolu v BackendHTTPSetting tooHTTPS, který je pak použije tooa back-endový fond.</span><span class="sxs-lookup"><span data-stu-id="55a3a-115">End tooend SSL is enabled by setting protocol setting in BackendHTTPSetting tooHTTPS, which is then applied tooa backend pool.</span></span> <span data-ttu-id="55a3a-116">Každý server back-end ve fondu back-end hello s tooend end, který povolen protokol SSL musí být nakonfigurované zabezpečené komunikace tooallow certifikátu.</span><span class="sxs-lookup"><span data-stu-id="55a3a-116">Each backend server in hello backend pool with end tooend SSL enabled must be configured with a certificate tooallow secure communication.</span></span>

![end tooend ssl scénář][1]

<span data-ttu-id="55a3a-118">V tomto příkladu požadavky pomocí TLS1.2 jsou směrované toobackend v Pool1 pomocí end tooend SSL.</span><span class="sxs-lookup"><span data-stu-id="55a3a-118">In this example, requests using TLS1.2 are routed toobackend servers in Pool1 using end tooend SSL.</span></span>

## <a name="end-tooend-ssl-and-whitelisting-of-certificates"></a><span data-ttu-id="55a3a-119">Ukončení tooend SSL a povolených certifikátů</span><span class="sxs-lookup"><span data-stu-id="55a3a-119">End tooend SSL and whitelisting of certificates</span></span>

<span data-ttu-id="55a3a-120">Aplikační brána komunikuje pouze se službou známé back-end instancí, které mají seznam povolených adres jejich certifikát s hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="55a3a-120">Application gateway only communicates with known backend instances that have whitelisted their certificate with hello application gateway.</span></span> <span data-ttu-id="55a3a-121">tooenable povolených certifikáty, musíte nahrát hello veřejný klíč back-end serveru certifikáty toohello aplikační brány (ne hello kořenový certifikát).</span><span class="sxs-lookup"><span data-stu-id="55a3a-121">tooenable whitelisting of certificates, you must upload hello public key of backend server certificates toohello application gateway (not hello root certificate).</span></span> <span data-ttu-id="55a3a-122">Pouze připojení tooknown a seznam povolených adres back-EndY jsou poté povoleny.</span><span class="sxs-lookup"><span data-stu-id="55a3a-122">Only connections tooknown and whitelisted backends are then allowed.</span></span> <span data-ttu-id="55a3a-123">Hello zbývající back-EndY výsledkem chyba brány.</span><span class="sxs-lookup"><span data-stu-id="55a3a-123">hello remaining backends results in a gateway error.</span></span> <span data-ttu-id="55a3a-124">Certifikáty podepsané svým držitelem slouží pouze k testování a nedoporučují se pro úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="55a3a-124">Self-signed certificates are for test purposes only and not recommended for production workloads.</span></span> <span data-ttu-id="55a3a-125">Tyto certifikáty mít seznam povolených adres toobe s hello aplikační bránu, jak je popsáno v předchozích krocích před použitím hello.</span><span class="sxs-lookup"><span data-stu-id="55a3a-125">Such certificates have toobe whitelisted with hello application gateway as described in hello preceding steps before they can be used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="55a3a-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55a3a-126">Next steps</span></span>

<span data-ttu-id="55a3a-127">Po získání informací o end tooend SSL, přejděte příliš[povolit koncové tooend SSL na aplikační brána](application-gateway-end-to-end-ssl-powershell.md) toocreate brány aplikace pomocí ukončení tooend SSL.</span><span class="sxs-lookup"><span data-stu-id="55a3a-127">After learning about end tooend SSL, go too[enable end tooend SSL on application gateway](application-gateway-end-to-end-ssl-powershell.md) toocreate an application gateway using end tooend SSL.</span></span>

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png

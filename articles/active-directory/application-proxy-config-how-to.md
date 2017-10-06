---
title: aaaHow tooconfigure aplikaci Proxy aplikace | Microsoft Docs
description: "Zjistěte, jak toocreate aplikaci Proxy aplikace v několika jednoduchých kroků konfigurace"
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
ms.openlocfilehash: c64019098fc124e4fe10b8288830bcd2b7239d3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application"></a><span data-ttu-id="c032d-103">Jak tooconfigure aplikaci Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="c032d-103">How tooconfigure an Application Proxy application</span></span>

<span data-ttu-id="c032d-104">Tento článek vám pomůže toounderstand jak tooconfigure aplikaci Proxy aplikace v rámci Azure AD tooexpose vaše místní aplikace toohello cloudové.</span><span class="sxs-lookup"><span data-stu-id="c032d-104">This article help you toounderstand how tooconfigure an Application Proxy application within Azure AD tooexpose your on-premises applications toohello cloud.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="c032d-105">Doporučené dokumenty</span><span class="sxs-lookup"><span data-stu-id="c032d-105">Recommended documents</span></span> 

<span data-ttu-id="c032d-106">toolearn o hello počáteční konfigurace a vytvoření aplikace Proxy aplikací prostřednictvím hello portál pro správu, postupujte podle hello [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="c032d-106">toolearn about hello initial configurations and creation of an Application Proxy application through hello Admin Portal, follow hello [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="c032d-107">Podrobnosti o konfiguraci konektorů najdete v tématu [povolení Proxy aplikace v portálu Azure hello](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="c032d-107">For details on configuring Connectors, see [Enable Application Proxy in hello Azure portal](active-directory-application-proxy-enable.md).</span></span>

<span data-ttu-id="c032d-108">Informace o nahrávání certifikátů a používání vlastní domény najdete v tématu [práce s vlastní domény v Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="c032d-108">For information on uploading certificates and using custom domains, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span>

## <a name="create-hello-applicationsetting-hello-urls"></a><span data-ttu-id="c032d-109">Vytvoření hello hello aplikace nebo nastavení adresy URL</span><span class="sxs-lookup"><span data-stu-id="c032d-109">Create hello Application/Setting hello URLs</span></span>

<span data-ttu-id="c032d-110">Pokud postupujete podle hello kroky hello [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) dokumentace a jsou získávání k chybě při vytváření aplikace hello, najdete v části Podrobnosti o chybě hello informace a návrhy, jak toofix hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c032d-110">If you are following hello steps in hello [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) documentation and are getting an error creating hello application, see hello error details for information and suggestions for how toofix hello application.</span></span> <span data-ttu-id="c032d-111">Většina chybové zprávy, obsahuje navrhované opravu.</span><span class="sxs-lookup"><span data-stu-id="c032d-111">Most error messages include a suggested fix.</span></span> <span data-ttu-id="c032d-112">tooavoid běžné chyby, zkontrolujte:</span><span class="sxs-lookup"><span data-stu-id="c032d-112">tooavoid common errors, verify:</span></span>

-   <span data-ttu-id="c032d-113">Jste přihlášeni jako správce s toocreate oprávnění aplikace Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="c032d-113">You are an administrator with permission toocreate an Application Proxy application</span></span>

-   <span data-ttu-id="c032d-114">Interní adresa URL Hello je jedinečný</span><span class="sxs-lookup"><span data-stu-id="c032d-114">hello internal URL is unique</span></span>

-   <span data-ttu-id="c032d-115">externí adresu URL Hello je jedinečný</span><span class="sxs-lookup"><span data-stu-id="c032d-115">hello external URL is unique</span></span>

-   <span data-ttu-id="c032d-116">Hello vycházejte adresy URL protokolu http nebo https a končit "/"</span><span class="sxs-lookup"><span data-stu-id="c032d-116">hello URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="c032d-117">Adresa URL Hello by měl být název domény, nikoli IP adresu</span><span class="sxs-lookup"><span data-stu-id="c032d-117">hello URL should be a domain name, not an IP address</span></span>

<span data-ttu-id="c032d-118">Hello chybová zpráva by měla zobrazit v pravém horním rohu hello při vytvoření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="c032d-118">hello error message should display in hello top right corner when you create hello application.</span></span> <span data-ttu-id="c032d-119">Můžete také vybrat hello oznámení ikonu toosee hello chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="c032d-119">You can also select hello notification icon toosee hello error messages.</span></span>

   ![Oznámení řádku](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a><span data-ttu-id="c032d-121">Nakonfigurujte konektory nebo konektor skupiny</span><span class="sxs-lookup"><span data-stu-id="c032d-121">Configure connectors/connector groups</span></span>

<span data-ttu-id="c032d-122">Pokud máte potíže s konfigurací aplikace z důvodu upozornění o hello konektory a konektor skupiny, najdete pokyny k povolení Proxy aplikace získáte informace o tom toodownload konektory.</span><span class="sxs-lookup"><span data-stu-id="c032d-122">If you are having difficulty configuring your application because of warning about hello connectors and connector groups, see instructions on enabling Application Proxy for details on how toodownload connectors.</span></span> <span data-ttu-id="c032d-123">Pokud chcete, aby toolearn více informací o konektory, přečtěte si téma hello [konektory dokumentaci](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span><span class="sxs-lookup"><span data-stu-id="c032d-123">If you want toolearn more about connectors, see hello [connectors documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span></span>

<span data-ttu-id="c032d-124">Pokud vaše konektory jsou neaktivní, znamená to, že jsou služby nelze tooreach hello.</span><span class="sxs-lookup"><span data-stu-id="c032d-124">If your connectors are inactive, this means that they are unable tooreach hello service.</span></span> <span data-ttu-id="c032d-125">Často je vzhledem k tomu, že všechny hello požadované porty nejsou otevřené.</span><span class="sxs-lookup"><span data-stu-id="c032d-125">This is often because all hello required ports are not open.</span></span> <span data-ttu-id="c032d-126">toosee seznam požadované porty, najdete v části předpoklady hello hello povolení Proxy aplikace dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="c032d-126">toosee a list of required ports, see hello pre-requisites section of hello enabling Application Proxy documentation.</span></span>

## <a name="upload-certificates-for-custom-domains"></a><span data-ttu-id="c032d-127">Nahrát certifikáty pro vlastní domény</span><span class="sxs-lookup"><span data-stu-id="c032d-127">Upload certificates for custom domains</span></span>

<span data-ttu-id="c032d-128">Vlastní domény povolit toospecify hello doménu vaší externí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="c032d-128">Custom Domains allow you toospecify hello domain of your external URLs.</span></span> <span data-ttu-id="c032d-129">vlastní domény toouse, potřebujete certifikát hello tooupload pro tuto doménu.</span><span class="sxs-lookup"><span data-stu-id="c032d-129">toouse custom domains, you need tooupload hello certificate for that domain.</span></span> <span data-ttu-id="c032d-130">Informace o používání vlastní domény a certifikáty, najdete v části [práce s vlastní domény v Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="c032d-130">For information on using custom domains and certificates, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span> 

<span data-ttu-id="c032d-131">Pokud narazíte na problémy odeslání vašeho certifikátu, vyhledejte hello chybové zprávy hello portálu a další informace o hello potížím s certifikátem hello.</span><span class="sxs-lookup"><span data-stu-id="c032d-131">If you are encountering issues uploading your certificate, look for hello error messages in hello portal for additional information on hello problem with hello certificate.</span></span> <span data-ttu-id="c032d-132">Běžné problémy s certifikátem patří:</span><span class="sxs-lookup"><span data-stu-id="c032d-132">Common certificate problems include:</span></span>

-   <span data-ttu-id="c032d-133">Vypršela platnost certifikátu</span><span class="sxs-lookup"><span data-stu-id="c032d-133">Expired certificate</span></span>

-   <span data-ttu-id="c032d-134">Certifikát je podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="c032d-134">Certificate is self-signed</span></span>

-   <span data-ttu-id="c032d-135">Certifikát chybí privátní klíč hello</span><span class="sxs-lookup"><span data-stu-id="c032d-135">Certificate is missing hello private key</span></span>

<span data-ttu-id="c032d-136">zobrazení chybových zpráv Hello v hello pravém horním rohu od vás tooupload hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c032d-136">hello error message display in hello top right corner as you try tooupload hello certificate.</span></span> <span data-ttu-id="c032d-137">Můžete také vybrat hello oznámení ikonu toosee hello chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="c032d-137">You can also select hello notification icon toosee hello error messages.</span></span>

   ![Oznámení řádku](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a><span data-ttu-id="c032d-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c032d-139">Next steps</span></span>
[<span data-ttu-id="c032d-140">Publikování aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c032d-140">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)

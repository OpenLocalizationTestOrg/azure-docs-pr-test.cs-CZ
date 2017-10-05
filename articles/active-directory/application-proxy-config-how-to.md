---
title: Postup konfigurace aplikace Proxy aplikace | Microsoft Docs
description: "Zjistěte, jak vytvořit aplikaci Proxy aplikace konfigurace v několika jednoduchých krocích"
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
ms.openlocfilehash: c8f98536048a85ebb3f061d840457130579196d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-an-application-proxy-application"></a><span data-ttu-id="305fe-103">Postup konfigurace aplikace Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="305fe-103">How to configure an Application Proxy application</span></span>

<span data-ttu-id="305fe-104">Tento článek vám pomůže lépe porozumět konfiguraci aplikaci Proxy aplikace v rámci Azure AD vystavit vaše místní aplikace do cloudu.</span><span class="sxs-lookup"><span data-stu-id="305fe-104">This article help you to understand how to configure an Application Proxy application within Azure AD to expose your on-premises applications to the cloud.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="305fe-105">Doporučené dokumenty</span><span class="sxs-lookup"><span data-stu-id="305fe-105">Recommended documents</span></span> 

<span data-ttu-id="305fe-106">Další informace o vytvoření aplikace Proxy aplikace prostřednictvím portálu pro správu a počáteční konfigurace, postupujte podle [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="305fe-106">To learn about the initial configurations and creation of an Application Proxy application through the Admin Portal, follow the [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="305fe-107">Podrobnosti o konfiguraci konektorů najdete v tématu [povolení Proxy aplikace v portálu Azure](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="305fe-107">For details on configuring Connectors, see [Enable Application Proxy in the Azure portal](active-directory-application-proxy-enable.md).</span></span>

<span data-ttu-id="305fe-108">Informace o nahrávání certifikátů a používání vlastní domény najdete v tématu [práce s vlastní domény v Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="305fe-108">For information on uploading certificates and using custom domains, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span>

## <a name="create-the-applicationsetting-the-urls"></a><span data-ttu-id="305fe-109">Vytvoření aplikace nebo nastavení adresy URL.</span><span class="sxs-lookup"><span data-stu-id="305fe-109">Create the Application/Setting the URLs</span></span>

<span data-ttu-id="305fe-110">Pokud postupujete podle kroků v [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) dokumentace a jsou získávání k chybě při vytváření aplikace, najdete v části Podrobnosti o chybě informace a návrhy, jak opravit aplikaci.</span><span class="sxs-lookup"><span data-stu-id="305fe-110">If you are following the steps in the [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) documentation and are getting an error creating the application, see the error details for information and suggestions for how to fix the application.</span></span> <span data-ttu-id="305fe-111">Většina chybové zprávy, obsahuje navrhované opravu.</span><span class="sxs-lookup"><span data-stu-id="305fe-111">Most error messages include a suggested fix.</span></span> <span data-ttu-id="305fe-112">Aby se zabránilo běžné chyby, ověřte:</span><span class="sxs-lookup"><span data-stu-id="305fe-112">To avoid common errors, verify:</span></span>

-   <span data-ttu-id="305fe-113">Správci s oprávněním k vytvoření aplikace Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="305fe-113">You are an administrator with permission to create an Application Proxy application</span></span>

-   <span data-ttu-id="305fe-114">Interní adresa URL je jedinečný</span><span class="sxs-lookup"><span data-stu-id="305fe-114">The internal URL is unique</span></span>

-   <span data-ttu-id="305fe-115">Externí adresa URL je jedinečný</span><span class="sxs-lookup"><span data-stu-id="305fe-115">The external URL is unique</span></span>

-   <span data-ttu-id="305fe-116">Adresy URL začínat protokolu http nebo https a končit "/"</span><span class="sxs-lookup"><span data-stu-id="305fe-116">The URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="305fe-117">Adresa URL by měla být název domény, nikoli IP adresu</span><span class="sxs-lookup"><span data-stu-id="305fe-117">The URL should be a domain name, not an IP address</span></span>

<span data-ttu-id="305fe-118">Chybová zpráva by měla zobrazit v pravém horním rohu při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="305fe-118">The error message should display in the top right corner when you create the application.</span></span> <span data-ttu-id="305fe-119">Můžete také vybrat na ikonu oznámení naleznete v chybových zprávách.</span><span class="sxs-lookup"><span data-stu-id="305fe-119">You can also select the notification icon to see the error messages.</span></span>

   ![Oznámení řádku](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a><span data-ttu-id="305fe-121">Nakonfigurujte konektory nebo konektor skupiny</span><span class="sxs-lookup"><span data-stu-id="305fe-121">Configure connectors/connector groups</span></span>

<span data-ttu-id="305fe-122">Pokud máte potíže s konfigurací aplikace z důvodu upozornění o konektorech a konektor skupiny, najdete pokyny k povolení Proxy aplikace podrobnosti o tom, jak stáhnout konektory.</span><span class="sxs-lookup"><span data-stu-id="305fe-122">If you are having difficulty configuring your application because of warning about the connectors and connector groups, see instructions on enabling Application Proxy for details on how to download connectors.</span></span> <span data-ttu-id="305fe-123">Pokud chcete získat další informace o konektory, přečtěte si téma [konektory dokumentaci](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span><span class="sxs-lookup"><span data-stu-id="305fe-123">If you want to learn more about connectors, see the [connectors documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span></span>

<span data-ttu-id="305fe-124">Pokud vaše konektory jsou neaktivní, znamená to, aby byly možné vás zastihnout na službu.</span><span class="sxs-lookup"><span data-stu-id="305fe-124">If your connectors are inactive, this means that they are unable to reach the service.</span></span> <span data-ttu-id="305fe-125">To je často protože nejsou otevřené požadované porty.</span><span class="sxs-lookup"><span data-stu-id="305fe-125">This is often because all the required ports are not open.</span></span> <span data-ttu-id="305fe-126">Pokud chcete zobrazit seznam požadované porty, najdete v části předpoklady povolení Proxy aplikace dokumentace.</span><span class="sxs-lookup"><span data-stu-id="305fe-126">To see a list of required ports, see the pre-requisites section of the enabling Application Proxy documentation.</span></span>

## <a name="upload-certificates-for-custom-domains"></a><span data-ttu-id="305fe-127">Nahrát certifikáty pro vlastní domény</span><span class="sxs-lookup"><span data-stu-id="305fe-127">Upload certificates for custom domains</span></span>

<span data-ttu-id="305fe-128">Vlastní domény umožňují zadejte doménu vaší externí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="305fe-128">Custom Domains allow you to specify the domain of your external URLs.</span></span> <span data-ttu-id="305fe-129">Pokud chcete používat vlastní domény, budete muset nahrát na server certifikát pro tuto doménu.</span><span class="sxs-lookup"><span data-stu-id="305fe-129">To use custom domains, you need to upload the certificate for that domain.</span></span> <span data-ttu-id="305fe-130">Informace o používání vlastní domény a certifikáty, najdete v části [práce s vlastní domény v Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="305fe-130">For information on using custom domains and certificates, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span> 

<span data-ttu-id="305fe-131">Pokud narazíte na problémy odeslání vašeho certifikátu, vyhledejte chybové zprávy na portálu pro další informace o problému s certifikátem.</span><span class="sxs-lookup"><span data-stu-id="305fe-131">If you are encountering issues uploading your certificate, look for the error messages in the portal for additional information on the problem with the certificate.</span></span> <span data-ttu-id="305fe-132">Běžné problémy s certifikátem patří:</span><span class="sxs-lookup"><span data-stu-id="305fe-132">Common certificate problems include:</span></span>

-   <span data-ttu-id="305fe-133">Vypršela platnost certifikátu</span><span class="sxs-lookup"><span data-stu-id="305fe-133">Expired certificate</span></span>

-   <span data-ttu-id="305fe-134">Certifikát je podepsaný svým držitelem</span><span class="sxs-lookup"><span data-stu-id="305fe-134">Certificate is self-signed</span></span>

-   <span data-ttu-id="305fe-135">Certifikát chybí privátní klíč</span><span class="sxs-lookup"><span data-stu-id="305fe-135">Certificate is missing the private key</span></span>

<span data-ttu-id="305fe-136">Chybová zpráva zobrazit v pravém horním rohu jako pokusíte odešlete certifikát.</span><span class="sxs-lookup"><span data-stu-id="305fe-136">The error message display in the top right corner as you try to upload the certificate.</span></span> <span data-ttu-id="305fe-137">Můžete také vybrat na ikonu oznámení naleznete v chybových zprávách.</span><span class="sxs-lookup"><span data-stu-id="305fe-137">You can also select the notification icon to see the error messages.</span></span>

   ![Oznámení řádku](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a><span data-ttu-id="305fe-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="305fe-139">Next steps</span></span>
[<span data-ttu-id="305fe-140">Publikování aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="305fe-140">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)

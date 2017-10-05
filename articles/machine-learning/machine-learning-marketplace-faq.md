---
title: "(nepoužívané) Časté otázky – publikování a používání aplikace Machine Learning v Azure Marketplace | Microsoft Docs"
description: "(nepoužívané) Časté otázky k publikování aplikací Machine Learning v Azure Marketplace"
services: machine-learning
documentationcenter: 
author: bharaths
manager: jhubbard
editor: cgronlun
ms.assetid: 26b3a1e7-8b9a-4004-98bc-17456d4c25e8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: a4631dfeb2f817b3a3c612a8f6af48398e4f2ab9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-the-azure-marketplace-faq"></a><span data-ttu-id="66cbb-103">(nepoužívané) Publikování a použití aplikace Machine Learning v Azure Marketplace: Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="66cbb-103">(deprecated) Publishing and using Machine Learning apps in the Azure Marketplace: FAQ</span></span>

> [!NOTE]
> <span data-ttu-id="66cbb-104">DataMarket a Data služby se postupně vyřazuje z provozu a odběry vyřadí a zrušena od 3/31/2017.</span><span class="sxs-lookup"><span data-stu-id="66cbb-104">DataMarket and Data Services are being retired, and existing subscriptions will be retired and cancelled starting 3/31/2017.</span></span> <span data-ttu-id="66cbb-105">V důsledku toho je zastaralá v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="66cbb-105">As a result, this article is being deprecated.</span></span> 
> 
> <span data-ttu-id="66cbb-106">Jako alternativu, můžete publikovat Machine Learning experimentů k [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) prospěch komunity vědecké účely data.</span><span class="sxs-lookup"><span data-stu-id="66cbb-106">As an alternative, you can publish your Machine Learning experiments to the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) for the benefit of the data science community.</span></span> <span data-ttu-id="66cbb-107">Další informace najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span><span class="sxs-lookup"><span data-stu-id="66cbb-107">For more information, see [Share and discover resources in the Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span></span>


## <a name="questions-about-consuming-from-marketplace"></a><span data-ttu-id="66cbb-108">Dotazy týkající se použití z webu Marketplace</span><span class="sxs-lookup"><span data-stu-id="66cbb-108">Questions about consuming from Marketplace</span></span>
<span data-ttu-id="66cbb-109">**1. Proč se po zadání vstupu pro webovou službu zobrazí následující chybová zpráva:**</span><span class="sxs-lookup"><span data-stu-id="66cbb-109">**1. Why do I get the following error message after I enter input for the web service:**</span></span>

<span data-ttu-id="66cbb-110">**Žádost byla vygenerována v back-end časového limitu nebo chyby back-end. Tým pracuje na odstranění příčin problému. Omlouváme se za nepříjemnosti se. (500)**</span><span class="sxs-lookup"><span data-stu-id="66cbb-110">**The request resulted in a back-end time out or back-end error. The team is investigating the issue. We are sorry for the inconvenience. (500)**</span></span>

<span data-ttu-id="66cbb-111">Vstupní parametry nemusí vyhovovat požadovaný formát pro konkrétní webovou službu.</span><span class="sxs-lookup"><span data-stu-id="66cbb-111">Your input parameter(s) may not conform to the required format for the specific web service.</span></span> <span data-ttu-id="66cbb-112">Podrobnosti naleznete na odpovídající odkaz dokumentaci najít správný formát pro vstupní parametry a omezení této webové služby.</span><span class="sxs-lookup"><span data-stu-id="66cbb-112">Please refer to the corresponding documentation link to find the correct format for input parameters and the limitations of this web service.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="66cbb-113">**2. Pokud lze zkopírovat odkaz na rozhraní API pro webovou službu, která je na stránky "Prozkoumat tuto datovou sadu" a vložíte ho do jiného okna prohlížeče, jaké přihlašovací údaje by je možné používat pro přístup k výsledky a jak je zobrazit?**</span><span class="sxs-lookup"><span data-stu-id="66cbb-113">**2. If I copy the API link for the web service that I see on the "Explore this dataset" page and paste it into another browser window, what credentials should I use to access the results, and how do I see them?**</span></span>

<span data-ttu-id="66cbb-114">Účtu webu Marketplace má použít jako uživatelské jméno a primární účet klíč jako heslo.</span><span class="sxs-lookup"><span data-stu-id="66cbb-114">You should use your Marketplace account as the username and the primary account key as the password.</span></span> <span data-ttu-id="66cbb-115">Klíč primární účtu naleznete na **prozkoumat tuto datovou sadu** stránky v části Popis webové služby (klikněte na tlačítko **zobrazit** tlačítko).</span><span class="sxs-lookup"><span data-stu-id="66cbb-115">The primary account key can be found on the **Explore this dataset** page under the description of the web service (click the **show** button).</span></span> <span data-ttu-id="66cbb-116">Výsledek se může zobrazovat v prohlížeči nebo může být k dispozici ke stažení, v závislosti na tom, které prohlížeče, kterou používáte.</span><span class="sxs-lookup"><span data-stu-id="66cbb-116">The result may display in the browser or it may be available to  download, depending on which browser you are using.</span></span>

<span data-ttu-id="66cbb-117">**3. Proč se po zadání vstupu pro webovou službu na stránce "Prozkoumat tuto datovou sadu" zobrazí následující chybová zpráva:**</span><span class="sxs-lookup"><span data-stu-id="66cbb-117">**3. Why do I get the following error message after I enter the input for the web service on the "Explore this dataset" page:**</span></span> 

<span data-ttu-id="66cbb-118">**Při zpracování vaší žádosti došlo k neočekávané chybě. Zkuste to prosím znovu.**</span><span class="sxs-lookup"><span data-stu-id="66cbb-118">**An unexpected error occurred while processing your request. Please try again.**</span></span>

<span data-ttu-id="66cbb-119">Jeden nebo více vstupních parametrů webové služby byl překročen limit délky při využívání webovou službu na webu marketplace **prozkoumat tuto datovou sadu** stránky.</span><span class="sxs-lookup"><span data-stu-id="66cbb-119">One or more input parameters of your web service may have exceeded the length limit when consuming the web service on the marketplace **Explore this dataset** page.</span></span> <span data-ttu-id="66cbb-120">Službu nelze volat s již vstupní délkou pomocí metod HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="66cbb-120">The services can be called with a longer input length by using HTTP POST methods.</span></span> <span data-ttu-id="66cbb-121">Příklady najdete v tématu [ukázkové řešení pomocí R na Machine Learning a publikované na webu Marketplace](machine-learning-r-csharp-web-service-examples.md).</span><span class="sxs-lookup"><span data-stu-id="66cbb-121">For examples, see [Sample solutions using R on Machine Learning and published to Marketplace](machine-learning-r-csharp-web-service-examples.md).</span></span>

<span data-ttu-id="66cbb-122">**4. Proč nevidím v kartě int "API EXPLORER" úložiště na portálu Azure Classic nic?**</span><span class="sxs-lookup"><span data-stu-id="66cbb-122">**4. Why do I not see anything in the "API EXPLORER" tab int the Store in the Azure Classic Portal?**</span></span> 

<span data-ttu-id="66cbb-123">Jedná se o známý problém s klasického portálu Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="66cbb-123">This is a known issue with the Azure Classic Portal Marketplace.</span></span> <span data-ttu-id="66cbb-124">Tým pracuje k vyřešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="66cbb-124">The team is working to resolve this issue.</span></span> 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a><span data-ttu-id="66cbb-125">Dotazy o publikování z Azure Machine Learning v Marketplace.</span><span class="sxs-lookup"><span data-stu-id="66cbb-125">Questions about publishing from Azure Machine Learning on Marketplace</span></span>
<span data-ttu-id="66cbb-126">**1. Proč nejsou Moje transakce loga nebo bitové kopie aktualizace pro webovou službu?**</span><span class="sxs-lookup"><span data-stu-id="66cbb-126">**1. Why are my transactions of logos or images not refreshing for my web service?**</span></span> 

<span data-ttu-id="66cbb-127">Loga a image jsou uložené v mezipaměti v portálu pro publikování a že může trvat až 10 dnů pro nové logo nebo bitovou kopii k aktualizaci na portálu.</span><span class="sxs-lookup"><span data-stu-id="66cbb-127">Logos and images are cached in the publishing portal, and it may take up to 10 days for the new logo or image to update on the portal.</span></span>

<span data-ttu-id="66cbb-128">**2. Proč je na kartě "Podrobné" webovou službu na Marketplace zobrazuje chybovou zprávu?**</span><span class="sxs-lookup"><span data-stu-id="66cbb-128">**2. Why is the “Detail" tab of my web service on Marketplace showing an error message?**</span></span>

<span data-ttu-id="66cbb-129">Při připojování k Azure Machine Learning podrobnosti služby není známý problém Marketplace.</span><span class="sxs-lookup"><span data-stu-id="66cbb-129">There is a known Marketplace issue when connecting to Azure Machine Learning for service details.</span></span> <span data-ttu-id="66cbb-130">Tým pracuje k vyřešení tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="66cbb-130">The team is working to resolve this issue.</span></span>

<span data-ttu-id="66cbb-131">**3. Proč ukázkový kód R ve webové služby Azure Machine Learning nefunguje pro využívání webových služeb v Marketplace?**</span><span class="sxs-lookup"><span data-stu-id="66cbb-131">**3. Why does the R sample code in the Azure Machine Learning web services not work for consuming the web services in Marketplace?**</span></span>

<span data-ttu-id="66cbb-132">Systémy ověřování se liší, při připojení k webovým službám Azure Machine Learning přímo ve srovnání s připojení k webovým službám přes Marketplace.</span><span class="sxs-lookup"><span data-stu-id="66cbb-132">The authentication systems are different when connecting to Azure Machine Learning web services directly compared to connecting to these web services through the Marketplace.</span></span> <span data-ttu-id="66cbb-133">Služeb v Marketplace jsou služby OData a volat pomocí metody GET nebo POST.</span><span class="sxs-lookup"><span data-stu-id="66cbb-133">The services in Marketplace are OData services, and they can be called with GET or POST methods.</span></span> 

<span data-ttu-id="66cbb-134">**4. Proč se podporu odkazů Moje webová služba nabízí není správně aktualizace pro některé Moje nabízí?**</span><span class="sxs-lookup"><span data-stu-id="66cbb-134">**4. Why are the support links of my web service offers not updating correctly for some of my offers?**</span></span>

<span data-ttu-id="66cbb-135">Globální za vydavatele, není za nabídka jsou odkazy na podporu.</span><span class="sxs-lookup"><span data-stu-id="66cbb-135">The support links are global per publisher, not per offer.</span></span> 

<span data-ttu-id="66cbb-136">**5. Jak lze publikovat webové služby pomocí vstupní režimu dávky v Marketplace?**</span><span class="sxs-lookup"><span data-stu-id="66cbb-136">**5. How do I publish a web service with batch input mode in Marketplace?**</span></span>

<span data-ttu-id="66cbb-137">Vstupní režim batch není aktuálně podporován v Marketplace webové služby.</span><span class="sxs-lookup"><span data-stu-id="66cbb-137">The batch input mode is currently not supported in Marketplace web services.</span></span>

<span data-ttu-id="66cbb-138">**6. Kdo je měli kontaktovat nápovědu, pokud je nutné dotazy týkající se stává stále vydavatel dat, nebo pokud mám problémy při publikování?**</span><span class="sxs-lookup"><span data-stu-id="66cbb-138">**6. Who should I contact to get help if I have questions about becoming a data publisher, or if I have issues during publishing?**</span></span>

<span data-ttu-id="66cbb-139">Kontaktujte tým služby Azure Marketplace v < mailto:datamarketbd@microsoft.com > Další informace.</span><span class="sxs-lookup"><span data-stu-id="66cbb-139">Please contact the Azure Marketplace team at <mailto:datamarketbd@microsoft.com> for more information.</span></span>


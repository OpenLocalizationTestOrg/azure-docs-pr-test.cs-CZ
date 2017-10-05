---
title: "Co je nového v Azure Machine Learning | Microsoft Docs"
description: "Nové funkce, které jsou k dispozici v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 551b977b90612ddbfa1514a9c2358ebf8179c385
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="whats-new-in-azure-machine-learning"></a><span data-ttu-id="b647a-103">Novinky ve službě Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b647a-103">What's New in Azure Machine Learning</span></span>

### <a name="the-march-2017-release-of-microsoft-azure-machine-learning-updates-provides-the-following-feature"></a><span data-ttu-id="b647a-104">2017 března verzi aktualizace Microsoft Azure Machine Learning poskytuje následující funkce:</span><span class="sxs-lookup"><span data-stu-id="b647a-104">The March 2017 release of Microsoft Azure Machine Learning updates provides the following feature:</span></span>



* <span data-ttu-id="b647a-105">Vyhrazené kapacity pro Azure Machine Learning BES úlohy</span><span class="sxs-lookup"><span data-stu-id="b647a-105">Dedicated Capacity for Azure Machine Learning BES Jobs</span></span>

    <span data-ttu-id="b647a-106">Počítač fondu Batch Learning zpracování používá [Azure Batch](../batch/batch-technical-overview.md) služba poskytnout spravované zákazníkem škálování pro službu Azure Machine Learning dávky spuštění.</span><span class="sxs-lookup"><span data-stu-id="b647a-106">Machine Learning Batch Pool processing uses the [Azure Batch](../batch/batch-technical-overview.md) service to provide customer-managed scale for the Azure Machine Learning Batch Execution Service.</span></span> <span data-ttu-id="b647a-107">Zpracování fondu batch můžete vytvořit fondy Azure Batch, na kterých můžete odesílat dávkové úlohy a jejich spuštění předvídatelný způsobem.</span><span class="sxs-lookup"><span data-stu-id="b647a-107">Batch Pool processing allows you to create Azure Batch pools on which you can submit batch jobs and have them execute in a predictable manner.</span></span>

    <span data-ttu-id="b647a-108">Další informace najdete v tématu [služby Azure Batch pro Machine Learning úlohy](machine-learning-dedicated-capacity-for-bes-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="b647a-108">For more information, see [Azure Batch service for Machine Learning jobs](machine-learning-dedicated-capacity-for-bes-jobs.md).</span></span>


### <a name="the-august-2016-release-of-microsoft-azure-machine-learning-updates-provide-the-following-features"></a><span data-ttu-id="b647a-109">Srpna 2016 verzi Microsoft Azure Machine Learning aktualizace poskytují následující funkce:</span><span class="sxs-lookup"><span data-stu-id="b647a-109">The August 2016 release of Microsoft Azure Machine Learning updates provide the following features:</span></span>
* <span data-ttu-id="b647a-110">Classic webové služby teď můžete spravovat v novém [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/) portál, který poskytuje jednom místě spravovat všechny aspekty webové služby.</span><span class="sxs-lookup"><span data-stu-id="b647a-110">Classic Web services can now be managed in the new [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/) portal that provides one place to manage all aspects of your Web service.</span></span>    
  * <span data-ttu-id="b647a-111">Které poskytuje webovou službu [Statistika využití](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="b647a-111">Which provides web service [usage statistics](machine-learning-manage-new-webservice.md).</span></span>
  * <span data-ttu-id="b647a-112">Zjednodušuje testování pomocí ukázkových dat volání Azure Machine Learning vzdáleného požadavku.</span><span class="sxs-lookup"><span data-stu-id="b647a-112">Simplifies testing of Azure Machine Learning Remote-Request calls using sample data.</span></span>
  * <span data-ttu-id="b647a-113">Poskytuje nové zkušební stránku služby Batch spuštění ukázkových dat a úlohy odeslání historie.</span><span class="sxs-lookup"><span data-stu-id="b647a-113">Provides a new Batch Execution Service test page with sample data and job submission history.</span></span>
  * <span data-ttu-id="b647a-114">Nabízí jednodušší správu koncový bod.</span><span class="sxs-lookup"><span data-stu-id="b647a-114">Provides easier endpoint management.</span></span>

### <a name="the-july-2016-release-of-microsoft-azure-machine-learning-updates-provide-the-following-features"></a><span data-ttu-id="b647a-115">Července 2016 verzi Microsoft Azure Machine Learning aktualizace poskytují následující funkce:</span><span class="sxs-lookup"><span data-stu-id="b647a-115">The July 2016 release of Microsoft Azure Machine Learning updates provide the following features:</span></span>
* <span data-ttu-id="b647a-116">Webové služby se nyní spravují jako prostředky Azure, které jsou spravované prostřednictvím [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) rozhraní, aby vám umožnil následující vylepšení:</span><span class="sxs-lookup"><span data-stu-id="b647a-116">Web services are now managed as Azure resources managed through [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) interfaces, allowing for the following enhancements:</span></span>
  * <span data-ttu-id="b647a-117">Existují nové [rozhraní REST API](https://msdn.microsoft.com/library/azure/Dn950030.aspx) k nasazení a správě Resource Manager na základě webové služby.</span><span class="sxs-lookup"><span data-stu-id="b647a-117">There are new [REST APIs](https://msdn.microsoft.com/library/azure/Dn950030.aspx) to deploy and manage your Resource Manager based Web services.</span></span>
  * <span data-ttu-id="b647a-118">Nová [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/) portál, který poskytuje jednom místě spravovat všechny aspekty webové služby.</span><span class="sxs-lookup"><span data-stu-id="b647a-118">There is a new [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/) portal that provides one place to manage all aspects of your Web service.</span></span>
* <span data-ttu-id="b647a-119">Zahrnuje nový model nasazení založené na předplatném, oblasti služby webové služby pomocí Správce prostředků na základě rozhraní API využívat poskytovatele prostředků Resource Manageru pro webové služby.</span><span class="sxs-lookup"><span data-stu-id="b647a-119">Incorporates a new subscription-based, multi-region web service deployment model using Resource Manager based APIs leveraging the Resource Manager Resource Provider for Web Services.</span></span>
* <span data-ttu-id="b647a-120">Zavádí nové [cenových plánů](https://azure.microsoft.com/pricing/details/machine-learning/) a naplánovat možnosti správy pomocí nového správce prostředků RP pro fakturaci.</span><span class="sxs-lookup"><span data-stu-id="b647a-120">Introduces new [pricing plans](https://azure.microsoft.com/pricing/details/machine-learning/) and plan management capabilities using the new Resource Manager RP for Billing.</span></span>
  * <span data-ttu-id="b647a-121">Teď můžete [nasazení webové služby do několika oblastí](machine-learning-how-to-deploy-to-multiple-regions.md) bez nutnosti vytvoření odběru v každé oblasti.</span><span class="sxs-lookup"><span data-stu-id="b647a-121">You can now [deploy your web service to multiple regions](machine-learning-how-to-deploy-to-multiple-regions.md) without needing to create a subscription in each region.</span></span>
* <span data-ttu-id="b647a-122">Poskytuje webovou službu [Statistika využití](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="b647a-122">Provides web service [usage statistics](machine-learning-manage-new-webservice.md).</span></span>
* <span data-ttu-id="b647a-123">Zjednodušuje testování pomocí ukázkových dat volání Azure Machine Learning vzdáleného požadavku.</span><span class="sxs-lookup"><span data-stu-id="b647a-123">Simplifies testing of Azure Machine Learning Remote-Request calls using sample data.</span></span>
* <span data-ttu-id="b647a-124">Poskytuje nové zkušební stránku služby Batch spuštění ukázkových dat a úlohy odeslání historie.</span><span class="sxs-lookup"><span data-stu-id="b647a-124">Provides a new Batch Execution Service test page with sample data and job submission history.</span></span>

<span data-ttu-id="b647a-125">Kromě toho Machine Learning Studio je aktualizovaná tak, aby umožňují nasadit do nového modelu webové služby nebo pokračovat v nasazování do klasického modelu webové služby.</span><span class="sxs-lookup"><span data-stu-id="b647a-125">In addition, the Machine Learning Studio has been updated to allow you to deploy to the new Web service model or continue to deploy to the classic Web service model.</span></span> 


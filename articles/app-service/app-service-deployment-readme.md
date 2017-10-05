---
title: "Nasazení aplikací do Azure App Service"
description: "Zjistěte, jak nasadit aplikace pro práci služby App Service"
keywords: "aplikace služby azure app service nasazení, nasazení"
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: 
ms.assetid: de12cd6e-e124-4e48-90bc-c3a3801305da
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 347e8b5177eac8e08ab0dea701b736b86d23904a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-deployment-overview"></a><span data-ttu-id="1603f-104">Přehled nasazení služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1603f-104">Azure App Service Deployment Overview</span></span>
<span data-ttu-id="1603f-105">Aplikační služba Azure poskytuje bohatý a integrované funkce nastavené pro podporu vytváření pracovních postupů výkonný a flexibilní nasazení.</span><span class="sxs-lookup"><span data-stu-id="1603f-105">Azure App Service provides a rich and integrated feature set to support creating powerful and flexible deployment workflows.</span></span> <span data-ttu-id="1603f-106">Nasazení aplikace můžete využít možnosti, které zahrnují průběžnou integraci nebo místního zdrojového kódu publikování, WebDeploy a FTP.</span><span class="sxs-lookup"><span data-stu-id="1603f-106">App deployment can leverage options that include continuous integration or local source control publishing, WebDeploy, and FTP.</span></span> <span data-ttu-id="1603f-107">Doporučenou metodou pro produkční nasazení aplikace je prohození slotů nasazení.</span><span class="sxs-lookup"><span data-stu-id="1603f-107">The recommended method for production app deployment is deployment slot swap.</span></span> <span data-ttu-id="1603f-108">Nasazovací sloty představují pracovní a integrace prostředí přidružené k produkční aplikace.</span><span class="sxs-lookup"><span data-stu-id="1603f-108">Deployment slots represent staging and integration environments associated with production apps.</span></span> <span data-ttu-id="1603f-109">Nasazovací sloty můžete nakonfigurovat a cílem webový provoz pro ověření a můžete si místo na vyžádání pro nasazení do produkčního prostředí bez dolů doby provozu a automatizované warm-up.</span><span class="sxs-lookup"><span data-stu-id="1603f-109">Deployment slots can be configured and targeted with web traffic for validation, and traffic can be swapped on demand for deployment to production with no down time and automated warm-up.</span></span> <span data-ttu-id="1603f-110">Postup nasazení pracovního postupu je možné snadno automatizovat prostřednictvím produkty pro správu verze jako je například Správa verzi Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1603f-110">The steps of a deployment workflow can be easily automated via release management products such as Visual Studio Release Management.</span></span> <span data-ttu-id="1603f-111">To je užitečné pro koordinaci s jiným prostředkům řešení (například úložiště dat), opakování a replikace napříč více jednotky nasazení.</span><span class="sxs-lookup"><span data-stu-id="1603f-111">This is useful for coordination with other solution resources (e.g. data store), recurrence, and replication across multiple units of deployment.</span></span> 

[!INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]


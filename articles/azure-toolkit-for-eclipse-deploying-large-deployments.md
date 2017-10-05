---
title: "Nasazení velkých nasazeních"
description: "Informace o nasazení pomocí sady nástrojů pro Azure pro Eclipse nasazení ve velkých organizacích."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5e18bace-5df0-4af8-ad86-6151ea8bd823
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: e12e379e2b6727653e2377b1760c3745596a1e9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploying-large-deployments"></a><span data-ttu-id="444fd-103">Nasazení velkých nasazeních</span><span class="sxs-lookup"><span data-stu-id="444fd-103">Deploying Large Deployments</span></span>
<span data-ttu-id="444fd-104">Pokud vaše nasazení je příliš velký být obsažené ve složce approot výchozí, můžete použít místní úložiště prostředků jako kořenové složky pro nasazení pro vaše JDK a aplikačního serveru.</span><span class="sxs-lookup"><span data-stu-id="444fd-104">If your deployment is too large to be contained in the default approot folder, you can use a local storage resource as the deployment root folder for your JDK and application server.</span></span>

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a><span data-ttu-id="444fd-105">Chcete použít místní úložiště prostředků jako kořenové složky pro nasazení pro velká nasazení</span><span class="sxs-lookup"><span data-stu-id="444fd-105">To use a local storage resource as the deployment root folder for large deployments</span></span>
1. <span data-ttu-id="444fd-106">Vytvoření nového prostředku Místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="444fd-106">Create a new local storage resource.</span></span> <span data-ttu-id="444fd-107">Název prostředku nezáleží.</span><span class="sxs-lookup"><span data-stu-id="444fd-107">The name of the resource does not matter.</span></span> <span data-ttu-id="444fd-108">Prostředky úložiště jsou definovány na úrovni role.</span><span class="sxs-lookup"><span data-stu-id="444fd-108">Storage resources are defined at the role level.</span></span> <span data-ttu-id="444fd-109">Nejrychlejší způsob, jak získat přístup k dialogu konfigurace místní úložiště, ze kterého můžete vytvořit nový prostředek Místní úložiště, je pomocí následujících kroků: klikněte pravým tlačítkem na roli v **Project Exploreru** zobrazení (Pokud nevidíte roli, rozbalte uzel vaší Azure projektu), klikněte na tlačítko **Azure**a potom klikněte na **místní úložiště**.</span><span class="sxs-lookup"><span data-stu-id="444fd-109">The quickest way to access the local storage configuration dialog, from which you could create a new local storage resource, is by using the following steps: Right-click the role in the **Project Explorer** view (expand your Azure project node if you don't see the role), click **Azure**, and then click **Local Storage**.</span></span> <span data-ttu-id="444fd-110">V rámci **místní úložiště** dialogové okno, klikněte na tlačítko **přidat** k vytvoření nového prostředku Místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="444fd-110">Within the **Local Storage** dialog, click **Add** to create a new local storage resource.</span></span>

2. <span data-ttu-id="444fd-111">Nastavte požadovanou velikost na alespoň 2048 MB (nic méně může mít stejný soubor velikost problémy jako by se mohl dostat approot).</span><span class="sxs-lookup"><span data-stu-id="444fd-111">Set the desired size to at least 2048 MB (anything less may cause the same file size problems as you would encounter in the approot).</span></span>

3. <span data-ttu-id="444fd-112">Ujistěte se, že **vyčistit obsah po recyklaci role instance** je zaškrtnuta možnost; To pomůže zabránit vzniku konfliktu s existující soubory v prostředku po recyklaci role instance logika spuštění nasazení.</span><span class="sxs-lookup"><span data-stu-id="444fd-112">Ensure that **Clean the contents when the role instance is recycled** is checked; this will help prevent the deployment's startup logic from running into conflicts with pre-existing files in the resource when the role instance is recycled.</span></span>

4. <span data-ttu-id="444fd-113">Ujistěte se, že **proměnnou prostředí ukládání cesta k adresáři prostředku po nasazení** hodnota nastavena na řetězec **DEPLOYROOT**.</span><span class="sxs-lookup"><span data-stu-id="444fd-113">Ensure that the **Environment variable storing the resource's directory path after deployment** value is set to the string **DEPLOYROOT**.</span></span> <span data-ttu-id="444fd-114">Dialogové okno vaše místní úložiště prostředků bude vypadat podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="444fd-114">Your local storage resource dialog will look similar to the following.</span></span>

   ![][ic667943]

<span data-ttu-id="444fd-115">Případně pokud používáte **DEPLOYROOT** jako *název* místní prostředek, a můžete neměňte název proměnné prostředí automaticky generované (který bude nastaven na **DEPLOYROOT_PATH** v takovém případě), že bude fungovat pro vaši aplikaci a.</span><span class="sxs-lookup"><span data-stu-id="444fd-115">Alternatively, if you use **DEPLOYROOT** as the *name* of your local resource and you do not change the automatically-generated environment variable name (which will be set to **DEPLOYROOT_PATH** in that case), that would work for your application as well.</span></span>

<span data-ttu-id="444fd-116">Další informace o vytvoření prostředku Místní úložiště lze najít na [místní úložiště vlastnosti][Local storage properties].</span><span class="sxs-lookup"><span data-stu-id="444fd-116">Additional information about creating a local storage resource can be found at [Local storage properties][Local storage properties].</span></span>

## <a name="see-also"></a><span data-ttu-id="444fd-117">Viz také</span><span class="sxs-lookup"><span data-stu-id="444fd-117">See Also</span></span>
<span data-ttu-id="444fd-118">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="444fd-118">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="444fd-119">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="444fd-119">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="444fd-120">[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="444fd-120">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="444fd-121">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="444fd-121">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

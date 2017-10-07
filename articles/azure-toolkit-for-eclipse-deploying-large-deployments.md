---
title: "aaaDeploying velká nasazení"
description: "Zjistěte, jak hello toodeploy velkých nasazeních pomocí nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 6b1d2a7a5e49c78154fc856a221e64ca8dcfbe9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-large-deployments"></a><span data-ttu-id="77a00-103">Nasazení velkých nasazeních</span><span class="sxs-lookup"><span data-stu-id="77a00-103">Deploying Large Deployments</span></span>
<span data-ttu-id="77a00-104">Pokud vaše nasazení je příliš velký toobe obsažené ve složce approot výchozí hello, můžete použít místní úložiště prostředků jako hello nasazení kořenové složky pro váš JDK a aplikačního serveru.</span><span class="sxs-lookup"><span data-stu-id="77a00-104">If your deployment is too large toobe contained in hello default approot folder, you can use a local storage resource as hello deployment root folder for your JDK and application server.</span></span>

## <a name="toouse-a-local-storage-resource-as-hello-deployment-root-folder-for-large-deployments"></a><span data-ttu-id="77a00-105">toouse místní úložiště prostředků jako hello nasazení kořenová složka pro velká nasazení</span><span class="sxs-lookup"><span data-stu-id="77a00-105">toouse a local storage resource as hello deployment root folder for large deployments</span></span>
1. <span data-ttu-id="77a00-106">Vytvoření nového prostředku Místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="77a00-106">Create a new local storage resource.</span></span> <span data-ttu-id="77a00-107">Hello název prostředku hello nezáleží.</span><span class="sxs-lookup"><span data-stu-id="77a00-107">hello name of hello resource does not matter.</span></span> <span data-ttu-id="77a00-108">Prostředky úložiště jsou definované na úrovni role hello.</span><span class="sxs-lookup"><span data-stu-id="77a00-108">Storage resources are defined at hello role level.</span></span> <span data-ttu-id="77a00-109">Hello nejrychlejší způsob, jak tooaccess hello místní úložiště konfigurace dialogové okno, ve kterém můžete vytvořit nový prostředek Místní úložiště, je pomocí následujících kroků hello: klikněte pravým tlačítkem na hello role v hello **Project Exploreru** zobrazení (rozšířit vaše Projektu Azure uzel Pokud nevidíte hello role), klikněte na tlačítko **Azure**a potom klikněte na **místní úložiště**.</span><span class="sxs-lookup"><span data-stu-id="77a00-109">hello quickest way tooaccess hello local storage configuration dialog, from which you could create a new local storage resource, is by using hello following steps: Right-click hello role in hello **Project Explorer** view (expand your Azure project node if you don't see hello role), click **Azure**, and then click **Local Storage**.</span></span> <span data-ttu-id="77a00-110">V rámci hello **místní úložiště** dialogové okno, klikněte na tlačítko **přidat** toocreate nový prostředek Místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="77a00-110">Within hello **Local Storage** dialog, click **Add** toocreate a new local storage resource.</span></span>

2. <span data-ttu-id="77a00-111">Sada hello potřeby tooat velikosti alespoň 2 048 MB paměti (nic méně může mít hello stejné problémy velikost souboru jako by se mohl dostat hello approot).</span><span class="sxs-lookup"><span data-stu-id="77a00-111">Set hello desired size tooat least 2048 MB (anything less may cause hello same file size problems as you would encounter in hello approot).</span></span>

3. <span data-ttu-id="77a00-112">Ujistěte se, že **vyčistit hello obsah po recyklaci hello role instance** je zaškrtnuta možnost; To pomůže zabránit vzniku konfliktu s existující soubory v prostředku hello logika spuštění nasazení hello při hello role instance je recykluje.</span><span class="sxs-lookup"><span data-stu-id="77a00-112">Ensure that **Clean hello contents when hello role instance is recycled** is checked; this will help prevent hello deployment's startup logic from running into conflicts with pre-existing files in hello resource when hello role instance is recycled.</span></span>

4. <span data-ttu-id="77a00-113">Ujistěte se, že hello **ukládání proměnné prostředí hello cesta k adresáři prostředku po nasazení** hodnota řetězce toohello **DEPLOYROOT**.</span><span class="sxs-lookup"><span data-stu-id="77a00-113">Ensure that hello **Environment variable storing hello resource's directory path after deployment** value is set toohello string **DEPLOYROOT**.</span></span> <span data-ttu-id="77a00-114">Dialogové okno vaše místní úložiště prostředků bude vypadat podobně jako následující toohello.</span><span class="sxs-lookup"><span data-stu-id="77a00-114">Your local storage resource dialog will look similar toohello following.</span></span>

   ![][ic667943]

<span data-ttu-id="77a00-115">Případně pokud používáte **DEPLOYROOT** jako hello *název* místní prostředek, a můžete neměnit název proměnné prostředí automaticky generované hello (které se nastaví příliš **DEPLOYROOT_PATH** v takovém případě), že by fungovat i vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="77a00-115">Alternatively, if you use **DEPLOYROOT** as hello *name* of your local resource and you do not change hello automatically-generated environment variable name (which will be set too**DEPLOYROOT_PATH** in that case), that would work for your application as well.</span></span>

<span data-ttu-id="77a00-116">Další informace o vytvoření prostředku Místní úložiště lze najít na [místní úložiště vlastnosti][Local storage properties].</span><span class="sxs-lookup"><span data-stu-id="77a00-116">Additional information about creating a local storage resource can be found at [Local storage properties][Local storage properties].</span></span>

## <a name="see-also"></a><span data-ttu-id="77a00-117">Viz také</span><span class="sxs-lookup"><span data-stu-id="77a00-117">See Also</span></span>
<span data-ttu-id="77a00-118">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="77a00-118">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="77a00-119">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="77a00-119">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="77a00-120">[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="77a00-120">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="77a00-121">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="77a00-121">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Local storage properties]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

---
title: "Zobrazování obsahu Javadoc v prostředí Eclipse pro balíček knihovny Azure pro jazyk Java"
description: "Postupy: zobrazení obsahu Javadoc pro knihovny Azure Libraries v prostředí Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: b44deb773b2159cba1d5d957455409f10fc49334
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a><span data-ttu-id="e0de4-103">Zobrazování obsahu Javadoc v prostředí Eclipse pro balíček knihovny Azure pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="e0de4-103">Displaying Javadoc Content in Eclipse for the Azure Libraries Package for Java</span></span>
<span data-ttu-id="e0de4-104">Obsah Javadoc pro knihovny Azure Libraries for Java lze zobrazit v rámci vašeho prostředí Eclipse tím, že přidružíte Javadoc obsahu do knihovny Azure Libraries for Java.</span><span class="sxs-lookup"><span data-stu-id="e0de4-104">The Javadoc content for the Azure Libraries for Java can be viewed within your Eclipse environment by associating the Javadoc content to the Azure Libraries for Java.</span></span> <span data-ttu-id="e0de4-105">Následující kroky ukazují, jak chcete používat tuto funkci v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="e0de4-105">The following steps show you how to use this functionality within Eclipse.</span></span>

<span data-ttu-id="e0de4-106">Tento postup předpokládá, že jste už přidali knihovny Azure pro jazyk Java na cestu k sestavení.</span><span class="sxs-lookup"><span data-stu-id="e0de4-106">This procedure assumes you have already added the Azure Library for Java to your build path.</span></span>

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a><span data-ttu-id="e0de4-107">Chcete-li zobrazit obsah Javadoc v prostředí Eclipse pro knihovny Azure Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="e0de4-107">To display Javadoc content in Eclipse for the Azure Libraries for Java</span></span>
* <span data-ttu-id="e0de4-108">V prostředí Eclipse v prohlížeči projektu klikněte v **odkazuje knihovny** části projektu, otevřete v místní nabídce knihovny Azure pro Java JAR.</span><span class="sxs-lookup"><span data-stu-id="e0de4-108">Within Eclipse's Project Explorer, in the **Referenced Libraries** section of your project, open the context menu for the Azure Library for Java JAR.</span></span> <span data-ttu-id="e0de4-109">Například **microsoft windowsazure rozhraní api 0.1.0.jar** (číslo verze může být různé, závisí na verzi, které jste nainstalovali).</span><span class="sxs-lookup"><span data-stu-id="e0de4-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (the version number may be different, dependent upon which version you have installed).</span></span>

* <span data-ttu-id="e0de4-110">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e0de4-110">Click **Properties**.</span></span>

* <span data-ttu-id="e0de4-111">V rámci **vlastnosti** dialogové okno, v levém podokně klikněte na tlačítko **Javadoc umístění**.</span><span class="sxs-lookup"><span data-stu-id="e0de4-111">Within the **Properties** dialog, in the left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="e0de4-112">**Javadoc umístění** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e0de4-112">The **Javadoc Location** dialog is displayed.</span></span>

* <span data-ttu-id="e0de4-113">Můžete zadat **Javadoc URL**, nebo **Javadoc v archivu**.</span><span class="sxs-lookup"><span data-stu-id="e0de4-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="e0de4-114">Pokud zvolíte možnost zadat **Javadoc URL**, jako například použít adresy URL **http://dl.windowsazure.com/javadoc** nebo **http://dl.windowsazure.com/storage/javadoc**.</span><span class="sxs-lookup"><span data-stu-id="e0de4-114">If you choose to specify a **Javadoc URL**, use the URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="e0de4-115">Pokud chcete použít **Javadoc v archivu**, můžete zadat externí soubor nebo soubor pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="e0de4-115">If you choose to use **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="e0de4-116">Zkontrolujte své volby a procházet nebo ověřit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="e0de4-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="e0de4-117">Následující příklad přidruží knihovny Azure Libraries for Java odpovídající Javadoc JAR který byl stažen místně do složky s názvem **c:\MyAzureJARs**.</span><span class="sxs-lookup"><span data-stu-id="e0de4-117">The following example associates the Azure Libraries for Java with the corresponding Javadoc JAR that has been downloaded locally to a folder named **c:\MyAzureJARs**.</span></span>

   ![][ic553487]

* <span data-ttu-id="e0de4-118">*Volitelný krok*: klikněte na tlačítko **ověření**.</span><span class="sxs-lookup"><span data-stu-id="e0de4-118">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="e0de4-119">Možné problémy s Javadoc JAR může zobrazí tady.</span><span class="sxs-lookup"><span data-stu-id="e0de4-119">Potential issues with the Javadoc JAR could be displayed here.</span></span>

* <span data-ttu-id="e0de4-120">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0de4-120">Click **OK**.</span></span>

<span data-ttu-id="e0de4-121">Jakmile přidružené ke knihovně, obsah Javadoc má zobrazit v rámci vašeho prostředí Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="e0de4-121">Once associated with the library, the Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="e0de4-122">Například pokud `blob` je definován typ `CloudBlockBlob` v kódu, tady je příklad Javadoc obsahu, který se zobrazí, když zadáte `blob.acquireLease` v kódu:</span><span class="sxs-lookup"><span data-stu-id="e0de4-122">For example, if `blob` is defined of type `CloudBlockBlob` within your code, the following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![][ic553488]

## <a name="see-also"></a><span data-ttu-id="e0de4-123">Viz také</span><span class="sxs-lookup"><span data-stu-id="e0de4-123">See Also</span></span>
<span data-ttu-id="e0de4-124">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e0de4-124">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="e0de4-125">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e0de4-125">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="e0de4-126">[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="e0de4-126">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="e0de4-127">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="e0de4-127">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

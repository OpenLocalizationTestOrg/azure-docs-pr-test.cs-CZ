---
title: "aaaDisplaying Javadoc obsah Eclipse pro hello balíček knihovny Azure pro jazyk Java"
description: "Jak toodisplay hello Javadoc obsah pro knihovny Azure hello v prostředí Eclipse."
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
ms.openlocfilehash: 8070023a24dc07eca8df906db5b8b662ceed6ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="displaying-javadoc-content-in-eclipse-for-hello-azure-libraries-package-for-java"></a><span data-ttu-id="5736e-103">Zobrazení obsahu Javadoc v prostředí Eclipse pro hello balíček knihovny Azure pro jazyk Java</span><span class="sxs-lookup"><span data-stu-id="5736e-103">Displaying Javadoc Content in Eclipse for hello Azure Libraries Package for Java</span></span>
<span data-ttu-id="5736e-104">Hello Javadoc obsah hello knihovny Azure Libraries for Java lze zobrazit v rámci vašeho prostředí Eclipse tím, že přidružíte hello Javadoc obsahu toohello knihovny Azure Libraries for Java.</span><span class="sxs-lookup"><span data-stu-id="5736e-104">hello Javadoc content for hello Azure Libraries for Java can be viewed within your Eclipse environment by associating hello Javadoc content toohello Azure Libraries for Java.</span></span> <span data-ttu-id="5736e-105">Hello následující kroky ukazují, jak toouse tuto funkci v Eclipse.</span><span class="sxs-lookup"><span data-stu-id="5736e-105">hello following steps show you how toouse this functionality within Eclipse.</span></span>

<span data-ttu-id="5736e-106">Tento postup předpokládá, že jste už přidali hello knihovny Azure pro cesta sestavení Java tooyour.</span><span class="sxs-lookup"><span data-stu-id="5736e-106">This procedure assumes you have already added hello Azure Library for Java tooyour build path.</span></span>

## <a name="toodisplay-javadoc-content-in-eclipse-for-hello-azure-libraries-for-java"></a><span data-ttu-id="5736e-107">toodisplay Javadoc obsah Eclipse pro hello knihovny Azure Libraries for Java</span><span class="sxs-lookup"><span data-stu-id="5736e-107">toodisplay Javadoc content in Eclipse for hello Azure Libraries for Java</span></span>
* <span data-ttu-id="5736e-108">V Eclipse na prohlížeči projektu klikněte v hello **odkazuje knihovny** části projektu, otevřete hello kontextovou nabídku hello knihovny Azure pro Java JAR.</span><span class="sxs-lookup"><span data-stu-id="5736e-108">Within Eclipse's Project Explorer, in hello **Referenced Libraries** section of your project, open hello context menu for hello Azure Library for Java JAR.</span></span> <span data-ttu-id="5736e-109">Například **microsoft windowsazure rozhraní api 0.1.0.jar** (číslo verze hello může být různé, závisí na verzi, které jste nainstalovali).</span><span class="sxs-lookup"><span data-stu-id="5736e-109">For example, **microsoft-windowsazure-api-0.1.0.jar** (hello version number may be different, dependent upon which version you have installed).</span></span>

* <span data-ttu-id="5736e-110">Klikněte na **Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="5736e-110">Click **Properties**.</span></span>

* <span data-ttu-id="5736e-111">V rámci hello **vlastnosti** dialogové okno, v levém podokně hello, klikněte na tlačítko **Javadoc umístění**.</span><span class="sxs-lookup"><span data-stu-id="5736e-111">Within hello **Properties** dialog, in hello left-hand pane, click **Javadoc Location**.</span></span> <span data-ttu-id="5736e-112">Hello **Javadoc umístění** se zobrazí dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5736e-112">hello **Javadoc Location** dialog is displayed.</span></span>

* <span data-ttu-id="5736e-113">Můžete zadat **Javadoc URL**, nebo **Javadoc v archivu**.</span><span class="sxs-lookup"><span data-stu-id="5736e-113">You can specify a **Javadoc URL**, or a **Javadoc in archive**.</span></span>

   * <span data-ttu-id="5736e-114">Pokud se rozhodnete toospecify **Javadoc URL**, jako například použít adresy URL hello **http://dl.windowsazure.com/javadoc** nebo **http://dl.windowsazure.com/storage/javadoc**.</span><span class="sxs-lookup"><span data-stu-id="5736e-114">If you choose toospecify a **Javadoc URL**, use hello URLs such as **http://dl.windowsazure.com/javadoc** or **http://dl.windowsazure.com/storage/javadoc**.</span></span>

   * <span data-ttu-id="5736e-115">Pokud se rozhodnete toouse **Javadoc v archivu**, můžete zadat externí soubor nebo soubor pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="5736e-115">If you choose toouse **Javadoc in archive**, you can specify an external file, or a workspace file.</span></span>

   <span data-ttu-id="5736e-116">Zkontrolujte své volby a procházet nebo ověřit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5736e-116">Make your choice and browse/validate as needed.</span></span> <span data-ttu-id="5736e-117">Hello následující příklad přidruží hello knihovny Azure Libraries for Java hello odpovídající Javadoc JAR, který byl stažen místně tooa složku s názvem **c:\MyAzureJARs**.</span><span class="sxs-lookup"><span data-stu-id="5736e-117">hello following example associates hello Azure Libraries for Java with hello corresponding Javadoc JAR that has been downloaded locally tooa folder named **c:\MyAzureJARs**.</span></span>

   ![][ic553487]

* <span data-ttu-id="5736e-118">*Volitelný krok*: klikněte na tlačítko **ověření**.</span><span class="sxs-lookup"><span data-stu-id="5736e-118">*Optional Step*: Click **Validate**.</span></span> <span data-ttu-id="5736e-119">Možné problémy s hello Javadoc JAR může zobrazí tady.</span><span class="sxs-lookup"><span data-stu-id="5736e-119">Potential issues with hello Javadoc JAR could be displayed here.</span></span>

* <span data-ttu-id="5736e-120">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5736e-120">Click **OK**.</span></span>

<span data-ttu-id="5736e-121">Jakmile přidružené hello knihovně, by měla zobrazovat hello Javadoc obsahu v rámci vašeho prostředí Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="5736e-121">Once associated with hello library, hello Javadoc content should display within your Eclipse IDE.</span></span> <span data-ttu-id="5736e-122">Například pokud `blob` je definován typ `CloudBlockBlob` vašeho kódu hello tady je příklad Javadoc obsahu, který se zobrazí, když zadáte `blob.acquireLease` v kódu:</span><span class="sxs-lookup"><span data-stu-id="5736e-122">For example, if `blob` is defined of type `CloudBlockBlob` within your code, hello following is an example of Javadoc content that appears when you type `blob.acquireLease` in code:</span></span>

![][ic553488]

## <a name="see-also"></a><span data-ttu-id="5736e-123">Viz také</span><span class="sxs-lookup"><span data-stu-id="5736e-123">See Also</span></span>
<span data-ttu-id="5736e-124">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="5736e-124">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="5736e-125">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="5736e-125">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="5736e-126">[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="5736e-126">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="5736e-127">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="5736e-127">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

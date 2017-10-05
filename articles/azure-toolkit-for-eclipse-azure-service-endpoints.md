---
title: "Koncové body služby Azure"
description: "Popisuje nastavení koncový bod služby Azure v sadě nástrojů Azure pro Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9c6125ec-7278-461e-b69c-ed56e844f742
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 6059c292c2687f1bf3d9be04c03aaaaf6adde945
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-service-endpoints"></a><span data-ttu-id="bfbb8-103">Koncové body služby Azure</span><span class="sxs-lookup"><span data-stu-id="bfbb8-103">Azure Service Endpoints</span></span>
<span data-ttu-id="bfbb8-104">Koncové body služby Azure určí, že jestli vaše aplikace je nasazená a spravuje globální platformy Azure, Azure provozována společností 21Vianet v Číně, nebo soukromé platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-104">Azure service endpoints determine whether your application is deployed to and managed by the global Azure platform, Azure operated by 21Vianet in China, or a private Azure platform.</span></span> <span data-ttu-id="bfbb8-105">**Koncové body služby** dialogové okno umožňuje určit, které koncové body služby, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-105">The **Service Endpoints** dialog allows you to specify which service endpoints you want to use.</span></span> <span data-ttu-id="bfbb8-106">Otevřete **koncové body služby** dialogové okno, v rámci prostředí Eclipse, klikněte na tlačítko **okno**, klikněte na tlačítko **Předvolby**, rozbalte položku **Azure**a potom klikněte na **koncové body služby**.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-106">To open the **Service Endpoints** dialog, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Service Endpoints**.</span></span> <span data-ttu-id="bfbb8-107">Nastavení **Active nastavit** pole určuje, které koncovým bodům služby Azure se použije pro Azure projekty v aktuálním pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-107">Setting the **Active Set** field determines which Azure service endpoints will be used for the Azure projects in your current workspace.</span></span>

<span data-ttu-id="bfbb8-108">Následující ukazuje **koncové body služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-108">The following shows the **Service Endpoints** dialog.</span></span>

![][ic719493]

## <a name="to-set-the-service-endpoints"></a><span data-ttu-id="bfbb8-109">Chcete-li nastavit koncové body služby</span><span class="sxs-lookup"><span data-stu-id="bfbb8-109">To set the service endpoints</span></span>
<span data-ttu-id="bfbb8-110">V **koncové body služby** dialogové okno, proveďte jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="bfbb8-110">In the **Service Endpoints** dialog, take one of the following actions:</span></span>

* <span data-ttu-id="bfbb8-111">Pokud chcete použít globální platformy Azure z **Active nastavit** rozevíracího seznamu vyberte **windowsazure.com** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-111">If you want to use the global Azure platform, from the **Active Set** dropdown list, select **windowsazure.com** and click **OK**.</span></span>

* <span data-ttu-id="bfbb8-112">Pokud chcete používat Azure provozované v Číně, společností 21Vianet z **Active nastavit** rozevíracího seznamu vyberte **windowsazure.cn (Čína)** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-112">If you want to use Azure operated by 21Vianet in China, from the **Active Set** dropdown list, select **windowsazure.cn (China)** and click **OK**.</span></span>

* <span data-ttu-id="bfbb8-113">Pokud chcete použít privátní platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="bfbb8-113">If you want to use a private Azure platform:</span></span>

  1. <span data-ttu-id="bfbb8-114">Klikněte na **Upravit**.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-114">Click **Edit**.</span></span>

  2. <span data-ttu-id="bfbb8-115">Otevře dialogové okno, vás informuje o který **koncové body služby** zavřou dialogové okno a otevřou se nastaví soubor předvoleb.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-115">A dialog box opens, informing you that the **Service Endpoints** dialog will be closed, and the preference sets file will be opened.</span></span> <span data-ttu-id="bfbb8-116">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-116">Click **OK**.</span></span>

  3. <span data-ttu-id="bfbb8-117">V souboru preferencesets.xml, vytvořte novou `preferenceset` elementu.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-117">In the preferencesets.xml file, create a new `preferenceset` element.</span></span> <span data-ttu-id="bfbb8-118">Pro tento nový element vytvořit `name`, `blob`, `management`, `portalURL` a `publishsettings` atributů a přidejte hodnoty pro ně, které odpovídají do vaší privátní platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-118">For this new element, create `name`, `blob`, `management`, `portalURL` and `publishsettings` attributes, and add values for them that correspond to your private Azure platform.</span></span> <span data-ttu-id="bfbb8-119">Můžete použít hodnoty zadané pro stávající `preferenceset` elementy jako šablony.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-119">You can use the values provided for the existing `preferenceset` elements as templates.</span></span> <span data-ttu-id="bfbb8-120">**Poznámka:**: hodnota použitá `blob` atribut musí obsahovat text "blob" v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-120">**Note**: The value used for the `blob` attribute must contain the text "blob" in the URL.</span></span>

  4. <span data-ttu-id="bfbb8-121">Uložte a zavřete preferencesets.xml.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-121">Save and close preferencesets.xml.</span></span>

  5. <span data-ttu-id="bfbb8-122">Otevřete **koncové body služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-122">Reopen the **Service Endpoints** dialog.</span></span>

  6. <span data-ttu-id="bfbb8-123">Z **Active nastavit** rozevíracím seznamu vyberte aktivní nastavit, že jste vytvořili a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-123">From the **Active Set** dropdown list, select the active set that you created and click **OK**.</span></span>

  7. <span data-ttu-id="bfbb8-124">Po vytvoření vaší privátní platformy Azure `preferenceset` elementu, můžete změnit hodnoty přiřadit klepnutím **upravit** v tlačítko **koncový bod služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-124">Once you've created your private Azure platform `preferenceset` element, you can change the values assigned to it by clicking the **Edit** button in the **Services Endpoint** dialog.</span></span> <span data-ttu-id="bfbb8-125">Můžete také vytvořit několik privátních platformy Azure `preferenceset` prvky, pokud očekáváte.</span><span class="sxs-lookup"><span data-stu-id="bfbb8-125">You can also create multiple private Azure platform `preferenceset` elements, if you desire.</span></span>

## <a name="see-also"></a><span data-ttu-id="bfbb8-126">Viz také</span><span class="sxs-lookup"><span data-stu-id="bfbb8-126">See Also</span></span>
<span data-ttu-id="bfbb8-127">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="bfbb8-127">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="bfbb8-128">[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="bfbb8-128">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="bfbb8-129">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="bfbb8-129">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="bfbb8-130">Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="bfbb8-130">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

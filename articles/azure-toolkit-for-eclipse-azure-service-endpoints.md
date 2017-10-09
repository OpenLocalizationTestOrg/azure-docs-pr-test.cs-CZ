---
title: "aaaAzure koncové body služby"
description: "Popisuje nastavení hello koncový bod služby Azure v hello nástrojů Azure pro prostředí Eclipse."
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
ms.openlocfilehash: 357aa56409a894719077f2c8f302575c8ebb6883
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-endpoints"></a><span data-ttu-id="4fc0c-103">Koncové body služby Azure</span><span class="sxs-lookup"><span data-stu-id="4fc0c-103">Azure Service Endpoints</span></span>
<span data-ttu-id="4fc0c-104">Koncové body služby Azure určí, že zda je aplikace nasazená tooand spravuje hello globální platformy Azure, Azure provozována společností 21Vianet v Číně, nebo soukromé platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-104">Azure service endpoints determine whether your application is deployed tooand managed by hello global Azure platform, Azure operated by 21Vianet in China, or a private Azure platform.</span></span> <span data-ttu-id="4fc0c-105">Hello **koncové body služby** dialogové okno vám umožní toospecify, který chcete toouse koncové body služby.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-105">hello **Service Endpoints** dialog allows you toospecify which service endpoints you want toouse.</span></span> <span data-ttu-id="4fc0c-106">tooopen hello **koncové body služby** dialogové okno, v rámci prostředí Eclipse, klikněte na tlačítko **okno**, klikněte na tlačítko **Předvolby**, rozbalte položku **Azure**a potom klikněte na **Koncové body služby**.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-106">tooopen hello **Service Endpoints** dialog, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Service Endpoints**.</span></span> <span data-ttu-id="4fc0c-107">Nastavení hello **Active nastavit** pole určuje, které služba Azure koncové body se použije pro hello Azure projekty v aktuálním pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-107">Setting hello **Active Set** field determines which Azure service endpoints will be used for hello Azure projects in your current workspace.</span></span>

<span data-ttu-id="4fc0c-108">Následující Hello ukazuje hello **koncové body služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-108">hello following shows hello **Service Endpoints** dialog.</span></span>

![][ic719493]

## <a name="tooset-hello-service-endpoints"></a><span data-ttu-id="4fc0c-109">Koncové body služby tooset hello</span><span class="sxs-lookup"><span data-stu-id="4fc0c-109">tooset hello service endpoints</span></span>
<span data-ttu-id="4fc0c-110">V hello **koncové body služby** dialogové okno, proveďte jednu z hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="4fc0c-110">In hello **Service Endpoints** dialog, take one of hello following actions:</span></span>

* <span data-ttu-id="4fc0c-111">Pokud chcete, aby toouse hello globální platformy Azure, z hello **Active nastavit** rozevíracího seznamu vyberte **windowsazure.com** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-111">If you want toouse hello global Azure platform, from hello **Active Set** dropdown list, select **windowsazure.com** and click **OK**.</span></span>

* <span data-ttu-id="4fc0c-112">Pokud chcete, aby toouse Azure provozované v Číně společností 21Vianet, z hello **Active nastavit** rozevíracího seznamu vyberte **windowsazure.cn (Čína)** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-112">If you want toouse Azure operated by 21Vianet in China, from hello **Active Set** dropdown list, select **windowsazure.cn (China)** and click **OK**.</span></span>

* <span data-ttu-id="4fc0c-113">Pokud chcete, aby toouse privátní platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="4fc0c-113">If you want toouse a private Azure platform:</span></span>

  1. <span data-ttu-id="4fc0c-114">Klikněte na **Upravit**.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-114">Click **Edit**.</span></span>

  2. <span data-ttu-id="4fc0c-115">Otevře dialogové okno oznamující, že hello **koncové body služby** dialogové okno se zavře a hello předvoleb nastaví soubor bude otevřen.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-115">A dialog box opens, informing you that hello **Service Endpoints** dialog will be closed, and hello preference sets file will be opened.</span></span> <span data-ttu-id="4fc0c-116">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-116">Click **OK**.</span></span>

  3. <span data-ttu-id="4fc0c-117">V souboru preferencesets.xml hello, vytvořte novou `preferenceset` elementu.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-117">In hello preferencesets.xml file, create a new `preferenceset` element.</span></span> <span data-ttu-id="4fc0c-118">Pro tento nový element vytvořit `name`, `blob`, `management`, `portalURL` a `publishsettings` atributy a přidejte hodnoty pro ně, které odpovídají tooyour privátní platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-118">For this new element, create `name`, `blob`, `management`, `portalURL` and `publishsettings` attributes, and add values for them that correspond tooyour private Azure platform.</span></span> <span data-ttu-id="4fc0c-119">Můžete použít hello hodnoty zadané pro existující hello `preferenceset` elementy jako šablony.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-119">You can use hello values provided for hello existing `preferenceset` elements as templates.</span></span> <span data-ttu-id="4fc0c-120">**Poznámka:**: hello hodnota používaná pro hello `blob` atribut musí obsahovat text hello "blob" v adrese URL hello.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-120">**Note**: hello value used for hello `blob` attribute must contain hello text "blob" in hello URL.</span></span>

  4. <span data-ttu-id="4fc0c-121">Uložte a zavřete preferencesets.xml.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-121">Save and close preferencesets.xml.</span></span>

  5. <span data-ttu-id="4fc0c-122">Znovu otevřete hello **koncové body služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-122">Reopen hello **Service Endpoints** dialog.</span></span>

  6. <span data-ttu-id="4fc0c-123">Z hello **Active nastavit** rozevíracího seznamu vyberte hello aktivní nastavit, že jste vytvořili a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-123">From hello **Active Set** dropdown list, select hello active set that you created and click **OK**.</span></span>

  7. <span data-ttu-id="4fc0c-124">Po vytvoření vaší privátní platformy Azure `preferenceset` elementu, můžete změnit hello přiřazených hodnot tooit kliknutím hello **upravit** tlačítka na hello **koncový bod služby** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-124">Once you've created your private Azure platform `preferenceset` element, you can change hello values assigned tooit by clicking hello **Edit** button in hello **Services Endpoint** dialog.</span></span> <span data-ttu-id="4fc0c-125">Můžete také vytvořit několik privátních platformy Azure `preferenceset` prvky, pokud očekáváte.</span><span class="sxs-lookup"><span data-stu-id="4fc0c-125">You can also create multiple private Azure platform `preferenceset` elements, if you desire.</span></span>

## <a name="see-also"></a><span data-ttu-id="4fc0c-126">Viz také</span><span class="sxs-lookup"><span data-stu-id="4fc0c-126">See Also</span></span>
<span data-ttu-id="4fc0c-127">[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="4fc0c-127">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="4fc0c-128">[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="4fc0c-128">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="4fc0c-129">[Vytvoření aplikace Hello World služby Azure v prostředí Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="4fc0c-129">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="4fc0c-130">Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="4fc0c-130">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

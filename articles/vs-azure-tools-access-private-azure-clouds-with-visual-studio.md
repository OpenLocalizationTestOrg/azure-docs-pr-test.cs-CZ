---
title: "Přístup k privátní cloud Azure pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak získat přístup k prostředkům privátního cloudu pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: b2578c837732ab05d538e9b896ed3a3035075a70
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a><span data-ttu-id="a8e60-103">Přístup k privátní cloud Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8e60-103">Accessing private Azure clouds with Visual Studio</span></span>
<span data-ttu-id="a8e60-104">Ve výchozím nastavení Visual Studio podporuje koncové body REST veřejného cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="a8e60-104">By default, Visual Studio supports public Azure cloud REST endpoints.</span></span> <span data-ttu-id="a8e60-105">V tomto tématu se naučíte používat privátní cloud certifikát pro přístup k – a interakci s - privátního cloudu ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8e60-105">In this topic, you learn how to use your private cloud's certificate to access - and interact with - the private cloud from Visual Studio.</span></span>

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a><span data-ttu-id="a8e60-106">Pro přístup k privátním Azure cloud v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8e60-106">To access a private Azure cloud in Visual Studio</span></span>
1. <span data-ttu-id="a8e60-107">V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885) pro daný privátní cloud, stáhněte soubor nastavení publikování, nebo se obraťte na svého správce pro soubor nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="a8e60-107">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) for the private cloud, download your publish-settings file, or contact your administrator for a publish-settings file.</span></span> <span data-ttu-id="a8e60-108">Ve veřejné verzi Azure, je odkaz na stažení to [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span><span class="sxs-lookup"><span data-stu-id="a8e60-108">On the public version of Azure, the link to download this is [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span></span> <span data-ttu-id="a8e60-109">(Stažený soubor by měl mít příponu `.publishsettings`)</span><span class="sxs-lookup"><span data-stu-id="a8e60-109">(The downloaded file should have an extension of `.publishsettings`)</span></span>

1. <span data-ttu-id="a8e60-110">Otevřete Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8e60-110">Open Visual Studio</span></span>

1. <span data-ttu-id="a8e60-111">V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **Azure** uzel a v místní nabídce vyberte **spravovat a odběry filtru**.</span><span class="sxs-lookup"><span data-stu-id="a8e60-111">In **Server Explorer**, right-click the **Azure** node and, from the context menu, select **Manage and Filter Subscriptions**.</span></span>
   
    ![Spravovat odběry příkaz](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. <span data-ttu-id="a8e60-113">V **spravovat předplatná Microsoft Azure** dialogovém okně, vyberte **certifikáty** a pak vyberte **Import**.</span><span class="sxs-lookup"><span data-stu-id="a8e60-113">In the **Manage Microsoft Azure Subscriptions** dialog, select the **Certificates** tab, and then select **Import**.</span></span>
   
    ![Import certifikátů Azure](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. <span data-ttu-id="a8e60-115">V **předplatná Microsoft Azure Import** dialogovém okně, vyberte **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="a8e60-115">In the **Import Microsoft Azure Subscriptions** dialog, select **Browse**.</span></span>

    ![V dialogovém okně předplatná Microsoft Azure Import tlačítko Procházet](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. <span data-ttu-id="a8e60-117">V **otevřete** dialogové okno, přejděte do adresáře, kam jste uložili soubor nastavení publikování, vyberte soubor a pak vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="a8e60-117">In the **Open** dialog, browse to the directory where you saved the publish-settings file, select the file, and then select **Open**.</span></span>

    ![Vyberte soubor nastavení publikování](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. <span data-ttu-id="a8e60-119">Když se vrátíte na **předplatná Microsoft Azure Import** dialogovém okně, vyberte **Import**.</span><span class="sxs-lookup"><span data-stu-id="a8e60-119">When returned to the **Import Microsoft Azure Subscriptions** dialog, select **Import**.</span></span>

    ![Importujte soubor nastavení publikování](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    <span data-ttu-id="a8e60-121">Certifikáty jsou importovány ze souboru nastavení publikování do sady Visual Studio a nyní můžete pracovat s vaší privátní cloudové prostředky.</span><span class="sxs-lookup"><span data-stu-id="a8e60-121">The certificates are imported from the publish-settings file into Visual Studio, and you can now interact with your private cloud resources.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="a8e60-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a8e60-122">Next steps</span></span>
- [<span data-ttu-id="a8e60-123">Publikování do cloudové služby Azure ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a8e60-123">Publishing to an Azure Cloud Service from Visual Studio</span></span>](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- <span data-ttu-id="a8e60-124">[Postupy: stahování a Import publikovat nastavení a informace o předplatném](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span><span class="sxs-lookup"><span data-stu-id="a8e60-124">[How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span></span>

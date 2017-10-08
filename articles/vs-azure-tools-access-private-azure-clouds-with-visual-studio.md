---
title: "aaaAccessing privátních cloudů Azure pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak tooaccess soukromé cloudové prostředky pomocí sady Visual Studio."
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
ms.openlocfilehash: 5cfd6439afdcf98c6f7d7f29ab6c4256ed02533a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a><span data-ttu-id="db148-103">Přístup k privátní cloud Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="db148-103">Accessing private Azure clouds with Visual Studio</span></span>
<span data-ttu-id="db148-104">Ve výchozím nastavení Visual Studio podporuje koncové body REST veřejného cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="db148-104">By default, Visual Studio supports public Azure cloud REST endpoints.</span></span> <span data-ttu-id="db148-105">V tomto tématu se dozvíte, jak toouse vaší privátní cloud je certifikát tooaccess - a interakci s – hello privátního cloudu ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="db148-105">In this topic, you learn how toouse your private cloud's certificate tooaccess - and interact with - hello private cloud from Visual Studio.</span></span>

## <a name="tooaccess-a-private-azure-cloud-in-visual-studio"></a><span data-ttu-id="db148-106">tooaccess privátní Azure cloud v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="db148-106">tooaccess a private Azure cloud in Visual Studio</span></span>
1. <span data-ttu-id="db148-107">V hello [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885) pro privátní cloud hello, stáhněte soubor nastavení publikování, nebo se obraťte na svého správce pro soubor nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="db148-107">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885) for hello private cloud, download your publish-settings file, or contact your administrator for a publish-settings file.</span></span> <span data-ttu-id="db148-108">Ve veřejné verzi hello Azure, hello odkaz toodownload jde [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span><span class="sxs-lookup"><span data-stu-id="db148-108">On hello public version of Azure, hello link toodownload this is [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/).</span></span> <span data-ttu-id="db148-109">(hello stažený soubor by měl mít příponu `.publishsettings`)</span><span class="sxs-lookup"><span data-stu-id="db148-109">(hello downloaded file should have an extension of `.publishsettings`)</span></span>

1. <span data-ttu-id="db148-110">Otevřete Visual Studio</span><span class="sxs-lookup"><span data-stu-id="db148-110">Open Visual Studio</span></span>

1. <span data-ttu-id="db148-111">V **Průzkumníka serveru**, klikněte pravým tlačítkem na hello **Azure** uzlu a hello místní nabídce vyberte **spravovat a odběry filtru**.</span><span class="sxs-lookup"><span data-stu-id="db148-111">In **Server Explorer**, right-click hello **Azure** node and, from hello context menu, select **Manage and Filter Subscriptions**.</span></span>
   
    ![Spravovat odběry příkaz](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. <span data-ttu-id="db148-113">V hello **spravovat předplatná Microsoft Azure** dialogové okno, vyberte hello **certifikáty** a pak vyberte **Import**.</span><span class="sxs-lookup"><span data-stu-id="db148-113">In hello **Manage Microsoft Azure Subscriptions** dialog, select hello **Certificates** tab, and then select **Import**.</span></span>
   
    ![Import certifikátů Azure](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. <span data-ttu-id="db148-115">V hello **předplatná Microsoft Azure Import** dialogovém okně, vyberte **Procházet**.</span><span class="sxs-lookup"><span data-stu-id="db148-115">In hello **Import Microsoft Azure Subscriptions** dialog, select **Browse**.</span></span>

    ![V dialogovém okně předplatná Microsoft Azure Import hello tlačítko Procházet](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. <span data-ttu-id="db148-117">V hello **otevřete** dialogové okno, toohello Procházet adresář, kde je uložený hello – soubor s nastavením publikování, vyberte hello souboru a pak vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="db148-117">In hello **Open** dialog, browse toohello directory where you saved hello publish-settings file, select hello file, and then select **Open**.</span></span>

    ![Vyberte soubor s nastavením publikování hello](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. <span data-ttu-id="db148-119">Když vrátil toohello **předplatná Microsoft Azure Import** dialogovém okně, vyberte **Import**.</span><span class="sxs-lookup"><span data-stu-id="db148-119">When returned toohello **Import Microsoft Azure Subscriptions** dialog, select **Import**.</span></span>

    ![Importovat soubor s nastavením publikování hello](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    <span data-ttu-id="db148-121">Hello certifikáty jsou importovány ze souboru nastavení publikování hello do sady Visual Studio a nyní můžete pracovat s vaší privátní cloudové prostředky.</span><span class="sxs-lookup"><span data-stu-id="db148-121">hello certificates are imported from hello publish-settings file into Visual Studio, and you can now interact with your private cloud resources.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="db148-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db148-122">Next steps</span></span>
- [<span data-ttu-id="db148-123">Publikování tooan Cloudová služba Azure ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="db148-123">Publishing tooan Azure Cloud Service from Visual Studio</span></span>](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- <span data-ttu-id="db148-124">[Postupy: stahování a Import publikovat nastavení a informace o předplatném](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span><span class="sxs-lookup"><span data-stu-id="db148-124">[How to: Download and Import Publish Settings and Subscription Information](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)</span></span>

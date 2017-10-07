---
title: "aaaConfigure projektu Azure cloud service pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak tooconfigure Azure cloudové služby projektu v sadě Visual Studio, v závislosti na požadavcích pro tento projekt."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: 40eb5eedd6ea23bf03c8707431799be28beae701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a><span data-ttu-id="02fbd-103">Konfigurace projektu Azure cloud service pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="02fbd-103">Configure an Azure cloud service project with Visual Studio</span></span>
<span data-ttu-id="02fbd-104">Projekt Azure cloud service můžete nakonfigurovat v závislosti na požadavcích pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="02fbd-104">You can configure an Azure cloud service project, depending on your requirements for that project.</span></span> <span data-ttu-id="02fbd-105">Můžete nastavit vlastnosti pro hello projekt pro hello následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="02fbd-105">You can set properties for hello project for hello following categories:</span></span>

- <span data-ttu-id="02fbd-106">**Publikování cloudové služby tooAzure** – můžete nastavit vlastnost toomake jistotu, že není existující služby nasadit tooAzure cloudu odstraněn omylem.</span><span class="sxs-lookup"><span data-stu-id="02fbd-106">**Publish a cloud service tooAzure** - You can set a property toomake sure that an existing cloud service deployed tooAzure is not accidentally deleted.</span></span>
- <span data-ttu-id="02fbd-107">**Spustit nebo ladění cloudové služby na místním počítači hello** – můžete vybrat toouse konfigurace služby a určení toho, zda emulátor úložiště Azure toostart hello.</span><span class="sxs-lookup"><span data-stu-id="02fbd-107">**Run or debug a cloud service on hello local computer** - You can select a service configuration toouse and indicate whether you want toostart hello Azure storage emulator.</span></span>
- <span data-ttu-id="02fbd-108">**Ověřit balíček cloudové služby, když je vytvořeno** -rozhodnete tootreat žádné upozornění jako chyby tak, aby můžete zajistit, že balíček hello cloudové služby nasadí bez problémů.</span><span class="sxs-lookup"><span data-stu-id="02fbd-108">**Validate a cloud service package when it is created** - You can decide tootreat any warnings as errors so that you can ensure that hello cloud service package deploys without any issues.</span></span> 

## <a name="steps-tooconfigure-an-azure-cloud-service-project"></a><span data-ttu-id="02fbd-109">Kroky tooconfigure projektu Azure cloud service</span><span class="sxs-lookup"><span data-stu-id="02fbd-109">Steps tooconfigure an Azure cloud service project</span></span>
1. <span data-ttu-id="02fbd-110">Otevřít nebo vytvořit projekt cloudové služby v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="02fbd-110">Open or create a cloud service project in Visual Studio</span></span>

1. <span data-ttu-id="02fbd-111">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a hello místní nabídce vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="02fbd-111">In **Solution Explorer**, right-click hello project, and, from hello context menu, select **Properties**.</span></span>
   
1. <span data-ttu-id="02fbd-112">Na stránce vlastností projektu hello vyberte hello **vývoj** kartě.</span><span class="sxs-lookup"><span data-stu-id="02fbd-112">In hello project's properties page, select hello **Development** tab.</span></span>

    ![Nabídka Vlastnosti projektu](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. <span data-ttu-id="02fbd-114">Nastavit **výzva před odstraněním stávajícího nasazení** příliš**True**.</span><span class="sxs-lookup"><span data-stu-id="02fbd-114">Set **Prompt before deleting an existing deployment** too**True**.</span></span> <span data-ttu-id="02fbd-115">Toto nastavení pomáhá tooensure nejsou omylem odstranit stávajícího nasazení v Azure</span><span class="sxs-lookup"><span data-stu-id="02fbd-115">This setting helps tooensure you don't accidentally delete an existing deployment in Azure</span></span>

1. <span data-ttu-id="02fbd-116">Vyberte hello potřeby **konfigurace služby** tooindicate konfiguraci služby, kterou chcete toouse při spouštění nebo ladění cloudové služby místně.</span><span class="sxs-lookup"><span data-stu-id="02fbd-116">Select hello desired **Service configuration** tooindicate which service configuration you want toouse when you run or debug your cloud service locally.</span></span> <span data-ttu-id="02fbd-117">Další informace o tom, v tématu Konfigurace služby pro roli, toomodify [jak tooconfigure hello role Azure cloud service pomocí sady Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span><span class="sxs-lookup"><span data-stu-id="02fbd-117">For more information on how toomodify a service configuration for a role, see [How tooconfigure hello roles for an Azure cloud service with Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).</span></span>

1. <span data-ttu-id="02fbd-118">Nastavit **emulátoru úložiště Azure spustit** příliš**True** toostart hello emulátoru úložiště Azure při spouštění nebo ladění cloudové služby místně.</span><span class="sxs-lookup"><span data-stu-id="02fbd-118">Set **Start Azure storage emulator** too**True** toostart hello Azure storage emulator when you run or debug your cloud service locally.</span></span>

1. <span data-ttu-id="02fbd-119">Nastavit **považovat upozornění jako chyby** příliš**True** toomake se, pokud nejsou chyby ověření balíček nelze publikovat.</span><span class="sxs-lookup"><span data-stu-id="02fbd-119">Set **Treat warnings as errors** too**True** toomake sure you cannot publish if there are package validation errors.</span></span>

1. <span data-ttu-id="02fbd-120">Nastavit **použít webový projekt porty** příliš**True** toomake jistotu, že vaši webovou roli používá stejný port se každý hello spuštění ho místně v IIS Express.</span><span class="sxs-lookup"><span data-stu-id="02fbd-120">Set **Use web project ports** too**True** toomake sure that your web role uses hello same port each time it starts locally in IIS Express.</span></span>

1. <span data-ttu-id="02fbd-121">Hello nástrojů Visual Studio, vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="02fbd-121">From hello Visual Studio toolbar, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02fbd-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02fbd-122">Next steps</span></span>
- [<span data-ttu-id="02fbd-123">Konfigurace projektu Azure pomocí více konfigurace služby</span><span class="sxs-lookup"><span data-stu-id="02fbd-123">Configure an Azure project using multiple service configurations</span></span>](vs-azure-tools-multiple-services-project-configurations.md)


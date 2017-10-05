---
title: "Správa rolí v cloudových služeb Azure pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak přidávat a odebírat role ve službě Azure cloud services pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 6ed857b857cf8c14506ca39725c214a7fea4fc95
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a><span data-ttu-id="8e0dc-103">Správa rolí v cloudových služeb Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e0dc-103">Managing roles in Azure cloud services with Visual Studio</span></span>
<span data-ttu-id="8e0dc-104">Po vytvoření cloudové služby Azure, můžete do ní přidejte nové role nebo z něj odebrat existující role.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-104">After you have created your Azure cloud service, you can add new roles to it or remove existing roles from it.</span></span> <span data-ttu-id="8e0dc-105">Můžete také importovat existující projekt a ho převést na roli.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-105">You can also import an existing project and convert it to a role.</span></span> <span data-ttu-id="8e0dc-106">Například můžete importovat webové aplikace ASP.NET a označit ji jako webové role.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-106">For example, you can import an ASP.NET web application and designate it as a web role.</span></span>

## <a name="adding-a-role-to-an-azure-cloud-service"></a><span data-ttu-id="8e0dc-107">Přidání role do cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="8e0dc-107">Adding a role to an Azure cloud service</span></span>
<span data-ttu-id="8e0dc-108">Následující postup vás provede přidáním role web nebo worker k projektu Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-108">The following steps guide you through adding a web or worker role to an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="8e0dc-109">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-109">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="8e0dc-110">V **Průzkumníku**, rozbalte uzel projektu</span><span class="sxs-lookup"><span data-stu-id="8e0dc-110">In **Solution Explorer**, expand the project node</span></span>

1. <span data-ttu-id="8e0dc-111">Klikněte pravým tlačítkem myši **role** uzel k zobrazení v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-111">Right-click the **Roles** node to display the context menu.</span></span> <span data-ttu-id="8e0dc-112">V místní nabídce vyberte **přidat**, pak vyberte existující webovou roli nebo role pracovního procesu z aktuální řešení, nebo vytvořte projekt role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-112">From the context menu, select **Add**, then select an existing web role or worker role from the current solution, or create a web or worker role project.</span></span> <span data-ttu-id="8e0dc-113">Můžete také vybrat příslušný projekt, jako je například projekt webové aplikace ASP.NET a přidružit projekt role.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-113">You can also select an appropriate project, such as an ASP.NET web application project, and associate it with a role project.</span></span>

    ![Možnosti nabídky k přidání role k projektu Azure cloud service](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a><span data-ttu-id="8e0dc-115">Odebrání role z cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="8e0dc-115">Removing a role from an Azure cloud service</span></span>
<span data-ttu-id="8e0dc-116">Následující kroky vás provedou odebrání role web nebo worker z projektu Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-116">The following steps guide you through removing a web or worker role from an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="8e0dc-117">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-117">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="8e0dc-118">V **Průzkumníku**, rozbalte uzel projektu</span><span class="sxs-lookup"><span data-stu-id="8e0dc-118">In **Solution Explorer**, expand the project node</span></span>

1. <span data-ttu-id="8e0dc-119">Rozbalte **role** uzlu.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-119">Expand the **Roles** node.</span></span>

1. <span data-ttu-id="8e0dc-120">Klikněte pravým tlačítkem na uzel, kterou chcete odebrat a v místní nabídce vyberte **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-120">Right-click the node you want to remove, and, from the context menu, select **Remove**.</span></span> 

    ![Možnosti nabídky přidat roli do cloudové služby Azure](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-to-an-azure-cloud-service-project"></a><span data-ttu-id="8e0dc-122">Nové přidání role k projektu Azure cloud service</span><span class="sxs-lookup"><span data-stu-id="8e0dc-122">Readding a role to an Azure cloud service project</span></span>
<span data-ttu-id="8e0dc-123">Pokud odeberete roli z projekt cloudové služby, ale později se rozhodnete přidat roli zpět do projektu, se přidají jenom deklarace role a základní atributy, jako například koncových bodů a diagnostické informace.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-123">If you remove a role from your cloud service project but later decide to add the role back to the project, only the role declaration and basic attributes, such as endpoints and diagnostics information, are added.</span></span> <span data-ttu-id="8e0dc-124">Žádné další prostředky ani odkazy jsou přidány do `ServiceDefinition.csdef` souboru nebo `ServiceConfiguration.cscfg` souboru.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-124">No additional resources or references are added to the `ServiceDefinition.csdef` file or to the `ServiceConfiguration.cscfg` file.</span></span> <span data-ttu-id="8e0dc-125">Pokud chcete přidat tyto informace, budete muset ručně přidat zpět do těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-125">If you want to add this information, you need to manually add it back into these files.</span></span>

<span data-ttu-id="8e0dc-126">Například je možné odebrat role webové služby a později se rozhodnete přidat tato role zpátky do vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-126">For example, you might remove a web service role and later you decide to add this role back into your solution.</span></span> <span data-ttu-id="8e0dc-127">Pokud to uděláte, dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-127">If you do this, an error occurs.</span></span> <span data-ttu-id="8e0dc-128">Aby se tato chyba, je nutné přidat `<LocalResources>` uvedeného v následující soubor XML zpět do prvku `ServiceDefinition.csdef` souboru.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-128">To prevent this error, you have to add the `<LocalResources>` element shown in the following XML back into the `ServiceDefinition.csdef` file.</span></span> <span data-ttu-id="8e0dc-129">Použijte název role webové služby, které jste přidali zpět do projektu jako součást atributu název pro  **<LocalStorage>**  element.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-129">Use the name of the web service role that you added back into the project as part of the name attribute for the **<LocalStorage>** element.</span></span> <span data-ttu-id="8e0dc-130">V tomto příkladu je název role webové služby **WCFServiceWebRole1**.</span><span class="sxs-lookup"><span data-stu-id="8e0dc-130">In this example, the name of the web service role is **WCFServiceWebRole1**.</span></span>

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a><span data-ttu-id="8e0dc-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e0dc-131">Next steps</span></span>
- [<span data-ttu-id="8e0dc-132">Konfigurace role pro cloudové služby Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8e0dc-132">Configure the Roles for an Azure cloud service with Visual Studio</span></span>](vs-azure-tools-configure-roles-for-cloud-service.md)

---
title: "aaaManaging role v Azure cloud services pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak tooadd a odebrat role v Azure cloud services pomocí sady Visual Studio."
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
ms.openlocfilehash: 131edc534d1110ba3d25cd00a3a24b643576875c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a><span data-ttu-id="02986-103">Správa rolí v cloudových služeb Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="02986-103">Managing roles in Azure cloud services with Visual Studio</span></span>
<span data-ttu-id="02986-104">Po vytvoření cloudové služby Azure, můžete přidat nové role tooit nebo z něj odebrat existující role.</span><span class="sxs-lookup"><span data-stu-id="02986-104">After you have created your Azure cloud service, you can add new roles tooit or remove existing roles from it.</span></span> <span data-ttu-id="02986-105">Můžete také importovat existující projekt a převeďte ho tooa role.</span><span class="sxs-lookup"><span data-stu-id="02986-105">You can also import an existing project and convert it tooa role.</span></span> <span data-ttu-id="02986-106">Například můžete importovat webové aplikace ASP.NET a označit ji jako webové role.</span><span class="sxs-lookup"><span data-stu-id="02986-106">For example, you can import an ASP.NET web application and designate it as a web role.</span></span>

## <a name="adding-a-role-tooan-azure-cloud-service"></a><span data-ttu-id="02986-107">Přidání role tooan cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="02986-107">Adding a role tooan Azure cloud service</span></span>
<span data-ttu-id="02986-108">Hello následující kroky vás provede přidáním web nebo worker role tooan Azure projekt cloudové služby v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="02986-108">hello following steps guide you through adding a web or worker role tooan Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="02986-109">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="02986-109">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="02986-110">V **Průzkumníku**, rozbalte uzel projektu hello</span><span class="sxs-lookup"><span data-stu-id="02986-110">In **Solution Explorer**, expand hello project node</span></span>

1. <span data-ttu-id="02986-111">Klikněte pravým tlačítkem na hello **role** uzlu toodisplay hello Kontextová nabídka.</span><span class="sxs-lookup"><span data-stu-id="02986-111">Right-click hello **Roles** node toodisplay hello context menu.</span></span> <span data-ttu-id="02986-112">Hello místní nabídce vyberte **přidat**, pak vyberte existující webovou roli nebo role pracovního procesu z aktuální řešení hello nebo vytvořte projekt role web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="02986-112">From hello context menu, select **Add**, then select an existing web role or worker role from hello current solution, or create a web or worker role project.</span></span> <span data-ttu-id="02986-113">Můžete také vybrat příslušný projekt, jako je například projekt webové aplikace ASP.NET a přidružit projekt role.</span><span class="sxs-lookup"><span data-stu-id="02986-113">You can also select an appropriate project, such as an ASP.NET web application project, and associate it with a role project.</span></span>

    ![Nabídka Možnosti tooadd projekt role tooan Azure cloudové služby](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a><span data-ttu-id="02986-115">Odebrání role z cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="02986-115">Removing a role from an Azure cloud service</span></span>
<span data-ttu-id="02986-116">Hello následující kroky vás provedou odebrání role web nebo worker z projektu Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="02986-116">hello following steps guide you through removing a web or worker role from an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="02986-117">Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="02986-117">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="02986-118">V **Průzkumníku**, rozbalte uzel projektu hello</span><span class="sxs-lookup"><span data-stu-id="02986-118">In **Solution Explorer**, expand hello project node</span></span>

1. <span data-ttu-id="02986-119">Rozbalte hello **role** uzlu.</span><span class="sxs-lookup"><span data-stu-id="02986-119">Expand hello **Roles** node.</span></span>

1. <span data-ttu-id="02986-120">Klikněte pravým tlačítkem na hello uzlu chcete tooremove a, hello místní nabídce vyberte **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="02986-120">Right-click hello node you want tooremove, and, from hello context menu, select **Remove**.</span></span> 

    ![Nabídka Možnosti tooadd tooan role cloudové služby Azure](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-tooan-azure-cloud-service-project"></a><span data-ttu-id="02986-122">Nové přidání projekt role tooan Azure cloudové služby</span><span class="sxs-lookup"><span data-stu-id="02986-122">Readding a role tooan Azure cloud service project</span></span>
<span data-ttu-id="02986-123">Pokud odeberete roli z projekt cloudové služby, ale později se rozhodnete tooadd hello zpátky roli toohello projektu, jenom deklarace role hello a základní atributy, jako například koncových bodů a diagnostické informace, jsou přidány.</span><span class="sxs-lookup"><span data-stu-id="02986-123">If you remove a role from your cloud service project but later decide tooadd hello role back toohello project, only hello role declaration and basic attributes, such as endpoints and diagnostics information, are added.</span></span> <span data-ttu-id="02986-124">Žádné další prostředky ani odkazy jsou přidány toohello `ServiceDefinition.csdef` soubor nebo toohello `ServiceConfiguration.cscfg` souboru.</span><span class="sxs-lookup"><span data-stu-id="02986-124">No additional resources or references are added toohello `ServiceDefinition.csdef` file or toohello `ServiceConfiguration.cscfg` file.</span></span> <span data-ttu-id="02986-125">Pokud chcete tyto informace tooadd, je nutné toomanually přidejte ho zpátky do těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="02986-125">If you want tooadd this information, you need toomanually add it back into these files.</span></span>

<span data-ttu-id="02986-126">Například je možné odebrat role webové služby a později rozhodnete tooadd této role zpátky do vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="02986-126">For example, you might remove a web service role and later you decide tooadd this role back into your solution.</span></span> <span data-ttu-id="02986-127">Pokud to uděláte, dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="02986-127">If you do this, an error occurs.</span></span> <span data-ttu-id="02986-128">tooprevent tato chyba, máte tooadd hello `<LocalResources>` element uvedené v následující hello XML zpět do hello `ServiceDefinition.csdef` souboru.</span><span class="sxs-lookup"><span data-stu-id="02986-128">tooprevent this error, you have tooadd hello `<LocalResources>` element shown in hello following XML back into hello `ServiceDefinition.csdef` file.</span></span> <span data-ttu-id="02986-129">Použijte název hello hello role webové služby, které jste přidali zpět do projektu hello jako součást atributu hello název pro hello  **<LocalStorage>**  element.</span><span class="sxs-lookup"><span data-stu-id="02986-129">Use hello name of hello web service role that you added back into hello project as part of hello name attribute for hello **<LocalStorage>** element.</span></span> <span data-ttu-id="02986-130">V tomto příkladu je název hello role webové služby hello **WCFServiceWebRole1**.</span><span class="sxs-lookup"><span data-stu-id="02986-130">In this example, hello name of hello web service role is **WCFServiceWebRole1**.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="02986-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02986-131">Next steps</span></span>
- [<span data-ttu-id="02986-132">Konfigurovat hello role pro cloudové služby Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="02986-132">Configure hello Roles for an Azure cloud service with Visual Studio</span></span>](vs-azure-tools-configure-roles-for-cloud-service.md)

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
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a>Správa rolí v cloudových služeb Azure pomocí sady Visual Studio
Po vytvoření cloudové služby Azure, můžete přidat nové role tooit nebo z něj odebrat existující role. Můžete také importovat existující projekt a převeďte ho tooa role. Například můžete importovat webové aplikace ASP.NET a označit ji jako webové role.

## <a name="adding-a-role-tooan-azure-cloud-service"></a>Přidání role tooan cloudové služby Azure
Hello následující kroky vás provede přidáním web nebo worker role tooan Azure projekt cloudové služby v sadě Visual Studio.

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku**, rozbalte uzel projektu hello

1. Klikněte pravým tlačítkem na hello **role** uzlu toodisplay hello Kontextová nabídka. Hello místní nabídce vyberte **přidat**, pak vyberte existující webovou roli nebo role pracovního procesu z aktuální řešení hello nebo vytvořte projekt role web nebo worker. Můžete také vybrat příslušný projekt, jako je například projekt webové aplikace ASP.NET a přidružit projekt role.

    ![Nabídka Možnosti tooadd projekt role tooan Azure cloudové služby](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a>Odebrání role z cloudové služby Azure
Hello následující kroky vás provedou odebrání role web nebo worker z projektu Azure cloud service v sadě Visual Studio.

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku**, rozbalte uzel projektu hello

1. Rozbalte hello **role** uzlu.

1. Klikněte pravým tlačítkem na hello uzlu chcete tooremove a, hello místní nabídce vyberte **odebrat**. 

    ![Nabídka Možnosti tooadd tooan role cloudové služby Azure](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-tooan-azure-cloud-service-project"></a>Nové přidání projekt role tooan Azure cloudové služby
Pokud odeberete roli z projekt cloudové služby, ale později se rozhodnete tooadd hello zpátky roli toohello projektu, jenom deklarace role hello a základní atributy, jako například koncových bodů a diagnostické informace, jsou přidány. Žádné další prostředky ani odkazy jsou přidány toohello `ServiceDefinition.csdef` soubor nebo toohello `ServiceConfiguration.cscfg` souboru. Pokud chcete tyto informace tooadd, je nutné toomanually přidejte ho zpátky do těchto souborů.

Například je možné odebrat role webové služby a později rozhodnete tooadd této role zpátky do vašeho řešení. Pokud to uděláte, dojde k chybě. tooprevent tato chyba, máte tooadd hello `<LocalResources>` element uvedené v následující hello XML zpět do hello `ServiceDefinition.csdef` souboru. Použijte název hello hello role webové služby, které jste přidali zpět do projektu hello jako součást atributu hello název pro hello  **<LocalStorage>**  element. V tomto příkladu je název hello role webové služby hello **WCFServiceWebRole1**.

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

## <a name="next-steps"></a>Další kroky
- [Konfigurovat hello role pro cloudové služby Azure pomocí sady Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)

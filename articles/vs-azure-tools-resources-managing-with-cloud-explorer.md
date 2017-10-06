---
title: "aaaManaging Azure prostředků pomocí Průzkumníka cloudu | Microsoft Docs"
description: "Zjistěte, jak toouse Průzkumník cloudu toobrowse a spravovat prostředky Azure v sadě Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/25/2017
ms.author: kraigb
ms.openlocfilehash: 8a81660074d5d04be063df9e25076b7a97586514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a>Spravovat hello prostředky přidružené k vaší účty Azure v cloudu Průzkumníka Visual Studio
Průzkumník cloudu umožňuje vám tooview vašich prostředků Azure a skupiny prostředků, kontrolovat jejich vlastnosti a provádět akce diagnostiky klíče vývojáře z Visual Studia. 

Jako hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), Průzkumník cloudu je založený na Azure Resource Manager zásobníku hello. Proto Průzkumník cloudu rozumí prostředkům, například skupin prostředků Azure a službami Azure, jako třeba logiku aplikace a aplikace API a podporuje [řízení přístupu na základě role](active-directory/role-based-access-control-configure.md) (RBAC). 

## <a name="prerequisites"></a>Požadavky
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) s hello **úloha Azure** vybrána, nebo starší verze sady Visual Studio s hello [Microsoft Azure SDK pro .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).
- Účet Microsoft Azure – Pokud nemáte účet, můžete [zaregistrovat k bezplatné zkušební verzi](http://go.microsoft.com/fwlink/?LinkId=623901) nebo [aktivovat výhody předplatitele sady Visual Studio](http://go.microsoft.com/fwlink/?LinkId=623901).

> [!NOTE]
> Vyberte tooview Průzkumník cloudu **zobrazení** > **Průzkumník cloudu** v řádku nabídek hello.   
> 
> 

## <a name="add-an-azure-account-toocloud-explorer"></a>Přidat účtu Azure tooCloud Explorer
tooview hello prostředky přidružené k účtu Azure, je nejprve nutno přidat hello účet tooCloud Explorer. 

1. V **Průzkumník cloudu**, vyberte **nastavení účtu Azure**.

    ![Ikona nastavení účtu Azure Průzkumníku cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Vyberte **nový účet přidal**. 

    ![Odkaz Přidat účet Průzkumník cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. Přihlaste se toohello účet Azure, jejichž zdroje, které má toobrowse. 

1. Po přihlášení tooan účet Azure, zobrazí se hello odběry přidružené k tomuto účtu. Vyberte hello zaškrtávací políčka pro předplatná účet hello chcete toobrowse a pak vyberte **použít**. 
 
    ![Průzkumník cloudu: Vyberte předplatná Azure toodisplay](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. Po výběru hello odběry jejichž prostředků, které chcete zobrazit toobrowse, tyto odběry a prostředky v hello Průzkumník cloudu.

    ![Průzkumník prostředků výpis pro účet Azure v cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a>Odeberte účet Azure z Průzkumníku cloudu 

1. V **Průzkumník cloudu**, vyberte **nastavení účtu Azure**.

    ![Ikona nastavení účtu Azure Průzkumníku cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Vyberte další toohello účet chcete tooremove, **odebrat**.

    ![Ikona nastavení účtu Azure Průzkumníku cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a>Zobrazit typy prostředků nebo skupiny prostředků
tooview prostředků Azure, můžete zvolit buď **typy prostředků** nebo **skupiny prostředků** zobrazení.

1. V **Průzkumník cloudu**, vyberte hello rozevírací zobrazení prostředků.

    ![Cloud Explorer rozevíracího seznamu tooselect hello požadovaného zobrazení prostředků](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. Hello místní nabídce vyberte hello požadovaného zobrazení: 

    - **Typy prostředků** zobrazení – zobrazení běžných hello používá na hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), zobrazuje vašich prostředků Azure uspořádány do kategorií podle jejich typu, jako jsou webové aplikace, účty úložiště a virtuálních počítačů. 
    - **Skupiny prostředků** zobrazit - prostředky Azure rozděluje skupinou hello prostředků Azure, ke kterému jsou přidružené. Skupina prostředků je sada prostředky Azure, obvykle používají v konkrétní aplikaci. toolearn Další informace o skupin prostředků Azure, najdete v části [přehled Azure Resource Manageru](./azure-resource-manager/resource-group-overview.md).

    Hello následující obrázek ukazuje porovnání hello dvě zobrazení prostředků:

    ![Porovnání zobrazení Průzkumníka prostředků cloudu](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a>Zobrazení a přejděte prostředky v Průzkumníku cloudu
toonavigate tooan prostředků Azure a zobrazit její informace v Průzkumníku cloudu, rozbalte položku hello typu nebo související skupiny prostředků a pak vyberte hello prostředků. Když vyberete prostředek, informace se zobrazí v hello dvě karty - **akce** a **vlastnosti** – dole hello v Průzkumníku cloudu. 

- **Akce** kartě - seznamy hello akce může trvat v Průzkumníku cloudu pro hello vybraný prostředek. Můžete také zobrazit tyto možnosti kliknutím pravým tlačítkem na hello prostředků tooview kontextovou nabídku.

- **Vlastnosti** kartě - zobrazí vlastnosti hello hello prostředků, například jeho typ, národní prostředí a prostředků skupiny, ke kterému je přiřazeno.

Hello následující obrázek ukazuje příklad porovnání zobrazené na jednotlivých kartách pro aplikační službu:

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

Každý prostředek má akce hello **otevřít na portálu**. Pokud zvolíte tuto akci, Průzkumník cloudu zobrazí hello vybrané prostředků v hello [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). Hello **otevřít na portálu** funkce je užitečný pro navigaci toodeeply vnořených prostředků.

Další akce a hodnot vlastností mohou zobrazit i podle hello prostředků Azure. Webové aplikace a aplikace logiky také mít například hello akce **otevřít v prohlížeči** a **připojit ladicí program** kromě příliš**otevřít na portálu**. Editory tooopen akce se zobrazí, když zvolíte účet úložiště objektů blob, fronty nebo tabulky. Azure aplikací mít **URL** a **stav** vlastností, zatímco prostředky úložiště mají vlastnosti klíče a připojovací řetězce.

## <a name="find-resources-in-cloud-explorer"></a>Vyhledávání prostředků v Průzkumníku cloudu
toolocate prostředky s konkrétním názvem v rámci vašich předplatných účet Azure, zadejte název hello v hello **vyhledávání** pole v Průzkumníku cloudu.

![Vyhledávání prostředků v Průzkumníku cloudu](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

Při zadávání znaků v hello **vyhledávání** pole jenom prostředky, které splňují tyto znaky se zobrazí ve stromu hello prostředků.

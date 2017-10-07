---
title: "aaaManage oprávnění a přístupová práva pomocí RBAC - Azure RBAC | Microsoft Docs"
description: "Začínáme se správou přístupu pomocí řízení přístupu na základě role Azure v hello portálu Azure. Pomocí oprávnění tooassign přiřazení role ve vašem adresáři."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 1c133b2b57b49d85f0e12a318c7997478e095fb9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-role-based-access-control-in-hello-azure-portal"></a>Začínáme s řízením přístupu na základě rolí v hello portálu Azure
Zaměřené na zabezpečení společnosti by měla soustředit na poskytnutí zaměstnanci hello přesný oprávnění, které potřebují. Příliš mnoho oprávnění můžou zpřístupnit tooattackers účtu. Příliš málo oprávnění znamená, že zaměstnanci nelze práci efektivně. Azure na základě rolí řízení přístupu (RBAC) pomáhá vyřešit tento problém tak, že nabídka vyladění správy přístupu pro Azure.

Pomocí RBAC, můžete v rámci týmu oddělit povinností a udělit pouze hello množství toousers přístup, že potřebují tooperform svou práci. Namísto udělení každý uživatel neomezený oprávnění v vašeho předplatného Azure nebo prostředky, můžete povolit jenom určité akce. Například použijte RBAC toolet jednoho zaměstnance spravovat virtuální počítače v předplatném, zatímco jiné můžete spravovat SQL databáze, v rámci hello stejné předplatné.

## <a name="basics-of-access-management-in-azure"></a>Základy správy přístupu v Azure
Každé předplatné Azure je přidružen jeden adresář Azure Active Directory (AD). Uživatelé, skupiny a aplikací z adresáře můžete spravovat prostředky v hello předplatného Azure. Přiřaďte tyto přístupová práva pomocí hello portálu Azure, nástroje příkazového řádku Azure a rozhraní API pro správu Azure.

Udělit přístup přiřazením toousers hello odpovídající RBAC role, skupiny a aplikace v určité oboru. předplatné, skupinu prostředků nebo jediný zdroj, může být Hello rozsah přiřazení role. Role přiřazené v nadřazeném oboru také uděluje přístup toohello podřízené objekty jsou v něm obsažena. Například uživatel s skupiny prostředků tooa přístupu můžete spravovat všechny hello prostředky, které obsahuje, jako jsou weby, virtuální počítače a podsítě.

![Vztah mezi elementy Azure Active Directory – diagram](./media/role-based-access-control-what-is/rbac_aad.png)

role RBAC Hello, který přiřadíte Určuje, jaké prostředky hello uživatele, skupiny nebo aplikace můžete spravovat v rámci tohoto oboru.

## <a name="built-in-roles"></a>Vestavěné role
Azure RBAC má tři základní rolí, které se vztahují tooall typy prostředků:

* **Vlastník** má úplný přístup k prostředkům tooall včetně tooothers přístup správné toodelegate hello.
* **Přispěvatel** můžete vytvářet a spravovat všechny typy prostředků Azure, ale nelze udělit přístup tooothers.
* **Čtečka** můžete zobrazit stávající prostředky Azure.

Hello zbytek hello RBAC role v Azure povolit správu konkrétních prostředků Azure. Například hello role Přispěvatel virtuálních počítačů umožňuje toocreate hello uživatele a spravovat virtuální počítače. Nedává je přístup toohello virtuální sítě nebo hello podsítě, která hello virtuální počítač připojí k. 

[Předdefinované role RBAC](role-based-access-built-in-roles.md) seznamy hello role v Azure k dispozici. Určuje hello operace a že každý předdefinovaná role uděluje toousers oboru. Pokud se díváte toodefine vlastní role pro ještě větší kontrolu, najdete v části Jak toobuild [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Dědění prostředků hierarchie a přístupu
* Každý **předplatné** v Azure patří tooonly jeden adresář. (Ale každý adresář může mít více než jedno předplatné).
* Každý **skupiny prostředků** patří tooonly jedno předplatné.
* Každý **prostředků** patří tooonly jedné skupiny prostředků.

V podřízené obory dědí přístup, který udělíte na nadřazené rozsahy. Například:

* Přiřadíte hello čtečky role tooan Azure AD skupina hello obor předplatného. Hello členy této skupiny můžete zobrazit všechny skupiny prostředků a prostředků v předplatném hello.
* Hello Přispěvatel role tooan aplikace v oboru skupiny prostředků hello přiřazení. Umožňuje spravovat prostředky všechny typy v příslušné skupině prostředků, ale ne další skupiny zdrojů v předplatném hello.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC oproti správci classic předplatného
Classic předplatné správci a pomocní Správci mají plný přístup toohello předplatného Azure. Můžou spravovat prostředky pomocí hello [portál Azure](https://portal.azure.com) pomocí rozhraní API Správce Azure Resource Manager nebo hello [portál Azure classic](https://manage.windowsazure.com) a modelu nasazení Azure classic. V modelu RBAC hello přiřazené klasických správců hello roli vlastníka na obor předplatného hello.

Pouze hello portál Azure a nové rozhraní API Správce Azure Resource Manager podpory Azure RBAC hello. Uživatelé a aplikace, které jsou přiřazené role RBAC nelze používat portál hello správu classic a modelu nasazení Azure classic hello.

## <a name="authorization-for-management-vs-data-operations"></a>Oprávnění pro správu oproti operace dat
Azure RBAC jen podporuje management operace hello prostředky služby Azure v hello portál Azure a rozhraní API Správce Azure Resource Manager. Všechny operace úrovně dat pro prostředky Azure se nejde autorizovat. Například může autorizovat někdo toomanage účty úložiště, ale ne objekty BLOB toohello nebo tabulky v rámci účtu úložiště. Podobně databáze SQL je možné spravovat, ale není hello tabulek v něm.

## <a name="next-steps"></a>Další kroky
* Začínáme s [řízení přístupu na základě rolí v hello portál Azure](role-based-access-control-configure.md).
* V tématu hello [předdefinované role RBAC](role-based-access-built-in-roles.md)
* Definujte své [Vlastní role funkce řízení přístupu na základě role v Azure](role-based-access-control-custom-roles.md).

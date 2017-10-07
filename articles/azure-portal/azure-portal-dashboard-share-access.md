---
title: "aaaShare Azure portálu řídicí panely pomocí RBAC | Microsoft Docs"
description: "Tento článek vysvětluje, jak hello tooshare a řídicího panelu portálu Azure pomocí řízení přístupu na základě Role."
services: azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: tomfitz
ms.openlocfilehash: b12f9f8582596ee14aa8bfdfb4772cc139e3bf45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Sdílet řídicí panely Azure pomocí řízení přístupu na základě Role
Po dokončení konfigurace na řídicí panel, můžete ji publikovat a sdílet s jinými uživateli ve vaší organizaci. Povolit ostatním tooview řídicího panelu pomocí služby Azure [řízení přístupu na základě Role](../active-directory/role-based-access-control-configure.md). Přiřadit uživatele nebo skupinu uživatelů tooa role, a tato role definuje, zda uživatelům, můžete zobrazit nebo upravit hello publikované řídicího panelu. 

Všechny publikované řídicí panely jsou implementované jako prostředky Azure, což znamená, že existovat jako spravovat položky v rámci vašeho předplatného a jsou obsaženy ve skupině prostředků.  Z hlediska řízení k přístupu řídicí panely jsou nejsou jiné než jiné prostředky, jako je virtuální počítač nebo účet úložiště.

> [!TIP]
> Jednotlivé dlaždice na řídicím panelu hello vynutit vlastní požadavky řízení přístupu na základě hello prostředků, které se zobrazí.  Proto můžete navrhnout řídicí panel, který je sdílen široce přitom hello dat na jednotlivé dlaždice.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Principy řízení přístupu pro řídicí panely
S na základě rolí řízení přístupu (RBAC) můžete přiřadit uživatele tooroles na třech různých úrovních oboru:

* předplatné
* skupina prostředků
* Prostředek

Hello oprávnění, které přiřadíte se dědí z předplatného dolů toohello prostředků. řídicí panel publikované Hello je prostředek. Proto už můžete mít tooroles uživatele přiřazené pro hello předplatné, které také pro řídicí panel publikované hello. 

Tady je příklad.  Řekněme, že máte předplatné Azure a různé členy týmu přiřazení rolí hello **vlastníka**, **Přispěvatel**, nebo **čtečky** pro předplatné hello. Uživatelé, kteří jsou vlastníci a přispěvatelé jsou možné toolist, zobrazení, vytvořit, upravit nebo odstranit řídicí panely v rámci předplatného hello.  Uživatelé, kteří jsou čtečky jsou možné toolist a zobrazení řídicích panelů, ale nelze upravit nebo odstranit je.  Uživatelé s přístupem čtečky jsou publikované řídicího panelu může toomake místní úpravy tooa (jako je třeba při řešení potíží s), ale nebylo možné toopublish jsou tyto změny zpět toohello serveru.  Budou mít možnost toomake hello privátní kopie řídicího panelu hello sami

Je však také přiřadit skupinu prostředků toohello oprávnění, která obsahuje několik řídicí panely nebo tooan jednotlivé řídicí panel. Například může rozhodnout, že skupina uživatelů by měl mají omezenou oprávnění napříč hello předplatné, ale větší přístup tooa konkrétní řídicího panelu. Můžete přiřadit role tooa tyto uživatele pro tento řídicí panel. 

## <a name="publish-dashboard"></a>publikovat řídicí panel
Předpokládejme, že dokončení konfigurace na řídicí panel, že chcete tooshare ke skupině uživatelů v rámci vašeho předplatného. Následující postup Hello zobrazit v ní vlastní skupinu s názvem Správci úložiště, ale můžete název vaší skupiny ať chcete. Informace o vytváření skupiny služby Active Directory a přidání uživatelů toothat skupiny najdete v tématu [Správa skupin v Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

1. V hello řídicí panel, vyberte **sdílené složky**.
   
     ![Vyberte sdílenou složku](./media/azure-portal-dashboard-share-access/select-share.png)
2. Před udělením přístupu, musíte publikovat hello řídicího panelu. Ve výchozím nastavení, bude řídicí panel hello publikované tooa skupinu prostředků s názvem **řídicí panely**. Vyberte **publikování**.
   
     ![publish](./media/azure-portal-dashboard-share-access/publish.png)

Řídicí panel je nyní publikován. Pokud jsou oprávnění hello zděděno z předplatného hello vhodný, není nutné toodo něco víc. Ostatní uživatelé ve vaší organizaci bude mít tooaccess a upravit řídicí panely hello na základě jejich role úrovně předplatného. Ale v tomto kurzu budeme přiřadit skupinu tooa role uživatele pro tento řídicí panel.

## <a name="assign-access-tooa-dashboard"></a>Přiřadit přístup tooa řídicí panel
1. Po publikování hello řídicí panel, vyberte **Správa uživatelů**.
   
     ![Správa uživatelů](./media/azure-portal-dashboard-share-access/manage-users.png)
2. Zobrazí se seznam stávajícím uživatelům, kteří jsou již přiřazena role pro tento řídicí panel. Seznam stávajících uživatelů bude jiné než hello obrázek níže. S největší pravděpodobností hello přiřazení se dědí z předplatného hello. tooadd nového uživatele nebo skupiny, vyberte **přidat**.
   
     ![Přidat uživatele](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Vyberte roli hello představuje hello oprávnění, které byste chtěli toogrant. V tomto příkladu vyberte **Přispěvatel**.
   
     ![Vyberte roli](./media/azure-portal-dashboard-share-access/select-role.png)
4. Vyberte hello uživatele nebo skupiny, které chcete tooassign toohello role. Pokud nevidíte hello uživatele nebo skupinu, kterou hledáte, v seznamu hello, použijte pole hledání hello. Seznam dostupných skupin bude záviset na hello skupin, které jste vytvořili ve službě Active Directory.
   
     ![Vyberte uživatele](./media/azure-portal-dashboard-share-access/select-user.png) 
5. Až dokončíte přidávání uživatelů nebo skupin, vyberte **OK**. 
6. nové přiřazení Hello se přidá toohello seznam uživatelů. Všimněte si, že jeho **přístup** je uveden jako **přiřazeno** místo **zděděné**.
   
     ![přiřazené role](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Další kroky
* Seznam rolí, najdete v části [RBAC: předdefinované role](../active-directory/role-based-access-built-in-roles.md).
* toolearn o správu prostředků, najdete v části [Azure spravovat prostředky prostřednictvím portálu](resource-group-portal.md).


---
title: "aaaCreate a sdílet řídicí panely Azure portálu | Microsoft Docs"
description: "Tento článek vysvětluje, jak hello toocreate a upravit řídicí panely v portálu Azure."
services: azure-portal
documentationcenter: 
author: sewatson
manager: timlt
editor: tysonn
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/06/2016
ms.author: sewatson
ms.openlocfilehash: 0facd10fe3284d7ad9a9e29791e5af5b5b95c97f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-dashboards-in-hello-azure-portal"></a>Vytvoření a sdílení řídicích panelů v hello portálu Azure
Můžete vytvořit více řídicí panely a sdílet je s ostatními uživateli, kteří mají přístup tooyour Azure odběry.  Tento článek probírá základní hello informace o vytváření, úpravy, publikování a správa toodashboards přístup.

## <a name="create-a-dashboard"></a>Vytvořit řídicí panel
toocreate řídicí panel, vyberte hello **novým řídicím panelem** tlačítko Další toohello aktuální na název řídicího panelu.  

![Vytvořit řídicí panel](./media/azure-portal-dashboards/new-dashboard.png)

Tato akce vytvoří řídicí panel nový, prázdný, privátní a vloží je do režimu přizpůsobení, kde můžete název řídicího panelu a přidání nebo změna uspořádání dlaždic.  V tomto režimu hello sbalitelné dlaždici Galerie trvá přes hello levé navigační nabídky.  Hello dlaždice galerie umožňuje najít dlaždice pro vaše prostředky Azure různými způsoby: můžete procházet podle [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups), pomocí typ prostředku, pomocí [značky](../azure-resource-manager/resource-group-using-tags.md), nebo pomocí vyhledávání podle názvu prostředku.  

![přizpůsobit řídicí panel](./media/azure-portal-dashboards/customize-dashboard.png)

Přidáte dlaždice přetahováním myší na plochu řídicí panel hello požadovaná místa.

Je nová kategorie **Obecné** pro dlaždice, které nejsou přidružené k určitému prostředku.  V tomto příkladu jsme připnout dlaždice Markdownu hello.  Můžete použít tento řídicí panel dlaždice tooadd vlastní tooyour obsahu.  Hello dlaždice podporuje prostý text, [syntaxe Markdownu](https://daringfireball.net/projects/markdown/syntax)a omezená sada HTML.  (Pro zabezpečení, že nemůžete provádět akce jako vložit `<script>` značky nebo použijte element určité stylů CSS, který může narušovat hello portálu.) 

![Přidat markdownu](./media/azure-portal-dashboards/add-markdown.png)

## <a name="edit-a-dashboard"></a>Úprava řídicího panelu
Po vytvoření řídicího panelu, budete moct připnout dlaždice z Galerie dlaždice hello nebo hello dlaždice reprezentace okna. Umožňuje připnout hello reprezentace naší skupiny prostředků. Můžete buď připnout při procházení hello položek, nebo v okně skupiny prostředků hello. Obou přístupů za následek Připnutí hello dlaždice reprezentace hello skupiny prostředků.

![Toodashboard PIN kód](./media/azure-portal-dashboards/pin-to-dashboard.png)

Po Připnutí hello položku, zobrazí se na řídicím panelu.

![Zobrazení řídicí panel](./media/azure-portal-dashboards/view-dashboard.png)

Teď, když máme dlaždici Markdownu a řídicí panel definovaného toohello skupiny prostředků, jsme změnit velikost a změna uspořádání hello dlaždice do vhodný rozložení.

Mohou přesouváním ukazatele myši a výběrem "...", nebo kliknete pravým tlačítkem na dlaždici se zobrazí všechny příkazy kontextové hello pro tuto dlaždici. Ve výchozím nastavení existují dvě položky:

1. **Odepnout z řídicího panelu** – odebere hello dlaždice z řídicího panelu hello
2. **Přizpůsobení** – vstupuje do režimu přizpůsobení

![přizpůsobení dlaždice](./media/azure-portal-dashboards/customize-tile.png)

Výběrem přizpůsobit, můžete změnit velikost a změnit pořadí dlaždice. tooresize dlaždice, vyberte hello novou velikost z hello kontextové nabídky, jak ukazuje následující obrázek hello.

![změnit velikost dlaždice](./media/azure-portal-dashboards/resize-tile.png)

Nebo, pokud hello dlaždice podporuje jakékoli velikosti, můžete přetáhnout hello dolní pravém rohu toohello potřeby velikost.

![změnit velikost dlaždice](./media/azure-portal-dashboards/resize-corner.png)

Po změně velikosti dlaždic, zobrazení řídicího panelu hello.

![dlaždice zobrazení](./media/azure-portal-dashboards/view-tile.png)

Jakmile budete hotovi, přizpůsobení řídicí panel, jednoduše vyberte hello **provádí přizpůsobení** tooexit režimu přizpůsobení, nebo klikněte pravým tlačítkem a vyberte **provádí přizpůsobení** hello místní nabídce.

## <a name="publish-a-dashboard-and-manage-access-control"></a>Řídicí panel publikování a správě řízení přístupu
Když vytváříte řídicí panel, je soukromé ve výchozím nastavení, což znamená, že jsou hello jediná osoba, která můžete zobrazit.  toomake ho viditelné tooothers použít hello **sdílenou složku** hello tlačítko, které se zobrazí spolu s jinými příkazy řídicího panelu.

![sdílení řídicího panelu.](./media/azure-portal-dashboards/share-dashboard.png)

Zobrazí se výzva toochoose skupinu předplatného a prostředků pro váš řídicí panel toobe publikovány na. tooseamlessly integrovat do hello ekosystém řídicí panely, implementovali jsme sdílené řídicí panely jako prostředky Azure (tak nemůžete sdílet zadáním e-mailovou adresu).  Se řídí přístup k informacím toohello zobrazit většinou hello dlaždice portálu hello [řízení přístupu na základě Role Azure](../active-directory/role-based-access-control-configure.md). Sdílené řídicí panely jsou z hlediska řízení k přístupu, nijak neliší od virtuálního počítače nebo účtu úložiště.  

Řekněme, že máte předplatné Azure a členy týmu přiřazení rolí hello **vlastníka**, **Přispěvatel**, nebo **čtečky** hello předplatného.  Uživatelé, kteří jsou vlastníci a přispěvatelé jsou možné toolist, zobrazení, vytvořit, upravit nebo odstranit řídicí panely v rámci tohoto předplatného.  Uživatelé, kteří jsou čtečky jsou možné toolist a zobrazení řídicích panelů, ale nelze upravit nebo odstranit je.  Uživatelé s přístupem čtečky jsou sdílené řídicího panelu může toomake místní úpravy tooa, ale nebylo možné toopublish tyto změny zpět toohello serveru.  Ale udělat privátní kopie hello řídicího panelu pro vlastní použití.  Jednotlivé dlaždice na řídicím panelu hello jako vždy vynutit svoje vlastní pravidla pro řízení přístupu na základě hello prostředků, které odpovídají.  

Pro usnadnění práce je portál hello publikování prostředí příručky směrem vzor umístění řídicí panely ve skupině prostředků jste volali metodu **řídicí panely**.  

![publikovat řídicí panel](./media/azure-portal-dashboards/publish-dashboard.png)

Můžete také toopublish řídicí panel tooa určité skupiny prostředků.  Hello řízení přístupu pro tento řídicí panel odpovídá hello řízení přístupu pro skupinu prostředků hello.  Uživatelé, kteří můžou spravovat hello prostředky v příslušné skupině prostředků mít také přístup toohello řídicí panely.

![publikování skupiny tooresource řídicí panel](./media/azure-portal-dashboards/publish-to-resource-group.png)

Po publikování řídicího panelu hello **sdílení + přístup** ovládacího prvku podokno bude aktualizovat a můžete zobrazit informace o hello publikované řídicí panel, včetně odkaz přístup uživatelského toomanage toohello řídicího panelu.  Tento odkaz spustí hello standardní řízení přístupu na základě Role okno používá toomanage přístup pro jakýmikoli prostředky Azure.  Vždy získáte zpět toothis zobrazení tak, že vyberete **sdílené složky**.

![Správa řízení přístupu](./media/azure-portal-dashboards/manage-access.png)

## <a name="next-steps"></a>Další kroky
* toomanage prostředky, najdete v části [Azure spravovat prostředky prostřednictvím portálu](../azure-resource-manager/resource-group-portal.md).
* toodeploy prostředky, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](../azure-resource-manager/resource-group-template-deploy-portal.md).


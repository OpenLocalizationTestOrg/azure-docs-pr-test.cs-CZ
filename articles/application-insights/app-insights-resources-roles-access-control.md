---
title: "aaaResources, rolí a řízení přístupu ve službě Azure Application Insights | Microsoft Docs"
description: "Vlastníci, přispěvatelé a čtenáři z vaší organizace statistiky."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: a6f6ca0443b5f60239f094606e124f856967d8ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Prostředky, role a řízení přístupu ve službě Application Insights
Můžete řídit, kdo má číst a aktualizovat data tooyour přístup v Azure [Application Insights][start], pomocí [řízení přístupu na základě Role ve službě Microsoft Azure](../active-directory/role-based-access-control-configure.md).

> [!IMPORTANT]
> Přiřadit přístup toousers v hello **skupiny prostředků nebo předplatného** toowhich prostředek vaší aplikace patří - není v hello prostředků sám sebe. Přiřadit hello **Přispěvatel součásti Application Insights** role. Tím se zajistí uniform řízení přístupu tooweb testy a výstrah spolu s prostředek vaší aplikace. [Další informace](#access).
> 
> 

## <a name="resources-groups-and-subscriptions"></a>Prostředky, skupiny a odběry
První, definice:

* **Prostředek** – instance služby Microsoft Azure. Prostředku Application Insights shromažďuje, analyzuje a zobrazí hello telemetrická data odesílaná z vaší aplikace.  Ostatní typy prostředků Azure obsahují webové aplikace, databáze a virtuální počítače.
  
    otevřít vaše prostředky toosee hello [portálu Azure][portal], přihlaste se a klikněte na možnost všechny prostředky. toofind prostředku, typ součástí jeho název v poli filtru hello.
  
    ![Seznam prostředků Azure](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Skupina prostředků** ] [ group] -každý prostředek patří tooone skupiny. Skupina je pohodlný způsob toomanage související prostředky, zejména pro řízení přístupu. Skupina by mohlo webové aplikace, aplikace hello toomonitor prostředku Application Insights a tookeep prostředků úložiště exportují do jeden prostředek data.

    ![Vyberte možnost Procházet, skupiny prostředků, a potom vyberte skupinu](./media/app-insights-resources-roles-access-control/11-group.png)

* [**Předplatné** ](https://manage.windowsazure.com) -toouse Application Insights nebo jiných prostředků Azure, přihlášení tooan předplatného Azure. Každá skupina prostředků patří tooone předplatné Azure, kde jste zvolte svůj balíček ceny a, pokud je předplatné organizace hello členy a jejich oprávnění k přístupu.
* [**Účet Microsoft** ] [ account] -hello uživatelské jméno a heslo použít toosign v tooMicrosoft Azure odběry, XBox Live, Outlook.com a dalších služeb společnosti Microsoft.

## <a name="access"></a>Řízení přístupu ve skupině prostředků hello
Je důležité toounderstand, že v přidání toohello prostředku, který jste vytvořili pro aplikace, existují také samostatné skrytá prostředky pro výstrahy a webové testy. Jsou připojené toohello stejné [skupiny prostředků](#resource-group) jako aplikace. Můžete také mít ukládat jinými službami Azure zde například weby nebo úložiště.

![Prostředky ve službě Application Insights](./media/app-insights-resources-roles-access-control/00-resources.png)

toocontrol přístup k toothese prostředkům, které se proto doporučuje:

* Řízení přístupu na hello **skupiny prostředků nebo předplatného** úroveň.
* Přiřadit hello **součást Statistika aplikace Přispěvatel** role toousers. To umožňuje jejich tooedit webové testy, výstrahy a prostředky Application Insights, bez zadání tooany přístup jiných služeb ve skupině hello.

## <a name="tooprovide-access-tooanother-user"></a>tooprovide přístup tooanother uživatele
Musíte mít předplatné toohello oprávnění vlastníka nebo skupinu prostředků hello.

musí mít uživatel Hello [Account Microsoft][account], nebo přístup tootheir [Account organizace Microsoft](../active-directory/sign-up-organization.md). Můžete zadat tooindividuals přístup a také toouser skupiny definované v Azure Active Directory.

#### <a name="navigate-toohello-resource-group"></a>Přejděte toohello skupiny prostředků
Přidáte hello uživatele existuje.

![V okně prostředků aplikace v otevřít Essentials, otevřete skupinu prostředků hello a existuje vyberte uživatele, nebo nastavení. Klikněte na Přidat.](./media/app-insights-resources-roles-access-control/01-add-user.png)

Nebo můžete přejít do jiné úrovně a přidejte uživatele toohello hello předplatného.

#### <a name="select-a-role"></a>Vyberte roli
![Vyberte roli pro nového uživatele hello](./media/app-insights-resources-roles-access-control/03-role.png)

| Role | Ve skupině prostředků hello |
| --- | --- |
| Vlastník |Můžete změnit všechno včetně přístupu uživatele |
| Přispěvatel |Můžete upravit jakýkoli, včetně všech prostředků |
| Přispěvatel Statistika součásti aplikace |Můžete upravit prostředky Application Insights, webové testy a upozornění |
| Čtenář |Můžete zobrazit, ale není nic nezmění. |

'Úpravy' zahrnuje vytváření, odstraňování a aktualizace:

* Zdroje
* Testy webu
* Výstrahy
* Průběžný export

#### <a name="select-hello-user"></a>Vyberte uživatele hello

Není-li hello uživatele, kterého chcete, aby v adresáři hello, můžete pozvat, každý uživatel s účtem Microsoft.
(Pokud používají služby, jako je Outlook.com, OneDrive, Windows Phone nebo XBox Live, budou mít účet Microsoft.)

## <a name="related-content"></a>Související obsah

* [Řízení přístupu v Azure na základě role](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md

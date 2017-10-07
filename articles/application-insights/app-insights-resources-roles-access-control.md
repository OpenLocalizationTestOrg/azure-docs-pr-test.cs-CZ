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
# <a name="resources-roles-and-access-control-in-application-insights"></a><span data-ttu-id="06fad-103">Prostředky, role a řízení přístupu ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="06fad-103">Resources, roles, and access control in Application Insights</span></span>
<span data-ttu-id="06fad-104">Můžete řídit, kdo má číst a aktualizovat data tooyour přístup v Azure [Application Insights][start], pomocí [řízení přístupu na základě Role ve službě Microsoft Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="06fad-104">You can control who has read and update access tooyour data in Azure [Application Insights][start], by using [Role-based access control in Microsoft Azure](../active-directory/role-based-access-control-configure.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06fad-105">Přiřadit přístup toousers v hello **skupiny prostředků nebo předplatného** toowhich prostředek vaší aplikace patří - není v hello prostředků sám sebe.</span><span class="sxs-lookup"><span data-stu-id="06fad-105">Assign access toousers in hello **resource group or subscription** toowhich your application resource belongs - not in hello resource itself.</span></span> <span data-ttu-id="06fad-106">Přiřadit hello **Přispěvatel součásti Application Insights** role.</span><span class="sxs-lookup"><span data-stu-id="06fad-106">Assign hello **Application Insights component contributor** role.</span></span> <span data-ttu-id="06fad-107">Tím se zajistí uniform řízení přístupu tooweb testy a výstrah spolu s prostředek vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="06fad-107">This ensures uniform control of access tooweb tests and alerts along with your application resource.</span></span> <span data-ttu-id="06fad-108">[Další informace](#access).</span><span class="sxs-lookup"><span data-stu-id="06fad-108">[Learn more](#access).</span></span>
> 
> 

## <a name="resources-groups-and-subscriptions"></a><span data-ttu-id="06fad-109">Prostředky, skupiny a odběry</span><span class="sxs-lookup"><span data-stu-id="06fad-109">Resources, groups and subscriptions</span></span>
<span data-ttu-id="06fad-110">První, definice:</span><span class="sxs-lookup"><span data-stu-id="06fad-110">First, some definitions:</span></span>

* <span data-ttu-id="06fad-111">**Prostředek** – instance služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="06fad-111">**Resource** - An instance of a Microsoft Azure service.</span></span> <span data-ttu-id="06fad-112">Prostředku Application Insights shromažďuje, analyzuje a zobrazí hello telemetrická data odesílaná z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="06fad-112">Your Application Insights resource collects, analyzes and displays hello telemetry data sent from your application.</span></span>  <span data-ttu-id="06fad-113">Ostatní typy prostředků Azure obsahují webové aplikace, databáze a virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="06fad-113">Other types of Azure resources include web apps, databases, and VMs.</span></span>
  
    <span data-ttu-id="06fad-114">otevřít vaše prostředky toosee hello [portálu Azure][portal], přihlaste se a klikněte na možnost všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="06fad-114">toosee your resources, open hello [Azure Portal][portal], sign in, and click All Resources.</span></span> <span data-ttu-id="06fad-115">toofind prostředku, typ součástí jeho název v poli filtru hello.</span><span class="sxs-lookup"><span data-stu-id="06fad-115">toofind a resource, type part of its name in hello filter field.</span></span>
  
    ![Seznam prostředků Azure](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* <span data-ttu-id="06fad-117">[**Skupina prostředků** ] [ group] -každý prostředek patří tooone skupiny.</span><span class="sxs-lookup"><span data-stu-id="06fad-117">[**Resource group**][group] - Every resource belongs tooone group.</span></span> <span data-ttu-id="06fad-118">Skupina je pohodlný způsob toomanage související prostředky, zejména pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="06fad-118">A group is a convenient way toomanage related resources, particularly for access control.</span></span> <span data-ttu-id="06fad-119">Skupina by mohlo webové aplikace, aplikace hello toomonitor prostředku Application Insights a tookeep prostředků úložiště exportují do jeden prostředek data.</span><span class="sxs-lookup"><span data-stu-id="06fad-119">For example, into one resource group you could put a Web App, an Application Insights resource toomonitor hello app, and a Storage resource tookeep exported data.</span></span>

    ![Vyberte možnost Procházet, skupiny prostředků, a potom vyberte skupinu](./media/app-insights-resources-roles-access-control/11-group.png)

* <span data-ttu-id="06fad-121">[**Předplatné** ](https://manage.windowsazure.com) -toouse Application Insights nebo jiných prostředků Azure, přihlášení tooan předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="06fad-121">[**Subscription**](https://manage.windowsazure.com) - toouse Application Insights or other Azure resources, you sign in tooan Azure subscription.</span></span> <span data-ttu-id="06fad-122">Každá skupina prostředků patří tooone předplatné Azure, kde jste zvolte svůj balíček ceny a, pokud je předplatné organizace hello členy a jejich oprávnění k přístupu.</span><span class="sxs-lookup"><span data-stu-id="06fad-122">Every resource group belongs tooone Azure subscription, where you choose your price package and, if it's an organization subscription, choose hello members and their access permissions.</span></span>
* <span data-ttu-id="06fad-123">[**Účet Microsoft** ] [ account] -hello uživatelské jméno a heslo použít toosign v tooMicrosoft Azure odběry, XBox Live, Outlook.com a dalších služeb společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="06fad-123">[**Microsoft account**][account] - hello username and password that you use toosign in tooMicrosoft Azure subscriptions, XBox Live, Outlook.com, and other Microsoft services.</span></span>

## <span data-ttu-id="06fad-124"><a name="access"></a>Řízení přístupu ve skupině prostředků hello</span><span class="sxs-lookup"><span data-stu-id="06fad-124"><a name="access"></a> Control access in hello resource group</span></span>
<span data-ttu-id="06fad-125">Je důležité toounderstand, že v přidání toohello prostředku, který jste vytvořili pro aplikace, existují také samostatné skrytá prostředky pro výstrahy a webové testy.</span><span class="sxs-lookup"><span data-stu-id="06fad-125">It's important toounderstand that in addition toohello resource you created for your application, there are also separate hidden resources for alerts and web tests.</span></span> <span data-ttu-id="06fad-126">Jsou připojené toohello stejné [skupiny prostředků](#resource-group) jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="06fad-126">They are attached toohello same [resource group](#resource-group) as your application.</span></span> <span data-ttu-id="06fad-127">Můžete také mít ukládat jinými službami Azure zde například weby nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="06fad-127">You might also have put other Azure services in there, such as websites or storage.</span></span>

![Prostředky ve službě Application Insights](./media/app-insights-resources-roles-access-control/00-resources.png)

<span data-ttu-id="06fad-129">toocontrol přístup k toothese prostředkům, které se proto doporučuje:</span><span class="sxs-lookup"><span data-stu-id="06fad-129">toocontrol access toothese resources it's therefore recommended to:</span></span>

* <span data-ttu-id="06fad-130">Řízení přístupu na hello **skupiny prostředků nebo předplatného** úroveň.</span><span class="sxs-lookup"><span data-stu-id="06fad-130">Control access at hello **resource group or subscription** level.</span></span>
* <span data-ttu-id="06fad-131">Přiřadit hello **součást Statistika aplikace Přispěvatel** role toousers.</span><span class="sxs-lookup"><span data-stu-id="06fad-131">Assign hello **Application Insights Component contributor** role toousers.</span></span> <span data-ttu-id="06fad-132">To umožňuje jejich tooedit webové testy, výstrahy a prostředky Application Insights, bez zadání tooany přístup jiných služeb ve skupině hello.</span><span class="sxs-lookup"><span data-stu-id="06fad-132">This allows them tooedit web tests, alerts, and Application Insights resources, without providing access tooany other services in hello group.</span></span>

## <a name="tooprovide-access-tooanother-user"></a><span data-ttu-id="06fad-133">tooprovide přístup tooanother uživatele</span><span class="sxs-lookup"><span data-stu-id="06fad-133">tooprovide access tooanother user</span></span>
<span data-ttu-id="06fad-134">Musíte mít předplatné toohello oprávnění vlastníka nebo skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="06fad-134">You must have Owner rights toohello subscription or hello resource group.</span></span>

<span data-ttu-id="06fad-135">musí mít uživatel Hello [Account Microsoft][account], nebo přístup tootheir [Account organizace Microsoft](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="06fad-135">hello user must have a [Microsoft Account][account], or access tootheir [organizational Microsoft Account](../active-directory/sign-up-organization.md).</span></span> <span data-ttu-id="06fad-136">Můžete zadat tooindividuals přístup a také toouser skupiny definované v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="06fad-136">You can provide access tooindividuals, and also toouser groups defined in Azure Active Directory.</span></span>

#### <a name="navigate-toohello-resource-group"></a><span data-ttu-id="06fad-137">Přejděte toohello skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="06fad-137">Navigate toohello resource group</span></span>
<span data-ttu-id="06fad-138">Přidáte hello uživatele existuje.</span><span class="sxs-lookup"><span data-stu-id="06fad-138">Add hello user there.</span></span>

![V okně prostředků aplikace v otevřít Essentials, otevřete skupinu prostředků hello a existuje vyberte uživatele, nebo nastavení.](./media/app-insights-resources-roles-access-control/01-add-user.png)

<span data-ttu-id="06fad-141">Nebo můžete přejít do jiné úrovně a přidejte uživatele toohello hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="06fad-141">Or you could go up another level and add hello user toohello Subscription.</span></span>

#### <a name="select-a-role"></a><span data-ttu-id="06fad-142">Vyberte roli</span><span class="sxs-lookup"><span data-stu-id="06fad-142">Select a role</span></span>
![Vyberte roli pro nového uživatele hello](./media/app-insights-resources-roles-access-control/03-role.png)

| <span data-ttu-id="06fad-144">Role</span><span class="sxs-lookup"><span data-stu-id="06fad-144">Role</span></span> | <span data-ttu-id="06fad-145">Ve skupině prostředků hello</span><span class="sxs-lookup"><span data-stu-id="06fad-145">In hello resource group</span></span> |
| --- | --- |
| <span data-ttu-id="06fad-146">Vlastník</span><span class="sxs-lookup"><span data-stu-id="06fad-146">Owner</span></span> |<span data-ttu-id="06fad-147">Můžete změnit všechno včetně přístupu uživatele</span><span class="sxs-lookup"><span data-stu-id="06fad-147">Can change anything, including user access</span></span> |
| <span data-ttu-id="06fad-148">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="06fad-148">Contributor</span></span> |<span data-ttu-id="06fad-149">Můžete upravit jakýkoli, včetně všech prostředků</span><span class="sxs-lookup"><span data-stu-id="06fad-149">Can edit anything, including all resources</span></span> |
| <span data-ttu-id="06fad-150">Přispěvatel Statistika součásti aplikace</span><span class="sxs-lookup"><span data-stu-id="06fad-150">Application Insights Component contributor</span></span> |<span data-ttu-id="06fad-151">Můžete upravit prostředky Application Insights, webové testy a upozornění</span><span class="sxs-lookup"><span data-stu-id="06fad-151">Can edit Application Insights resources, web tests and alerts</span></span> |
| <span data-ttu-id="06fad-152">Čtenář</span><span class="sxs-lookup"><span data-stu-id="06fad-152">Reader</span></span> |<span data-ttu-id="06fad-153">Můžete zobrazit, ale není nic nezmění.</span><span class="sxs-lookup"><span data-stu-id="06fad-153">Can view but not change anything</span></span> |

<span data-ttu-id="06fad-154">'Úpravy' zahrnuje vytváření, odstraňování a aktualizace:</span><span class="sxs-lookup"><span data-stu-id="06fad-154">'Editing' includes creating, deleting and updating:</span></span>

* <span data-ttu-id="06fad-155">Zdroje</span><span class="sxs-lookup"><span data-stu-id="06fad-155">Resources</span></span>
* <span data-ttu-id="06fad-156">Testy webu</span><span class="sxs-lookup"><span data-stu-id="06fad-156">Web tests</span></span>
* <span data-ttu-id="06fad-157">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="06fad-157">Alerts</span></span>
* <span data-ttu-id="06fad-158">Průběžný export</span><span class="sxs-lookup"><span data-stu-id="06fad-158">Continuous export</span></span>

#### <a name="select-hello-user"></a><span data-ttu-id="06fad-159">Vyberte uživatele hello</span><span class="sxs-lookup"><span data-stu-id="06fad-159">Select hello user</span></span>

<span data-ttu-id="06fad-160">Není-li hello uživatele, kterého chcete, aby v adresáři hello, můžete pozvat, každý uživatel s účtem Microsoft.</span><span class="sxs-lookup"><span data-stu-id="06fad-160">If hello user you want isn't in hello directory, you can invite anyone with a Microsoft account.</span></span>
<span data-ttu-id="06fad-161">(Pokud používají služby, jako je Outlook.com, OneDrive, Windows Phone nebo XBox Live, budou mít účet Microsoft.)</span><span class="sxs-lookup"><span data-stu-id="06fad-161">(If they use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, they have a Microsoft account.)</span></span>

## <a name="related-content"></a><span data-ttu-id="06fad-162">Související obsah</span><span class="sxs-lookup"><span data-stu-id="06fad-162">Related content</span></span>

* [<span data-ttu-id="06fad-163">Řízení přístupu v Azure na základě role</span><span class="sxs-lookup"><span data-stu-id="06fad-163">Role based access control in Azure</span></span>](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md

---
title: "Prostředky, rolí a řízení přístupu ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: c979a8bfbeecacc7c0bbc112e02a4b68e874c219
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="resources-roles-and-access-control-in-application-insights"></a><span data-ttu-id="be73b-103">Prostředky, role a řízení přístupu ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="be73b-103">Resources, roles, and access control in Application Insights</span></span>
<span data-ttu-id="be73b-104">Můžete řídit, kdo má číst a aktualizovat přístupu k datům v Azure [Application Insights][start], pomocí [řízení přístupu na základě Role ve službě Microsoft Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="be73b-104">You can control who has read and update access to your data in Azure [Application Insights][start], by using [Role-based access control in Microsoft Azure](../active-directory/role-based-access-control-configure.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="be73b-105">Přiřadit uživatelům v přístupu **skupiny prostředků nebo předplatného** , ke které prostředek vaší aplikace patří - není v prostředku sám sebe.</span><span class="sxs-lookup"><span data-stu-id="be73b-105">Assign access to users in the **resource group or subscription** to which your application resource belongs - not in the resource itself.</span></span> <span data-ttu-id="be73b-106">Přiřazení **Přispěvatel součásti Application Insights** role.</span><span class="sxs-lookup"><span data-stu-id="be73b-106">Assign the **Application Insights component contributor** role.</span></span> <span data-ttu-id="be73b-107">Tím se zajistí uniform řízení přístupu k webové testy a výstrahy společně s prostředek vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="be73b-107">This ensures uniform control of access to web tests and alerts along with your application resource.</span></span> <span data-ttu-id="be73b-108">[Další informace](#access).</span><span class="sxs-lookup"><span data-stu-id="be73b-108">[Learn more](#access).</span></span>
> 
> 

## <a name="resources-groups-and-subscriptions"></a><span data-ttu-id="be73b-109">Prostředky, skupiny a odběry</span><span class="sxs-lookup"><span data-stu-id="be73b-109">Resources, groups and subscriptions</span></span>
<span data-ttu-id="be73b-110">První, definice:</span><span class="sxs-lookup"><span data-stu-id="be73b-110">First, some definitions:</span></span>

* <span data-ttu-id="be73b-111">**Prostředek** – instance služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="be73b-111">**Resource** - An instance of a Microsoft Azure service.</span></span> <span data-ttu-id="be73b-112">Prostředku Application Insights shromažďuje, analyzuje a zobrazuje telemetrická data odeslaná z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="be73b-112">Your Application Insights resource collects, analyzes and displays the telemetry data sent from your application.</span></span>  <span data-ttu-id="be73b-113">Ostatní typy prostředků Azure obsahují webové aplikace, databáze a virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="be73b-113">Other types of Azure resources include web apps, databases, and VMs.</span></span>
  
    <span data-ttu-id="be73b-114">Chcete-li zobrazit vaše prostředky, otevřete [portálu Azure][portal], přihlaste se a klikněte na možnost všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="be73b-114">To see your resources, open the [Azure Portal][portal], sign in, and click All Resources.</span></span> <span data-ttu-id="be73b-115">Najít prostředek, napište část názvu do pole filtru.</span><span class="sxs-lookup"><span data-stu-id="be73b-115">To find a resource, type part of its name in the filter field.</span></span>
  
    ![Seznam prostředků Azure](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* <span data-ttu-id="be73b-117">[**Skupina prostředků** ] [ group] -každý prostředek patří do jedné skupiny.</span><span class="sxs-lookup"><span data-stu-id="be73b-117">[**Resource group**][group] - Every resource belongs to one group.</span></span> <span data-ttu-id="be73b-118">Skupina je pohodlný způsob, jak spravovat související prostředky, zejména pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="be73b-118">A group is a convenient way to manage related resources, particularly for access control.</span></span> <span data-ttu-id="be73b-119">Do jedné skupiny prostředků může například dávat webovou aplikaci, prostředek Application Insights pro sledování aplikace a prostředků úložiště, aby exportovaná data.</span><span class="sxs-lookup"><span data-stu-id="be73b-119">For example, into one resource group you could put a Web App, an Application Insights resource to monitor the app, and a Storage resource to keep exported data.</span></span>

    ![Vyberte možnost Procházet, skupiny prostředků, a potom vyberte skupinu](./media/app-insights-resources-roles-access-control/11-group.png)

* <span data-ttu-id="be73b-121">[**Předplatné** ](https://manage.windowsazure.com) – Pokud chcete použít Application Insights nebo další prostředky Azure, přihlaste se k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="be73b-121">[**Subscription**](https://manage.windowsazure.com) - To use Application Insights or other Azure resources, you sign in to an Azure subscription.</span></span> <span data-ttu-id="be73b-122">Každá skupina prostředků patří do jedno předplatné, kde vyberete vašeho balíčku ceny a, pokud je předplatné organizace, zvolte členy a jejich oprávnění k přístupu.</span><span class="sxs-lookup"><span data-stu-id="be73b-122">Every resource group belongs to one Azure subscription, where you choose your price package and, if it's an organization subscription, choose the members and their access permissions.</span></span>
* <span data-ttu-id="be73b-123">[**Účet Microsoft** ] [ account] -uživatelské jméno a heslo, které používáte pro přihlášení k Microsoft Azure odběry, XBox Live, Outlook.com a dalších služeb společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="be73b-123">[**Microsoft account**][account] - The username and password that you use to sign in to Microsoft Azure subscriptions, XBox Live, Outlook.com, and other Microsoft services.</span></span>

## <span data-ttu-id="be73b-124"><a name="access"></a>Řízení přístupu ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="be73b-124"><a name="access"></a> Control access in the resource group</span></span>
<span data-ttu-id="be73b-125">Je důležité si uvědomit, že kromě prostředek, který jste vytvořili pro vaši aplikaci, existují také samostatné skrytá prostředky pro výstrahy a webové testy.</span><span class="sxs-lookup"><span data-stu-id="be73b-125">It's important to understand that in addition to the resource you created for your application, there are also separate hidden resources for alerts and web tests.</span></span> <span data-ttu-id="be73b-126">Jsou připojené ke stejné [skupiny prostředků](#resource-group) jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="be73b-126">They are attached to the same [resource group](#resource-group) as your application.</span></span> <span data-ttu-id="be73b-127">Můžete také mít ukládat jinými službami Azure zde například weby nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="be73b-127">You might also have put other Azure services in there, such as websites or storage.</span></span>

![Prostředky ve službě Application Insights](./media/app-insights-resources-roles-access-control/00-resources.png)

<span data-ttu-id="be73b-129">K řízení přístupu na tyto prostředky se proto doporučuje:</span><span class="sxs-lookup"><span data-stu-id="be73b-129">To control access to these resources it's therefore recommended to:</span></span>

* <span data-ttu-id="be73b-130">Řízení přístupu na **skupiny prostředků nebo předplatného** úroveň.</span><span class="sxs-lookup"><span data-stu-id="be73b-130">Control access at the **resource group or subscription** level.</span></span>
* <span data-ttu-id="be73b-131">Přiřazení **součást Statistika aplikace Přispěvatel** role pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="be73b-131">Assign the **Application Insights Component contributor** role to users.</span></span> <span data-ttu-id="be73b-132">To umožňuje upravovat webové testy, výstrahy a prostředky Application Insights, bez poskytnutí přístupu k jiným službám ve skupině.</span><span class="sxs-lookup"><span data-stu-id="be73b-132">This allows them to edit web tests, alerts, and Application Insights resources, without providing access to any other services in the group.</span></span>

## <a name="to-provide-access-to-another-user"></a><span data-ttu-id="be73b-133">K poskytování přístupu k jinému uživateli</span><span class="sxs-lookup"><span data-stu-id="be73b-133">To provide access to another user</span></span>
<span data-ttu-id="be73b-134">Musí mít oprávnění vlastníka pro předplatné nebo skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="be73b-134">You must have Owner rights to the subscription or the resource group.</span></span>

<span data-ttu-id="be73b-135">Uživatel musí mít [Account Microsoft][account], nebo přístup k jejich [Account organizace Microsoft](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="be73b-135">The user must have a [Microsoft Account][account], or access to their [organizational Microsoft Account](../active-directory/sign-up-organization.md).</span></span> <span data-ttu-id="be73b-136">Zajistíte přístup jednotlivcům a taky do skupin uživatelů, které jsou definované v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="be73b-136">You can provide access to individuals, and also to user groups defined in Azure Active Directory.</span></span>

#### <a name="navigate-to-the-resource-group"></a><span data-ttu-id="be73b-137">Přejděte do skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="be73b-137">Navigate to the resource group</span></span>
<span data-ttu-id="be73b-138">Přidejte uživatele existuje.</span><span class="sxs-lookup"><span data-stu-id="be73b-138">Add the user there.</span></span>

![V okně prostředků aplikace v otevřít Essentials, otevřete skupinu prostředků a existuje vyberte uživatele, nebo nastavení.](./media/app-insights-resources-roles-access-control/01-add-user.png)

<span data-ttu-id="be73b-141">Nebo můžete přejít do jiné úrovně a přidejte uživatele k předplatnému.</span><span class="sxs-lookup"><span data-stu-id="be73b-141">Or you could go up another level and add the user to the Subscription.</span></span>

#### <a name="select-a-role"></a><span data-ttu-id="be73b-142">Vyberte roli</span><span class="sxs-lookup"><span data-stu-id="be73b-142">Select a role</span></span>
![Vyberte role pro nového uživatele](./media/app-insights-resources-roles-access-control/03-role.png)

| <span data-ttu-id="be73b-144">Role</span><span class="sxs-lookup"><span data-stu-id="be73b-144">Role</span></span> | <span data-ttu-id="be73b-145">Ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="be73b-145">In the resource group</span></span> |
| --- | --- |
| <span data-ttu-id="be73b-146">Vlastník</span><span class="sxs-lookup"><span data-stu-id="be73b-146">Owner</span></span> |<span data-ttu-id="be73b-147">Můžete změnit všechno včetně přístupu uživatele</span><span class="sxs-lookup"><span data-stu-id="be73b-147">Can change anything, including user access</span></span> |
| <span data-ttu-id="be73b-148">Přispěvatel</span><span class="sxs-lookup"><span data-stu-id="be73b-148">Contributor</span></span> |<span data-ttu-id="be73b-149">Můžete upravit jakýkoli, včetně všech prostředků</span><span class="sxs-lookup"><span data-stu-id="be73b-149">Can edit anything, including all resources</span></span> |
| <span data-ttu-id="be73b-150">Přispěvatel Statistika součásti aplikace</span><span class="sxs-lookup"><span data-stu-id="be73b-150">Application Insights Component contributor</span></span> |<span data-ttu-id="be73b-151">Můžete upravit prostředky Application Insights, webové testy a upozornění</span><span class="sxs-lookup"><span data-stu-id="be73b-151">Can edit Application Insights resources, web tests and alerts</span></span> |
| <span data-ttu-id="be73b-152">Čtenář</span><span class="sxs-lookup"><span data-stu-id="be73b-152">Reader</span></span> |<span data-ttu-id="be73b-153">Můžete zobrazit, ale není nic nezmění.</span><span class="sxs-lookup"><span data-stu-id="be73b-153">Can view but not change anything</span></span> |

<span data-ttu-id="be73b-154">'Úpravy' zahrnuje vytváření, odstraňování a aktualizace:</span><span class="sxs-lookup"><span data-stu-id="be73b-154">'Editing' includes creating, deleting and updating:</span></span>

* <span data-ttu-id="be73b-155">Zdroje</span><span class="sxs-lookup"><span data-stu-id="be73b-155">Resources</span></span>
* <span data-ttu-id="be73b-156">Testy webu</span><span class="sxs-lookup"><span data-stu-id="be73b-156">Web tests</span></span>
* <span data-ttu-id="be73b-157">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="be73b-157">Alerts</span></span>
* <span data-ttu-id="be73b-158">Průběžný export</span><span class="sxs-lookup"><span data-stu-id="be73b-158">Continuous export</span></span>

#### <a name="select-the-user"></a><span data-ttu-id="be73b-159">Vybrat uživatele</span><span class="sxs-lookup"><span data-stu-id="be73b-159">Select the user</span></span>

<span data-ttu-id="be73b-160">Pokud uživatel, který má být není v adresáři, můžete pozvat, každý uživatel s účtem Microsoft.</span><span class="sxs-lookup"><span data-stu-id="be73b-160">If the user you want isn't in the directory, you can invite anyone with a Microsoft account.</span></span>
<span data-ttu-id="be73b-161">(Pokud používají služby, jako je Outlook.com, OneDrive, Windows Phone nebo XBox Live, budou mít účet Microsoft.)</span><span class="sxs-lookup"><span data-stu-id="be73b-161">(If they use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, they have a Microsoft account.)</span></span>

## <a name="related-content"></a><span data-ttu-id="be73b-162">Související obsah</span><span class="sxs-lookup"><span data-stu-id="be73b-162">Related content</span></span>

* [<span data-ttu-id="be73b-163">Řízení přístupu v Azure na základě role</span><span class="sxs-lookup"><span data-stu-id="be73b-163">Role based access control in Azure</span></span>](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md

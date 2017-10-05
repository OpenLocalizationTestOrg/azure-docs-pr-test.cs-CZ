---
title: "Upgrade clusteru služby Azure Service Fabric | Microsoft Docs"
description: "Upgrade Service Fabric kód nebo konfigurace používající cluster Service Fabric, včetně nastavení režimu aktualizace clusteru upgrade certifikáty, přidání aplikace porty, provádění oprav operačního systému, a tak dále. Co můžete očekávat, když se provádí upgrady?"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: chackdan
ms.openlocfilehash: 7ea71ab891583c51b3c07a4d0a9f0b4f54e56669
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a><span data-ttu-id="3a69f-104">Upgrade clusteru služby Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3a69f-104">Upgrade an Azure Service Fabric cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3a69f-105">Azure clusteru</span><span class="sxs-lookup"><span data-stu-id="3a69f-105">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="3a69f-106">Samostatné clusteru</span><span class="sxs-lookup"><span data-stu-id="3a69f-106">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

<span data-ttu-id="3a69f-107">Pro libovolný systém moderní návrhu pro zdokonalovány je klíčem k dosažení dlouhodobý úspěch vašeho produktu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-107">For any modern system, designing for upgradability is key to achieving long-term success of your product.</span></span> <span data-ttu-id="3a69f-108">Cluster služby Azure Service Fabric je na prostředek, který vlastníte, ale je částečně spravováno společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3a69f-108">An Azure Service Fabric cluster is a resource that you own, but is partly managed by Microsoft.</span></span> <span data-ttu-id="3a69f-109">Tento článek popisuje, co je spravováno automaticky a co můžete nakonfigurovat sami.</span><span class="sxs-lookup"><span data-stu-id="3a69f-109">This article describes what is managed automatically and what you can configure yourself.</span></span>

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="3a69f-110">Řízení verze fabric, která běží na clusteru</span><span class="sxs-lookup"><span data-stu-id="3a69f-110">Controlling the fabric version that runs on your Cluster</span></span>
<span data-ttu-id="3a69f-111">Můžete nastavit na automatické fabric aktualizace vydané společností Microsoft nebo můžete vybrat verzi podporované fabric, že chcete clusteru na clusteru.</span><span class="sxs-lookup"><span data-stu-id="3a69f-111">You can set your cluster to receive automatic fabric upgrades as they are released by Microsoft or you can select a supported fabric version you want your cluster to be on.</span></span>

<span data-ttu-id="3a69f-112">To provedete nastavením konfigurace clusteru "upgradeMode" na portálu nebo pomocí Správce prostředků v době vytvoření nebo později na clusteru s podporou za provozu</span><span class="sxs-lookup"><span data-stu-id="3a69f-112">You do this by setting the "upgradeMode" cluster configuration on the portal or using Resource Manager at the time of creation or later on a live cluster</span></span> 

> [!NOTE]
> <span data-ttu-id="3a69f-113">Ujistěte se, že zachovat clusteru vždy používá verzi podporovanou prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="3a69f-113">Make sure to keep your cluster running a supported fabric version always.</span></span> <span data-ttu-id="3a69f-114">Jak a kdy jsme oznamujeme vydání nové verze služby infrastruktury, předchozí verze budou označena k ukončení podpory po 60 dnů od tohoto dne.</span><span class="sxs-lookup"><span data-stu-id="3a69f-114">As and when we announce the release of a new version of service fabric, the previous version is marked for end of support after a minimum of 60 days from that date.</span></span> <span data-ttu-id="3a69f-115">nové verze není zmínka [na blogu týmu služby fabric](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="3a69f-115">the  The new releases are announced [on the service fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="3a69f-116">Nové verze je k dispozici, pak vybrat.</span><span class="sxs-lookup"><span data-stu-id="3a69f-116">The new release is available to choose then.</span></span> 
> 
> 

<span data-ttu-id="3a69f-117">14 dnů před vypršení platnosti verze, které váš clusteru je spuštěn, událost stavu se vygeneruje, umístí clusteru do stavu upozornění.</span><span class="sxs-lookup"><span data-stu-id="3a69f-117">14 days prior to the expiry of the release your cluster is running, a health event is generated that puts your cluster into a warning health state.</span></span> <span data-ttu-id="3a69f-118">Cluster zůstane ve stavu upozornění, dokud neprovedete upgrade na verzi, podporované prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="3a69f-118">The cluster remains in a warning state until you upgrade to a supported fabric version.</span></span>

### <a name="setting-the-upgrade-mode-via-portal"></a><span data-ttu-id="3a69f-119">Nastavení režimu upgradu prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="3a69f-119">Setting the upgrade mode via portal</span></span>
<span data-ttu-id="3a69f-120">Clusteru můžete nastavit na automatické nebo ruční při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="3a69f-120">You can set the cluster to automatic or manual when you are creating the cluster.</span></span>

![Create_Manualmode][Create_Manualmode]

<span data-ttu-id="3a69f-122">Clusteru můžete nastavit na automatické nebo ruční v clusteru s podporou za provozu, pomocí možnosti Správa.</span><span class="sxs-lookup"><span data-stu-id="3a69f-122">You can set the cluster to automatic or manual when on a live cluster, using the manage experience.</span></span> 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a><span data-ttu-id="3a69f-123">Upgrade na novou verzi v clusteru, který je nastavený na ruční režim přes portál.</span><span class="sxs-lookup"><span data-stu-id="3a69f-123">Upgrading to a new version on a cluster that is set to Manual mode via portal.</span></span>
<span data-ttu-id="3a69f-124">Pokud chcete upgradovat na novou verzi, všechny, které musíte udělat je z rozevíracího seznamu vyberte verzi k dispozici a uložte.</span><span class="sxs-lookup"><span data-stu-id="3a69f-124">To upgrade to a new version, all you need to do is select the available version from the dropdown and save.</span></span> <span data-ttu-id="3a69f-125">Upgrade pro Fabric získá spuštěna automaticky.</span><span class="sxs-lookup"><span data-stu-id="3a69f-125">The Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="3a69f-126">Zásady stavu clusteru (kombinaci uzlu stav a stav všechny aplikace běžící v clusteru) jsou, kterého se během upgradu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-126">The cluster health policies (a combination of node health and the health all the applications running in the cluster) are adhered to during the upgrade.</span></span>

<span data-ttu-id="3a69f-127">Pokud zásady stavu clusteru nejsou splněny, upgrade je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="3a69f-127">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="3a69f-128">Projděte dolů tento dokument Další informace o tom, jak nastavit tyto zásady vlastní stavu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-128">Scroll down this document to read more on how to set those custom health policies.</span></span> 

<span data-ttu-id="3a69f-129">Po opravě problémy, jejichž výsledkem vrácení zpět, budete muset znovu zahájit upgrade podle následujících kroků stejná jako před.</span><span class="sxs-lookup"><span data-stu-id="3a69f-129">Once you have fixed the issues that resulted in the rollback, you need to initiate the upgrade again, by following the same steps as before.</span></span>

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a><span data-ttu-id="3a69f-131">Nastavení režimu upgradu pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="3a69f-131">Setting the upgrade mode via a Resource Manager template</span></span>
<span data-ttu-id="3a69f-132">Přidejte definici zdrojů Microsoft.ServiceFabric/clusters konfiguraci "upgradeMode" a "clusterCodeVersion" nastavena na jednu z verzí podporované prostředků infrastruktury, jak je uvedeno níže a pak nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="3a69f-132">Add the "upgradeMode" configuration to the Microsoft.ServiceFabric/clusters resource definition and set the "clusterCodeVersion" to one of the supported fabric versions as shown below and then deploy the template.</span></span> <span data-ttu-id="3a69f-133">Platné hodnoty pro "upgradeMode" jsou "Ruční" nebo "Automatické"</span><span class="sxs-lookup"><span data-stu-id="3a69f-133">The valid values for "upgradeMode" are "Manual" or "Automatic"</span></span>

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a><span data-ttu-id="3a69f-135">Upgrade na novou verzi v clusteru, který je nastavený na ruční režim pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="3a69f-135">Upgrading to a new version on a cluster that is set to Manual mode via a Resource Manager template.</span></span>
<span data-ttu-id="3a69f-136">Až bude cluster v ručním režimu, upgrade na novou verzi, změňte "clusterCodeVersion" na podporovanou verzi a nasadit.</span><span class="sxs-lookup"><span data-stu-id="3a69f-136">When the cluster is in Manual mode, to upgrade to a new version, change the "clusterCodeVersion" to a supported version and deploy it.</span></span> <span data-ttu-id="3a69f-137">Nasazení šablony, zejména kopance o upgrade pro Fabric získá spuštěna automaticky.</span><span class="sxs-lookup"><span data-stu-id="3a69f-137">The deployment of the template, kicks of the Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="3a69f-138">Zásady stavu clusteru (kombinaci uzlu stav a stav všechny aplikace běžící v clusteru) jsou, kterého se během upgradu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-138">The cluster health policies (a combination of node health and the health all the applications running in the cluster) are adhered to during the upgrade.</span></span>

<span data-ttu-id="3a69f-139">Pokud zásady stavu clusteru nejsou splněny, upgrade je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="3a69f-139">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="3a69f-140">Projděte dolů tento dokument Další informace o tom, jak nastavit tyto zásady vlastní stavu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-140">Scroll down this document to read more on how to set those custom health policies.</span></span> 

<span data-ttu-id="3a69f-141">Po opravě problémy, jejichž výsledkem vrácení zpět, budete muset znovu zahájit upgrade podle následujících kroků stejná jako před.</span><span class="sxs-lookup"><span data-stu-id="3a69f-141">Once you have fixed the issues that resulted in the rollback, you need to initiate the upgrade again, by following the same steps as before.</span></span>

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a><span data-ttu-id="3a69f-142">Získejte seznam všech dostupných verze pro všechna prostředí pro dané předplatné</span><span class="sxs-lookup"><span data-stu-id="3a69f-142">Get list of all available version for all environments for a given subscription</span></span>
<span data-ttu-id="3a69f-143">Spusťte následující příkaz a měli byste obdržet výstup podobně jako tento.</span><span class="sxs-lookup"><span data-stu-id="3a69f-143">Run the following command, and you should get an output similar to this.</span></span>

<span data-ttu-id="3a69f-144">"supportExpiryUtc" informuje vaší Pokud danou verzi vyprší nebo vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="3a69f-144">"supportExpiryUtc" tells your when a given release is expiring or has expired.</span></span> <span data-ttu-id="3a69f-145">Nejnovější verze nemá platné datum - má hodnotu "9999-12-31T23:59:59.9999999", což právě znamená, že datum vypršení platnosti ještě není nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="3a69f-145">The latest release does not have a valid date - it has a value of "9999-12-31T23:59:59.9999999", which just means that the expiry date is not yet set.</span></span>

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a><span data-ttu-id="3a69f-146">Fabric upgradu chování, pokud cluster upgradovat režimu je automatický</span><span class="sxs-lookup"><span data-stu-id="3a69f-146">Fabric upgrade behavior when the cluster Upgrade Mode is Automatic</span></span>
<span data-ttu-id="3a69f-147">Společnost Microsoft udržuje kód prostředků infrastruktury a konfigurace, která běží v clusteru služby Azure.</span><span class="sxs-lookup"><span data-stu-id="3a69f-147">Microsoft maintains the fabric code and configuration that runs in an Azure cluster.</span></span> <span data-ttu-id="3a69f-148">Můžeme provést automatické upgrady monitorovaných softwaru podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3a69f-148">We perform automatic monitored upgrades to the software on an as-needed basis.</span></span> <span data-ttu-id="3a69f-149">Tyto upgrady může být kódu, konfigurace nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="3a69f-149">These upgrades could be code, configuration, or both.</span></span> <span data-ttu-id="3a69f-150">Abyste měli jistotu, že vaše aplikace vyskytne žádný dopad nebo minimální dopad kvůli tyto upgrady, jsme upgrady provádět v následujících fází:</span><span class="sxs-lookup"><span data-stu-id="3a69f-150">To make sure that your application suffers no impact or minimal impact due to these upgrades, we perform the upgrades in the following phases:</span></span>

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a><span data-ttu-id="3a69f-151">Fáze 1: Upgrade se provádí pomocí všechny zásady stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="3a69f-151">Phase 1: An upgrade is performed by using all cluster health policies</span></span>
<span data-ttu-id="3a69f-152">V průběhu této fáze upgrady pokračovat jednu upgradovací doménu najednou a aplikace, které byly spuštěny v clusteru dál běžet bez odstávky.</span><span class="sxs-lookup"><span data-stu-id="3a69f-152">During this phase, the upgrades proceed one upgrade domain at a time, and the applications that were running in the cluster continue to run without any downtime.</span></span> <span data-ttu-id="3a69f-153">Zásady stavu clusteru (kombinaci uzlu stav a stav všechny aplikace běžící v clusteru) jsou, kterého se během upgradu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-153">The cluster health policies (a combination of node health and the health all the applications running in the cluster) are adhered to during the upgrade.</span></span>

<span data-ttu-id="3a69f-154">Pokud zásady stavu clusteru nejsou splněny, upgrade je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="3a69f-154">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="3a69f-155">E-mailu se pak odešlou vlastníka předplatného.</span><span class="sxs-lookup"><span data-stu-id="3a69f-155">Then an email is sent to the owner of the subscription.</span></span> <span data-ttu-id="3a69f-156">E-mailu obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="3a69f-156">The email contains the following information:</span></span>

* <span data-ttu-id="3a69f-157">Oznámení, že jsme museli vrátit zpět na upgrade clusteru.</span><span class="sxs-lookup"><span data-stu-id="3a69f-157">Notification that we had to roll back a cluster upgrade.</span></span>
* <span data-ttu-id="3a69f-158">Navrhované nápravné akce, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="3a69f-158">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="3a69f-159">Počet dní (ne), dokud jsme provést fáze 2.</span><span class="sxs-lookup"><span data-stu-id="3a69f-159">The number of days (n) until we execute Phase 2.</span></span>

<span data-ttu-id="3a69f-160">Pokusíme se provést stejný upgrade několik vícekrát v případě, že jakéhokoli upgradu se nezdařilo z důvodů infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="3a69f-160">We try to execute the same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="3a69f-161">Po n dní od data, kdy byla odeslána e-mailu budeme pokračovat fáze 2.</span><span class="sxs-lookup"><span data-stu-id="3a69f-161">After the n days from the date the email was sent, we proceed to Phase 2.</span></span>

<span data-ttu-id="3a69f-162">Pokud jsou splněny zásady stavu clusteru, je upgrade považuje za úspěšnou a označeny jako dokončené.</span><span class="sxs-lookup"><span data-stu-id="3a69f-162">If the cluster health policies are met, the upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="3a69f-163">Může dojít během počáteční upgradu nebo některého z opakování upgradu v této fázi.</span><span class="sxs-lookup"><span data-stu-id="3a69f-163">This can happen during the initial upgrade or any of the upgrade reruns in this phase.</span></span> <span data-ttu-id="3a69f-164">Neexistuje žádné potvrzení e-mailu úspěšně spustit.</span><span class="sxs-lookup"><span data-stu-id="3a69f-164">There is no email confirmation of a successful run.</span></span> <span data-ttu-id="3a69f-165">Toto je vyhnout odesílání, že jste příliš mnoho e-mailů – příjem e-mailu měla by se zobrazit jako výjimku do normální.</span><span class="sxs-lookup"><span data-stu-id="3a69f-165">This is to avoid sending you too many emails--receiving an email should be seen as an exception to normal.</span></span> <span data-ttu-id="3a69f-166">Očekáváme, že většina upgrady clusteru úspěšné bez dopadu na dostupnost vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3a69f-166">We expect most of the cluster upgrades to succeed without impacting your application availability.</span></span>

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a><span data-ttu-id="3a69f-167">Fáze 2: Upgrade se provádí pomocí výchozího stavu pouze zásady</span><span class="sxs-lookup"><span data-stu-id="3a69f-167">Phase 2: An upgrade is performed by using default health policies only</span></span>
<span data-ttu-id="3a69f-168">Zásady stavu v této fázi jsou nastavené tak, že počet aplikací, které byly na začátku upgradu v pořádku, zůstane stejný po dobu trvání procesu upgradu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-168">The health policies in this phase are set in such a way that the number of applications that were healthy at the beginning of the upgrade remains the same for the duration of the upgrade process.</span></span> <span data-ttu-id="3a69f-169">Jako fáze 1 upgrady fáze 2 pokračovat jednu upgradovací doménu najednou a aplikace, které byly spuštěny v clusteru dál běžet bez odstávky.</span><span class="sxs-lookup"><span data-stu-id="3a69f-169">As in Phase 1, the Phase 2 upgrades proceed one upgrade domain at a time, and the applications that were running in the cluster continue to run without any downtime.</span></span> <span data-ttu-id="3a69f-170">Zásady stavu clusteru (kombinaci uzlu stav a stav všechny aplikace běžící v clusteru) jsou zachovány po dobu trvání upgradu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-170">The cluster health policies (a combination of node health and the health all the applications running in the cluster) are adhered to for the duration of the upgrade.</span></span>

<span data-ttu-id="3a69f-171">Pokud platí zásady stavu clusteru nejsou splněny, upgrade je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="3a69f-171">If the cluster health policies in effect are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="3a69f-172">E-mailu se pak odešlou vlastníka předplatného.</span><span class="sxs-lookup"><span data-stu-id="3a69f-172">Then an email is sent to the owner of the subscription.</span></span> <span data-ttu-id="3a69f-173">E-mailu obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="3a69f-173">The email contains the following information:</span></span>

* <span data-ttu-id="3a69f-174">Oznámení, že jsme museli vrátit zpět na upgrade clusteru.</span><span class="sxs-lookup"><span data-stu-id="3a69f-174">Notification that we had to roll back a cluster upgrade.</span></span>
* <span data-ttu-id="3a69f-175">Navrhované nápravné akce, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="3a69f-175">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="3a69f-176">Počet dní (ne), dokud jsme provést fáze 3.</span><span class="sxs-lookup"><span data-stu-id="3a69f-176">The number of days (n) until we execute Phase 3.</span></span>

<span data-ttu-id="3a69f-177">Pokusíme se provést stejný upgrade několik vícekrát v případě, že jakéhokoli upgradu se nezdařilo z důvodů infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="3a69f-177">We try to execute the same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="3a69f-178">Připomenutí e-mail je odeslán během několika dní, než n dní jsou.</span><span class="sxs-lookup"><span data-stu-id="3a69f-178">A reminder email is sent a couple of days before n days are up.</span></span> <span data-ttu-id="3a69f-179">Po n dní od data, kdy byla odeslána e-mailu budeme pokračovat fáze 3.</span><span class="sxs-lookup"><span data-stu-id="3a69f-179">After the n days from the date the email was sent, we proceed to Phase 3.</span></span> <span data-ttu-id="3a69f-180">E-mailů, které vám můžeme poslat v fáze 2 bude nutno vážně a musí být přijata nápravné akce.</span><span class="sxs-lookup"><span data-stu-id="3a69f-180">The emails we send you in Phase 2 must be taken seriously and remedial actions must be taken.</span></span>

<span data-ttu-id="3a69f-181">Pokud jsou splněny zásady stavu clusteru, je upgrade považuje za úspěšnou a označeny jako dokončené.</span><span class="sxs-lookup"><span data-stu-id="3a69f-181">If the cluster health policies are met, the upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="3a69f-182">Může dojít během počáteční upgradu nebo některého z opakování upgradu v této fázi.</span><span class="sxs-lookup"><span data-stu-id="3a69f-182">This can happen during the initial upgrade or any of the upgrade reruns in this phase.</span></span> <span data-ttu-id="3a69f-183">Neexistuje žádné potvrzení e-mailu úspěšně spustit.</span><span class="sxs-lookup"><span data-stu-id="3a69f-183">There is no email confirmation of a successful run.</span></span>

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a><span data-ttu-id="3a69f-184">Fáze 3: Upgrade se provádí na základě zásad agresivní stavu</span><span class="sxs-lookup"><span data-stu-id="3a69f-184">Phase 3: An upgrade is performed by using aggressive health policies</span></span>
<span data-ttu-id="3a69f-185">Tyto zásady stavu v této fázi jsou s ohledem na dokončení upgradu, nikoli stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="3a69f-185">These health policies in this phase are geared towards completion of the upgrade rather than the health of the applications.</span></span> <span data-ttu-id="3a69f-186">V této fázi skončili jen několik upgradů clusteru.</span><span class="sxs-lookup"><span data-stu-id="3a69f-186">Very few cluster upgrades end up in this phase.</span></span> <span data-ttu-id="3a69f-187">Pokud váš cluster získá do této fáze, je velmi pravděpodobné, že vaše aplikace se změní na není v pořádku nebo ztrátu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="3a69f-187">If your cluster gets to this phase, there is a good chance that your application becomes unhealthy and/or lose availability.</span></span>

<span data-ttu-id="3a69f-188">Podobně jako u jiných dvě fáze, fáze 3 upgrady pokračovat jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="3a69f-188">Similar to the other two phases, Phase 3 upgrades proceed one upgrade domain at a time.</span></span>

<span data-ttu-id="3a69f-189">Pokud zásady stavu clusteru nejsou splněny, upgrade je vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="3a69f-189">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="3a69f-190">Pokusíme se provést stejný upgrade několik vícekrát v případě, že jakéhokoli upgradu se nezdařilo z důvodů infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="3a69f-190">We try to execute the same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="3a69f-191">Potom je ukotvena clusteru, tak, aby se už nebude podporu nebo upgrady.</span><span class="sxs-lookup"><span data-stu-id="3a69f-191">After that, the cluster is pinned, so that it will no longer receive support and/or upgrades.</span></span>

<span data-ttu-id="3a69f-192">E-mail s tyto informace je odeslán vlastník předplatného, společně s nápravné akce.</span><span class="sxs-lookup"><span data-stu-id="3a69f-192">An email with this information is sent to the subscription owner, along with the remedial actions.</span></span> <span data-ttu-id="3a69f-193">Není Očekáváme, že všechny clustery získat do stavu, kde se nezdařilo fáze 3.</span><span class="sxs-lookup"><span data-stu-id="3a69f-193">We do not expect any clusters to get into a state where Phase 3 has failed.</span></span>

<span data-ttu-id="3a69f-194">Pokud jsou splněny zásady stavu clusteru, je upgrade považuje za úspěšnou a označeny jako dokončené.</span><span class="sxs-lookup"><span data-stu-id="3a69f-194">If the cluster health policies are met, the upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="3a69f-195">Může dojít během počáteční upgradu nebo některého z opakování upgradu v této fázi.</span><span class="sxs-lookup"><span data-stu-id="3a69f-195">This can happen during the initial upgrade or any of the upgrade reruns in this phase.</span></span> <span data-ttu-id="3a69f-196">Neexistuje žádné potvrzení e-mailu úspěšně spustit.</span><span class="sxs-lookup"><span data-stu-id="3a69f-196">There is no email confirmation of a successful run.</span></span>

## <a name="cluster-configurations-that-you-control"></a><span data-ttu-id="3a69f-197">Konfigurace clusteru, které řídíte</span><span class="sxs-lookup"><span data-stu-id="3a69f-197">Cluster configurations that you control</span></span>
<span data-ttu-id="3a69f-198">Kromě možnost nastavit cluster upgradovat režimu, tady jsou konfigurace, které můžete změnit na cluster za provozu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-198">In addition to the ability to set the cluster upgrade mode, Here are the configurations that you can change on a live cluster.</span></span>

### <a name="certificates"></a><span data-ttu-id="3a69f-199">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="3a69f-199">Certificates</span></span>
<span data-ttu-id="3a69f-200">Můžete přidat nové nebo snadno odstranit certifikáty pro cluster a klienta přes portál.</span><span class="sxs-lookup"><span data-stu-id="3a69f-200">You can add new or delete certificates for the cluster and client via the portal easily.</span></span> <span data-ttu-id="3a69f-201">Odkazovat na [tento dokument podrobné pokyny](service-fabric-cluster-security-update-certs-azure.md)</span><span class="sxs-lookup"><span data-stu-id="3a69f-201">Refer to [this document for detailed instructions](service-fabric-cluster-security-update-certs-azure.md)</span></span>

![Snímek obrazovky zobrazující kryptografické otisky certifikátu na portálu Azure.][CertificateUpgrade]

### <a name="application-ports"></a><span data-ttu-id="3a69f-203">Porty aplikace</span><span class="sxs-lookup"><span data-stu-id="3a69f-203">Application ports</span></span>
<span data-ttu-id="3a69f-204">Porty aplikace můžete změnit změnou vlastnosti prostředků nástroj pro vyrovnávání zatížení, které jsou přidruženy k typu uzlu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-204">You can change application ports by changing the Load Balancer resource properties that are associated with the node type.</span></span> <span data-ttu-id="3a69f-205">Můžete použít na portálu, nebo můžete použít Správce prostředků PowerShell přímo.</span><span class="sxs-lookup"><span data-stu-id="3a69f-205">You can use the portal, or you can use Resource Manager PowerShell directly.</span></span>

<span data-ttu-id="3a69f-206">Otevřete nový port na všech virtuálních počítačích v typ uzlu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="3a69f-206">To open a new port on all VMs in a node type, do the following:</span></span>

1. <span data-ttu-id="3a69f-207">Přidáte nový test odpovídající služba Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3a69f-207">Add a new probe to the appropriate load balancer.</span></span>
   
    <span data-ttu-id="3a69f-208">Pokud jste nasadili cluster pomocí portálu, jsou nástroje pro vyrovnávání zatížení s názvem "LB-název skupiny prostředků – NodeTypename", jednu pro každý typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-208">If you deployed your cluster by using the portal, the load balancers are named "LB-name of the Resource group-NodeTypename", one for each node type.</span></span> <span data-ttu-id="3a69f-209">Vzhledem k tomu, že jsou pouze ve skupině prostředků jedinečné názvy nástroje pro vyrovnávání zatížení, je nejlepší Pokud vyhledávání v rámci určité skupiny zdrojů.</span><span class="sxs-lookup"><span data-stu-id="3a69f-209">Since the load balancer names are unique only within a resource group, it is best if you search for them under a specific resource group.</span></span>
   
    ![Snímek obrazovky, který ukazuje sondu přidání do Vyrovnávání zatížení na portálu.][AddingProbes]
2. <span data-ttu-id="3a69f-211">Přidáte nové pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="3a69f-211">Add a new rule to the load balancer.</span></span>
   
    <span data-ttu-id="3a69f-212">Přidáte nové pravidlo Vyrovnávání zatížení, stejné pomocí testu, který jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="3a69f-212">Add a new rule to the same load balancer by using the probe that you created in the previous step.</span></span>
   
    ![Přidání nové pravidlo Vyrovnávání zatížení na portálu.][AddingLBRules]

### <a name="placement-properties"></a><span data-ttu-id="3a69f-214">Vlastnosti umístění</span><span class="sxs-lookup"><span data-stu-id="3a69f-214">Placement properties</span></span>
<span data-ttu-id="3a69f-215">Pro každý typ uzlu můžete přidat vlastní umístění vlastnosti, které chcete používat ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="3a69f-215">For each of the node types, you can add custom placement properties that you want to use in your applications.</span></span> <span data-ttu-id="3a69f-216">Typ uzlu je ve výchozím nastavení, kterou můžete použít bez přidání explicitně.</span><span class="sxs-lookup"><span data-stu-id="3a69f-216">NodeType is a default property that you can use without adding it explicitly.</span></span>

> [!NOTE]
> <span data-ttu-id="3a69f-217">Podrobnosti o použití omezení umístění, vlastnosti uzlu a jak se definovat informace naleznete v sekci "Umístění omezení a vlastnosti uzlu" v dokumentu správce prostředků clusteru Service Fabric na [popisující clusteru](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="3a69f-217">For details on the use of placement constraints, node properties, and how to define them, refer to the section "Placement Constraints and Node Properties" in the Service Fabric Cluster Resource Manager Document on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md).</span></span>
> 
> 

### <a name="capacity-metrics"></a><span data-ttu-id="3a69f-218">Metriky kapacity</span><span class="sxs-lookup"><span data-stu-id="3a69f-218">Capacity metrics</span></span>
<span data-ttu-id="3a69f-219">Pro každý typ uzlu můžete přidat vlastní kapacity metriky, které chcete použít v aplikacích pro zatížení sestavy.</span><span class="sxs-lookup"><span data-stu-id="3a69f-219">For each of the node types, you can add custom capacity metrics that you want to use in your applications to report load.</span></span> <span data-ttu-id="3a69f-220">Podrobnosti o použití metriky kapacity pro zatížení sestavy, získáte v dokumentech správce prostředků clusteru Service Fabric na [popisující vaše clusteru](service-fabric-cluster-resource-manager-cluster-description.md) a [metriky a zatížení](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="3a69f-220">For details on the use of capacity metrics to report load, refer to the Service Fabric Cluster Resource Manager Documents on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md) and [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md).</span></span>

### <a name="fabric-upgrade-settings---health-polices"></a><span data-ttu-id="3a69f-221">Upgrade nastavení prostředků infrastruktury – zásady stavu</span><span class="sxs-lookup"><span data-stu-id="3a69f-221">Fabric upgrade settings - Health polices</span></span>
<span data-ttu-id="3a69f-222">Můžete zadat, že stav vlastní zásady pro upgrade pro fabric.</span><span class="sxs-lookup"><span data-stu-id="3a69f-222">You can specify custom health polices for fabric upgrade.</span></span> <span data-ttu-id="3a69f-223">Pokud nastavíte clusteru na fabric automatické upgrady získat tyto zásady použít pro fáze 1 upgrady automatické prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="3a69f-223">If you have set your cluster to Automatic fabric upgrades, then these policies get applied to the Phase-1 of the automatic fabric upgrades.</span></span>
<span data-ttu-id="3a69f-224">Pokud jste nastavili clusteru pro ruční fabric upgrady, získat tyto zásady použít pokaždé, když vyberete novou verzi spouštění systému ji upgrade pro fabric v clusteru.</span><span class="sxs-lookup"><span data-stu-id="3a69f-224">If you have set your cluster for Manual fabric upgrades, then these policies get applied each time you select a new version triggering the system to kick off the fabric upgrade in your cluster.</span></span> <span data-ttu-id="3a69f-225">Pokud není přepsat zásady, budou použity výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3a69f-225">If you do not override the policies, the defaults are used.</span></span>

<span data-ttu-id="3a69f-226">Můžete určit zásady vlastní stavu nebo zkontrolujte aktuální nastavení v okně "upgrade pro fabric" tak, že vyberete pokročilé nastavení upgradu.</span><span class="sxs-lookup"><span data-stu-id="3a69f-226">You can specify the custom health policies or review the current settings under the "fabric upgrade" blade, by selecting the advanced upgrade settings.</span></span> <span data-ttu-id="3a69f-227">Přečtěte si o tom, jak na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="3a69f-227">Review the following picture on how to.</span></span> 

![Správa vlastní stav zásad][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a><span data-ttu-id="3a69f-229">Přizpůsobení nastavení prostředků infrastruktury pro váš cluster</span><span class="sxs-lookup"><span data-stu-id="3a69f-229">Customize Fabric settings for your cluster</span></span>
<span data-ttu-id="3a69f-230">Odkazovat na [nastavení prostředků infrastruktury clusteru prostředků infrastruktury služby](service-fabric-cluster-fabric-settings.md) na co a jak je můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="3a69f-230">Refer to [service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md) on what and how you can customize them.</span></span>

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a><span data-ttu-id="3a69f-231">Opravy operačního systému na virtuální počítače, které tvoří clusteru</span><span class="sxs-lookup"><span data-stu-id="3a69f-231">OS patches on the VMs that make up the cluster</span></span>
<span data-ttu-id="3a69f-232">Odkazovat na [použití opravy Orchestration](service-fabric-patch-orchestration-application.md) které mohl být nasazen v clusteru orchestrated způsobem, zachovat dostupnost služeb vždy instalaci oprav ze služby Windows Update.</span><span class="sxs-lookup"><span data-stu-id="3a69f-232">Refer to [Patch Orchestration Application](service-fabric-patch-orchestration-application.md) which can be deployed on your cluster to install patches from Windows Update in an orchestrated manner, keeping the services available all the time.</span></span> 

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a><span data-ttu-id="3a69f-233">Upgrade operačního systému na virtuální počítače, které tvoří clusteru</span><span class="sxs-lookup"><span data-stu-id="3a69f-233">OS upgrades on the VMs that make up the cluster</span></span>
<span data-ttu-id="3a69f-234">Pokud upgradujete bitovou kopii operačního systému na virtuální počítače clusteru, je nutné se jeden virtuální počítač najednou.</span><span class="sxs-lookup"><span data-stu-id="3a69f-234">If you must upgrade the OS image on the virtual machines of the cluster, you must do it one VM at a time.</span></span> <span data-ttu-id="3a69f-235">Jste zodpovědní pro tento upgrade – v současné době není žádné automation pro tento.</span><span class="sxs-lookup"><span data-stu-id="3a69f-235">You are responsible for this upgrade--there is currently no automation for this.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a69f-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a69f-236">Next steps</span></span>
* <span data-ttu-id="3a69f-237">Zjistěte, jak přizpůsobit některé [nastavení prostředků infrastruktury clusteru prostředků infrastruktury služby](service-fabric-cluster-fabric-settings.md)</span><span class="sxs-lookup"><span data-stu-id="3a69f-237">Learn how to customize some of the [service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md)</span></span>
* <span data-ttu-id="3a69f-238">Zjistěte, jak [škálování vašeho clusteru a odhlášení](service-fabric-cluster-scale-up-down.md)</span><span class="sxs-lookup"><span data-stu-id="3a69f-238">Learn how to [scale your cluster in and out](service-fabric-cluster-scale-up-down.md)</span></span>
* <span data-ttu-id="3a69f-239">Další informace o [upgradů aplikací](service-fabric-application-upgrade.md)</span><span class="sxs-lookup"><span data-stu-id="3a69f-239">Learn about [application upgrades](service-fabric-application-upgrade.md)</span></span>

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG

---
title: "aaaUpgrade clusteru služby Azure Service Fabric | Microsoft Docs"
description: "Upgradujte hello Service Fabric kód nebo konfigurace používající cluster Service Fabric, včetně nastavení režimu aktualizace clusteru upgrade certifikáty, přidání aplikace porty, provádění oprav operačního systému, a tak dále. Co můžete očekávat, když se provádí upgrade hello?"
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
ms.openlocfilehash: 94ac3833ec0810f79de06ecb50f254028fa90408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a><span data-ttu-id="ecc3a-104">Upgrade clusteru služby Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ecc3a-104">Upgrade an Azure Service Fabric cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ecc3a-105">Azure clusteru</span><span class="sxs-lookup"><span data-stu-id="ecc3a-105">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="ecc3a-106">Samostatné clusteru</span><span class="sxs-lookup"><span data-stu-id="ecc3a-106">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

<span data-ttu-id="ecc3a-107">Pro všechny moderní systém je návrh pro zdokonalovány klíče tooachieving dlouhodobý úspěch vašeho produktu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-107">For any modern system, designing for upgradability is key tooachieving long-term success of your product.</span></span> <span data-ttu-id="ecc3a-108">Cluster služby Azure Service Fabric je na prostředek, který vlastníte, ale je částečně spravováno společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-108">An Azure Service Fabric cluster is a resource that you own, but is partly managed by Microsoft.</span></span> <span data-ttu-id="ecc3a-109">Tento článek popisuje, co je spravováno automaticky a co můžete nakonfigurovat sami.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-109">This article describes what is managed automatically and what you can configure yourself.</span></span>

## <a name="controlling-hello-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="ecc3a-110">Řízení hello verze fabric, která běží na clusteru</span><span class="sxs-lookup"><span data-stu-id="ecc3a-110">Controlling hello fabric version that runs on your Cluster</span></span>
<span data-ttu-id="ecc3a-111">Můžete nastavit architektury clusteru tooreceive automatické upgrady jako vydané společností Microsoft nebo můžete vybrat verzi podporované prostředků infrastruktury, které chcete toobe vašeho clusteru na.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-111">You can set your cluster tooreceive automatic fabric upgrades as they are released by Microsoft or you can select a supported fabric version you want your cluster toobe on.</span></span>

<span data-ttu-id="ecc3a-112">To provedete pomocí nastavení hello "upgradeMode" konfigurace clusteru na hello portálu nebo pomocí Správce prostředků v době vytvoření hello nebo později na clusteru s podporou za provozu</span><span class="sxs-lookup"><span data-stu-id="ecc3a-112">You do this by setting hello "upgradeMode" cluster configuration on hello portal or using Resource Manager at hello time of creation or later on a live cluster</span></span> 

> [!NOTE]
> <span data-ttu-id="ecc3a-113">Ujistěte se, že tookeep cluster vždy používá verzi podporovanou prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-113">Make sure tookeep your cluster running a supported fabric version always.</span></span> <span data-ttu-id="ecc3a-114">Jak a kdy jsme oznamujeme hello vydání nové verze aplikace service fabric, předchozí verze hello je označen pro ukončení podpory po 60 dnů od tohoto dne.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-114">As and when we announce hello release of a new version of service fabric, hello previous version is marked for end of support after a minimum of 60 days from that date.</span></span> <span data-ttu-id="ecc3a-115">Hello hello nové verze není zmínka [na blogu týmu prostředků infrastruktury služby hello](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="ecc3a-115">hello  hello new releases are announced [on hello service fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="ecc3a-116">pak je k dispozici toochoose Hello nová verze.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-116">hello new release is available toochoose then.</span></span> 
> 
> 

<span data-ttu-id="ecc3a-117">14 dnů před toohello vypršení platnosti hello verze vašeho clusteru je spuštěn, událost stavu se vygeneruje, který umísťuje clusteru do stavu upozornění.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-117">14 days prior toohello expiry of hello release your cluster is running, a health event is generated that puts your cluster into a warning health state.</span></span> <span data-ttu-id="ecc3a-118">Hello cluster zůstane ve stavu upozornění, dokud neprovedete upgrade verze tooa podporované fabric.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-118">hello cluster remains in a warning state until you upgrade tooa supported fabric version.</span></span>

### <a name="setting-hello-upgrade-mode-via-portal"></a><span data-ttu-id="ecc3a-119">Nastavení režimu upgradu hello prostřednictvím portálu</span><span class="sxs-lookup"><span data-stu-id="ecc3a-119">Setting hello upgrade mode via portal</span></span>
<span data-ttu-id="ecc3a-120">Hello clusteru tooautomatic nebo ruční můžete nastavit při vytváření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-120">You can set hello cluster tooautomatic or manual when you are creating hello cluster.</span></span>

![Create_Manualmode][Create_Manualmode]

<span data-ttu-id="ecc3a-122">Můžete nastavit hello clusteru tooautomatic nebo ruční v clusteru s podporou za provozu, pomocí hello Správa prostředí.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-122">You can set hello cluster tooautomatic or manual when on a live cluster, using hello manage experience.</span></span> 

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-portal"></a><span data-ttu-id="ecc3a-123">Upgrade tooa novou verzi v clusteru, který je nastavený režim tooManual přes portál.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-123">Upgrading tooa new version on a cluster that is set tooManual mode via portal.</span></span>
<span data-ttu-id="ecc3a-124">tooupgrade tooa je nová verze, stačí toodo vyberte z rozevíracího seznamu hello hello k dispozici verzi a uložit.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-124">tooupgrade tooa new version, all you need toodo is select hello available version from hello dropdown and save.</span></span> <span data-ttu-id="ecc3a-125">upgrade pro Fabric Hello získá spuštěna automaticky.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-125">hello Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="ecc3a-126">Hello zásady stavu clusteru (kombinaci uzlu stavu a stavu hello všechny hello aplikace běžící v clusteru hello) jsou zachovány tooduring hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-126">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered tooduring hello upgrade.</span></span>

<span data-ttu-id="ecc3a-127">Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-127">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ecc3a-128">Projděte dolů tento dokument tooread Další informace o tom tooset tyto zásady vlastní stavu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-128">Scroll down this document tooread more on how tooset those custom health policies.</span></span> 

<span data-ttu-id="ecc3a-129">Po opravě hello problémy, které výsledkem hello vrácení musíte tooinitiate hello upgradu znovu podle následujících hello stejné kroky jako předtím.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-129">Once you have fixed hello issues that resulted in hello rollback, you need tooinitiate hello upgrade again, by following hello same steps as before.</span></span>

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-hello-upgrade-mode-via-a-resource-manager-template"></a><span data-ttu-id="ecc3a-131">Nastavení režimu upgradu hello pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ecc3a-131">Setting hello upgrade mode via a Resource Manager template</span></span>
<span data-ttu-id="ecc3a-132">Přidejte hello definice prostředků Microsoft.ServiceFabric/clusters toohello konfigurace "upgradeMode" a sadu hello "clusterCodeVersion" tooone Dobrý den podporované verze prostředků infrastruktury, jak je uvedeno níže a pak nasaďte šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-132">Add hello "upgradeMode" configuration toohello Microsoft.ServiceFabric/clusters resource definition and set hello "clusterCodeVersion" tooone of hello supported fabric versions as shown below and then deploy hello template.</span></span> <span data-ttu-id="ecc3a-133">jsou platné hodnoty "upgradeMode" Hello "Ruční" nebo "Automatické"</span><span class="sxs-lookup"><span data-stu-id="ecc3a-133">hello valid values for "upgradeMode" are "Manual" or "Automatic"</span></span>

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-tooa-new-version-on-a-cluster-that-is-set-toomanual-mode-via-a-resource-manager-template"></a><span data-ttu-id="ecc3a-135">Upgrade tooa novou verzi v clusteru, který je nastavený režim tooManual pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-135">Upgrading tooa new version on a cluster that is set tooManual mode via a Resource Manager template.</span></span>
<span data-ttu-id="ecc3a-136">Po hello clusteru v ručním režimu tooupgrade tooa novou verzi, změňte hello "clusterCodeVersion" tooa podporovanou verzi a nasadit.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-136">When hello cluster is in Manual mode, tooupgrade tooa new version, change hello "clusterCodeVersion" tooa supported version and deploy it.</span></span> <span data-ttu-id="ecc3a-137">nasazení Hello hello šablony, zejména kopance o upgrade pro Fabric hello získá spuštěna automaticky.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-137">hello deployment of hello template, kicks of hello Fabric upgrade gets kicked off automatically.</span></span> <span data-ttu-id="ecc3a-138">Hello zásady stavu clusteru (kombinaci uzlu stavu a stavu hello všechny hello aplikace běžící v clusteru hello) jsou zachovány tooduring hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-138">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered tooduring hello upgrade.</span></span>

<span data-ttu-id="ecc3a-139">Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-139">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ecc3a-140">Projděte dolů tento dokument tooread Další informace o tom tooset tyto zásady vlastní stavu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-140">Scroll down this document tooread more on how tooset those custom health policies.</span></span> 

<span data-ttu-id="ecc3a-141">Po opravě hello problémy, které výsledkem hello vrácení musíte tooinitiate hello upgradu znovu podle následujících hello stejné kroky jako předtím.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-141">Once you have fixed hello issues that resulted in hello rollback, you need tooinitiate hello upgrade again, by following hello same steps as before.</span></span>

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a><span data-ttu-id="ecc3a-142">Získejte seznam všech dostupných verze pro všechna prostředí pro dané předplatné</span><span class="sxs-lookup"><span data-stu-id="ecc3a-142">Get list of all available version for all environments for a given subscription</span></span>
<span data-ttu-id="ecc3a-143">Spustit hello následující příkaz a měli byste obdržet podobné toothis výstupu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-143">Run hello following command, and you should get an output similar toothis.</span></span>

<span data-ttu-id="ecc3a-144">"supportExpiryUtc" informuje vaší Pokud danou verzi vyprší nebo vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-144">"supportExpiryUtc" tells your when a given release is expiring or has expired.</span></span> <span data-ttu-id="ecc3a-145">Hello nejnovější verze nemá platné datum - má hodnotu "9999-12-31T23:59:59.9999999", což právě znamená, že datum vypršení platnosti hello ještě není nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-145">hello latest release does not have a valid date - it has a value of "9999-12-31T23:59:59.9999999", which just means that hello expiry date is not yet set.</span></span>

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

## <a name="fabric-upgrade-behavior-when-hello-cluster-upgrade-mode-is-automatic"></a><span data-ttu-id="ecc3a-146">Fabric upgradu chování, když hello cluster upgradovat režimu je automatický</span><span class="sxs-lookup"><span data-stu-id="ecc3a-146">Fabric upgrade behavior when hello cluster Upgrade Mode is Automatic</span></span>
<span data-ttu-id="ecc3a-147">Společnost Microsoft udržuje kód hello prostředků infrastruktury a konfigurace, která běží v clusteru služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-147">Microsoft maintains hello fabric code and configuration that runs in an Azure cluster.</span></span> <span data-ttu-id="ecc3a-148">Nemůžeme provést automatické upgrady monitorovaných toohello softwaru podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-148">We perform automatic monitored upgrades toohello software on an as-needed basis.</span></span> <span data-ttu-id="ecc3a-149">Tyto upgrady může být kódu, konfigurace nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-149">These upgrades could be code, configuration, or both.</span></span> <span data-ttu-id="ecc3a-150">toomake se, že vaše aplikace vyskytne žádný dopad nebo minimálním dopadem kvůli toothese upgrady, proveďte jsme hello upgrady v hello následující fáze:</span><span class="sxs-lookup"><span data-stu-id="ecc3a-150">toomake sure that your application suffers no impact or minimal impact due toothese upgrades, we perform hello upgrades in hello following phases:</span></span>

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a><span data-ttu-id="ecc3a-151">Fáze 1: Upgrade se provádí pomocí všechny zásady stavu clusteru</span><span class="sxs-lookup"><span data-stu-id="ecc3a-151">Phase 1: An upgrade is performed by using all cluster health policies</span></span>
<span data-ttu-id="ecc3a-152">V průběhu této fáze upgradu hello pokračovat jednu upgradovací doménu najednou a hello aplikace, které byly spuštěny v clusteru hello pokračovat toorun bez odstávky.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-152">During this phase, hello upgrades proceed one upgrade domain at a time, and hello applications that were running in hello cluster continue toorun without any downtime.</span></span> <span data-ttu-id="ecc3a-153">Hello zásady stavu clusteru (kombinaci uzlu stavu a stavu hello všechny hello aplikace běžící v clusteru hello) jsou zachovány tooduring hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-153">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered tooduring hello upgrade.</span></span>

<span data-ttu-id="ecc3a-154">Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-154">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ecc3a-155">E-mailu se pak odešlou toohello vlastníka předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-155">Then an email is sent toohello owner of hello subscription.</span></span> <span data-ttu-id="ecc3a-156">e-mailu Hello obsahuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="ecc3a-156">hello email contains hello following information:</span></span>

* <span data-ttu-id="ecc3a-157">Oznámení, že jsme měli tooroll zpět na upgrade clusteru.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-157">Notification that we had tooroll back a cluster upgrade.</span></span>
* <span data-ttu-id="ecc3a-158">Navrhované nápravné akce, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-158">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="ecc3a-159">Hello počet dní (ne), dokud jsme provést fáze 2.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-159">hello number of days (n) until we execute Phase 2.</span></span>

<span data-ttu-id="ecc3a-160">Pokusíme tooexecute hello stejný upgrade několik vícekrát v případě, že jakéhokoli upgradu se nezdařilo z důvodů infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-160">We try tooexecute hello same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="ecc3a-161">Po hello byla odeslána n dní od hello datum hello e-mailu, budeme pokračovat tooPhase 2.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-161">After hello n days from hello date hello email was sent, we proceed tooPhase 2.</span></span>

<span data-ttu-id="ecc3a-162">Pokud jsou splněny zásady stavu hello clusteru, je hello upgrade považuje za úspěšnou a označeny jako dokončené.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-162">If hello cluster health policies are met, hello upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="ecc3a-163">Může dojít během upgradu počáteční hello ani v žádné z opakování upgradu hello v této fázi.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-163">This can happen during hello initial upgrade or any of hello upgrade reruns in this phase.</span></span> <span data-ttu-id="ecc3a-164">Neexistuje žádné potvrzení e-mailu úspěšně spustit.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-164">There is no email confirmation of a successful run.</span></span> <span data-ttu-id="ecc3a-165">Toto je tooavoid odesílání, že jste příliš mnoho e-mailů – příjem e-mailu měla by se zobrazit jako toonormal k výjimce.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-165">This is tooavoid sending you too many emails--receiving an email should be seen as an exception toonormal.</span></span> <span data-ttu-id="ecc3a-166">Očekáváme, že většina toosucceed upgrady hello clusteru bez dopadu na dostupnost vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-166">We expect most of hello cluster upgrades toosucceed without impacting your application availability.</span></span>

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a><span data-ttu-id="ecc3a-167">Fáze 2: Upgrade se provádí pomocí výchozího stavu pouze zásady</span><span class="sxs-lookup"><span data-stu-id="ecc3a-167">Phase 2: An upgrade is performed by using default health policies only</span></span>
<span data-ttu-id="ecc3a-168">Hello stavu v této fázi jsou zásady nastavené tak, aby zůstala hello počet aplikací, které byly od začátku hello hello upgradu v pořádku pro stejný hello dobu trvání procesu upgradu hello hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-168">hello health policies in this phase are set in such a way that hello number of applications that were healthy at hello beginning of hello upgrade remains hello same for hello duration of hello upgrade process.</span></span> <span data-ttu-id="ecc3a-169">Jako fáze 1 hello fáze 2 upgrady pokračovat jednu upgradovací doménu najednou a hello aplikace, které byly spuštěny v clusteru hello pokračovat toorun bez odstávky.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-169">As in Phase 1, hello Phase 2 upgrades proceed one upgrade domain at a time, and hello applications that were running in hello cluster continue toorun without any downtime.</span></span> <span data-ttu-id="ecc3a-170">zásady stavu clusteru Hello (kombinaci uzlu stavu a stavu hello všechny hello aplikace běžící v clusteru hello) jsou nalepeného toofor hello trvání upgradu hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-170">hello cluster health policies (a combination of node health and hello health all hello applications running in hello cluster) are adhered toofor hello duration of hello upgrade.</span></span>

<span data-ttu-id="ecc3a-171">Pokud platí zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-171">If hello cluster health policies in effect are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ecc3a-172">E-mailu se pak odešlou toohello vlastníka předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-172">Then an email is sent toohello owner of hello subscription.</span></span> <span data-ttu-id="ecc3a-173">e-mailu Hello obsahuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="ecc3a-173">hello email contains hello following information:</span></span>

* <span data-ttu-id="ecc3a-174">Oznámení, že jsme měli tooroll zpět na upgrade clusteru.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-174">Notification that we had tooroll back a cluster upgrade.</span></span>
* <span data-ttu-id="ecc3a-175">Navrhované nápravné akce, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-175">Suggested remedial actions, if any.</span></span>
* <span data-ttu-id="ecc3a-176">Hello počet dní (ne), dokud jsme provést fáze 3.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-176">hello number of days (n) until we execute Phase 3.</span></span>

<span data-ttu-id="ecc3a-177">Pokusíme tooexecute hello stejný upgrade několik vícekrát v případě, že jakéhokoli upgradu se nezdařilo z důvodů infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-177">We try tooexecute hello same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="ecc3a-178">Připomenutí e-mail je odeslán během několika dní, než n dní jsou.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-178">A reminder email is sent a couple of days before n days are up.</span></span> <span data-ttu-id="ecc3a-179">Po hello byla odeslána n dní od hello datum hello e-mailu, budeme pokračovat tooPhase 3.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-179">After hello n days from hello date hello email was sent, we proceed tooPhase 3.</span></span> <span data-ttu-id="ecc3a-180">Hello e-mailů, které vám můžeme poslat v fáze 2 bude nutno vážně a musí být přijata nápravné akce.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-180">hello emails we send you in Phase 2 must be taken seriously and remedial actions must be taken.</span></span>

<span data-ttu-id="ecc3a-181">Pokud jsou splněny zásady stavu hello clusteru, je hello upgrade považuje za úspěšnou a označeny jako dokončené.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-181">If hello cluster health policies are met, hello upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="ecc3a-182">Může dojít během upgradu počáteční hello ani v žádné z opakování upgradu hello v této fázi.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-182">This can happen during hello initial upgrade or any of hello upgrade reruns in this phase.</span></span> <span data-ttu-id="ecc3a-183">Neexistuje žádné potvrzení e-mailu úspěšně spustit.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-183">There is no email confirmation of a successful run.</span></span>

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a><span data-ttu-id="ecc3a-184">Fáze 3: Upgrade se provádí na základě zásad agresivní stavu</span><span class="sxs-lookup"><span data-stu-id="ecc3a-184">Phase 3: An upgrade is performed by using aggressive health policies</span></span>
<span data-ttu-id="ecc3a-185">Tyto zásady stavu v této fázi jsou s ohledem na dokončení upgradu hello spíše než hello stav hello aplikací.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-185">These health policies in this phase are geared towards completion of hello upgrade rather than hello health of hello applications.</span></span> <span data-ttu-id="ecc3a-186">V této fázi skončili jen několik upgradů clusteru.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-186">Very few cluster upgrades end up in this phase.</span></span> <span data-ttu-id="ecc3a-187">Pokud váš cluster získá toothis fáze, je velmi pravděpodobné, že vaše aplikace se změní na není v pořádku nebo ztrátu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-187">If your cluster gets toothis phase, there is a good chance that your application becomes unhealthy and/or lose availability.</span></span>

<span data-ttu-id="ecc3a-188">Podobně jako toohello další dvě fáze 3 fáze upgradu pokračovat jednu upgradovací doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-188">Similar toohello other two phases, Phase 3 upgrades proceed one upgrade domain at a time.</span></span>

<span data-ttu-id="ecc3a-189">Pokud zásady stavu hello clusteru nejsou splněny, hello upgrade vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-189">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="ecc3a-190">Pokusíme tooexecute hello stejný upgrade několik vícekrát v případě, že jakéhokoli upgradu se nezdařilo z důvodů infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-190">We try tooexecute hello same upgrade a few more times in case any upgrades failed for infrastructure reasons.</span></span> <span data-ttu-id="ecc3a-191">Potom je ukotvena hello clusteru, tak, aby se už nebude podporu nebo upgrady.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-191">After that, hello cluster is pinned, so that it will no longer receive support and/or upgrades.</span></span>

<span data-ttu-id="ecc3a-192">Vlastník předplatného toohello, společně s hello nápravné akce se odeslal e-mail s tyto informace.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-192">An email with this information is sent toohello subscription owner, along with hello remedial actions.</span></span> <span data-ttu-id="ecc3a-193">Není Očekáváme, že všechny clustery tooget do stavu, kde se nezdařilo fáze 3.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-193">We do not expect any clusters tooget into a state where Phase 3 has failed.</span></span>

<span data-ttu-id="ecc3a-194">Pokud jsou splněny zásady stavu hello clusteru, je hello upgrade považuje za úspěšnou a označeny jako dokončené.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-194">If hello cluster health policies are met, hello upgrade is considered successful and marked complete.</span></span> <span data-ttu-id="ecc3a-195">Může dojít během upgradu počáteční hello ani v žádné z opakování upgradu hello v této fázi.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-195">This can happen during hello initial upgrade or any of hello upgrade reruns in this phase.</span></span> <span data-ttu-id="ecc3a-196">Neexistuje žádné potvrzení e-mailu úspěšně spustit.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-196">There is no email confirmation of a successful run.</span></span>

## <a name="cluster-configurations-that-you-control"></a><span data-ttu-id="ecc3a-197">Konfigurace clusteru, které řídíte</span><span class="sxs-lookup"><span data-stu-id="ecc3a-197">Cluster configurations that you control</span></span>
<span data-ttu-id="ecc3a-198">Kromě toho toohello možnost tooset hello cluster upgradovat režimu, tady jsou hello konfigurace, které můžete změnit na cluster za provozu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-198">In addition toohello ability tooset hello cluster upgrade mode, Here are hello configurations that you can change on a live cluster.</span></span>

### <a name="certificates"></a><span data-ttu-id="ecc3a-199">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="ecc3a-199">Certificates</span></span>
<span data-ttu-id="ecc3a-200">Můžete přidat nové nebo snadno odstranit certifikáty pro hello clusteru a klientů přes portál hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-200">You can add new or delete certificates for hello cluster and client via hello portal easily.</span></span> <span data-ttu-id="ecc3a-201">Odkazovat příliš[tento dokument podrobné pokyny](service-fabric-cluster-security-update-certs-azure.md)</span><span class="sxs-lookup"><span data-stu-id="ecc3a-201">Refer too[this document for detailed instructions](service-fabric-cluster-security-update-certs-azure.md)</span></span>

![Snímek obrazovky, který zobrazuje kryptografické otisky certifikátu v hello portálu Azure.][CertificateUpgrade]

### <a name="application-ports"></a><span data-ttu-id="ecc3a-203">Porty aplikace</span><span class="sxs-lookup"><span data-stu-id="ecc3a-203">Application ports</span></span>
<span data-ttu-id="ecc3a-204">Porty aplikace můžete změnit změnou hello nástroj pro vyrovnávání zatížení prostředků vlastnosti, které jsou přidružené typ uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-204">You can change application ports by changing hello Load Balancer resource properties that are associated with hello node type.</span></span> <span data-ttu-id="ecc3a-205">Můžete použít portál hello nebo můžete použít Správce prostředků PowerShell přímo.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-205">You can use hello portal, or you can use Resource Manager PowerShell directly.</span></span>

<span data-ttu-id="ecc3a-206">tooopen nový port na všech virtuálních počítačích v uzlu typu hello následující:</span><span class="sxs-lookup"><span data-stu-id="ecc3a-206">tooopen a new port on all VMs in a node type, do hello following:</span></span>

1. <span data-ttu-id="ecc3a-207">Přidejte nového testu toohello odpovídající služba Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-207">Add a new probe toohello appropriate load balancer.</span></span>
   
    <span data-ttu-id="ecc3a-208">Pokud jste nasadili cluster pomocí portálu hello, jsou nástroje pro vyrovnávání zatížení hello s názvem "LB-název skupiny prostředků hello-NodeTypename", jednu pro každý typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-208">If you deployed your cluster by using hello portal, hello load balancers are named "LB-name of hello Resource group-NodeTypename", one for each node type.</span></span> <span data-ttu-id="ecc3a-209">Vzhledem k tomu, že jsou názvy nástroje pro vyrovnávání zatížení hello pouze v rámci skupiny prostředků jedinečný, je nejlepší Pokud vyhledávání v rámci určité skupiny zdrojů.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-209">Since hello load balancer names are unique only within a resource group, it is best if you search for them under a specific resource group.</span></span>
   
    ![Snímek obrazovky zobrazující přidání tooa testu vyrovnávání zátěže hello portálu.][AddingProbes]
2. <span data-ttu-id="ecc3a-211">Přidáte nové pravidlo Vyrovnávání zatížení toohello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-211">Add a new rule toohello load balancer.</span></span>
   
    <span data-ttu-id="ecc3a-212">Přidáte nové pravidlo toohello stejný nástroj pro vyrovnávání zatížení pomocí hello kontrolu, kterou jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-212">Add a new rule toohello same load balancer by using hello probe that you created in hello previous step.</span></span>
   
    ![Přidání nové pravidlo Vyrovnávání zatížení tooa hello portálu.][AddingLBRules]

### <a name="placement-properties"></a><span data-ttu-id="ecc3a-214">Vlastnosti umístění</span><span class="sxs-lookup"><span data-stu-id="ecc3a-214">Placement properties</span></span>
<span data-ttu-id="ecc3a-215">Pro každý typ uzlu hello můžete přidat vlastní umístění vlastnosti chcete toouse ve svých aplikacích.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-215">For each of hello node types, you can add custom placement properties that you want toouse in your applications.</span></span> <span data-ttu-id="ecc3a-216">Typ uzlu je ve výchozím nastavení, kterou můžete použít bez přidání explicitně.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-216">NodeType is a default property that you can use without adding it explicitly.</span></span>

> [!NOTE]
> <span data-ttu-id="ecc3a-217">Podrobnosti o použití hello omezení umístění, vlastnosti uzlu, a jak toodefine, najdete v části toohello "Omezení umístění a vlastnosti uzlu" v hello dokumentu správce prostředků clusteru Service Fabric na [popisující vaše clusteru ](service-fabric-cluster-resource-manager-cluster-description.md).</span><span class="sxs-lookup"><span data-stu-id="ecc3a-217">For details on hello use of placement constraints, node properties, and how toodefine them, refer toohello section "Placement Constraints and Node Properties" in hello Service Fabric Cluster Resource Manager Document on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md).</span></span>
> 
> 

### <a name="capacity-metrics"></a><span data-ttu-id="ecc3a-218">Metriky kapacity</span><span class="sxs-lookup"><span data-stu-id="ecc3a-218">Capacity metrics</span></span>
<span data-ttu-id="ecc3a-219">Pro každý typ uzlu hello můžete přidat metriky vlastní kapacity chcete toouse v tooreport zatížení aplikací.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-219">For each of hello node types, you can add custom capacity metrics that you want toouse in your applications tooreport load.</span></span> <span data-ttu-id="ecc3a-220">Podrobnosti o použití hello zatížení tooreport metriky kapacity naleznete toohello dokumenty správce prostředků clusteru Service Fabric v [popisující vaše clusteru](service-fabric-cluster-resource-manager-cluster-description.md) a [metriky a zatížení](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="ecc3a-220">For details on hello use of capacity metrics tooreport load, refer toohello Service Fabric Cluster Resource Manager Documents on [Describing Your Cluster](service-fabric-cluster-resource-manager-cluster-description.md) and [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md).</span></span>

### <a name="fabric-upgrade-settings---health-polices"></a><span data-ttu-id="ecc3a-221">Upgrade nastavení prostředků infrastruktury – zásady stavu</span><span class="sxs-lookup"><span data-stu-id="ecc3a-221">Fabric upgrade settings - Health polices</span></span>
<span data-ttu-id="ecc3a-222">Můžete zadat, že stav vlastní zásady pro upgrade pro fabric.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-222">You can specify custom health polices for fabric upgrade.</span></span> <span data-ttu-id="ecc3a-223">Pokud jste nastavili vašeho clusteru tooAutomatic upgrady prostředků infrastruktury, získat tyto zásady použité toohello fáze 1 upgradu hello automatické prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-223">If you have set your cluster tooAutomatic fabric upgrades, then these policies get applied toohello Phase-1 of hello automatic fabric upgrades.</span></span>
<span data-ttu-id="ecc3a-224">Pokud jste nastavili clusteru pro ruční fabric upgrady, pak tyto zásady použije pokaždé, když vyberete novou verzi aktivován hello systému tookick vypnout upgrade pro fabric hello v clusteru.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-224">If you have set your cluster for Manual fabric upgrades, then these policies get applied each time you select a new version triggering hello system tookick off hello fabric upgrade in your cluster.</span></span> <span data-ttu-id="ecc3a-225">Pokud není přepsat hello zásady, budou použity výchozí hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-225">If you do not override hello policies, hello defaults are used.</span></span>

<span data-ttu-id="ecc3a-226">Můžete určit zásady vlastní stavu hello nebo zkontrolujte hello aktuální nastavení v okně "upgrade pro fabric" hello, výběrem hello Upřesnit nastavení upgradu.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-226">You can specify hello custom health policies or review hello current settings under hello "fabric upgrade" blade, by selecting hello advanced upgrade settings.</span></span> <span data-ttu-id="ecc3a-227">Zkontrolujte následující obrázek o tom, jak hello.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-227">Review hello following picture on how to.</span></span> 

![Správa vlastní stav zásad][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a><span data-ttu-id="ecc3a-229">Přizpůsobení nastavení prostředků infrastruktury pro váš cluster</span><span class="sxs-lookup"><span data-stu-id="ecc3a-229">Customize Fabric settings for your cluster</span></span>
<span data-ttu-id="ecc3a-230">Odkazovat příliš[nastavení prostředků infrastruktury clusteru prostředků infrastruktury služby](service-fabric-cluster-fabric-settings.md) na co a jak je můžete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-230">Refer too[service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md) on what and how you can customize them.</span></span>

### <a name="os-patches-on-hello-vms-that-make-up-hello-cluster"></a><span data-ttu-id="ecc3a-231">Opravy operačního systému na hello virtuálních počítačů, které tvoří hello clusteru</span><span class="sxs-lookup"><span data-stu-id="ecc3a-231">OS patches on hello VMs that make up hello cluster</span></span>
<span data-ttu-id="ecc3a-232">Odkazovat příliš[použití opravy Orchestration](service-fabric-patch-orchestration-application.md) které by mohl být nasazen na váš clusteru tooinstall opravy ze služby Windows Update orchestrated způsobem, sledovat dostupnost služeb hello všechny hello čas.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-232">Refer too[Patch Orchestration Application](service-fabric-patch-orchestration-application.md) which can be deployed on your cluster tooinstall patches from Windows Update in an orchestrated manner, keeping hello services available all hello time.</span></span> 

### <a name="os-upgrades-on-hello-vms-that-make-up-hello-cluster"></a><span data-ttu-id="ecc3a-233">Upgrade operačního systému na hello virtuálních počítačů, které tvoří hello clusteru</span><span class="sxs-lookup"><span data-stu-id="ecc3a-233">OS upgrades on hello VMs that make up hello cluster</span></span>
<span data-ttu-id="ecc3a-234">Pokud musíte provést upgrade hello bitové kopie operačního systému na virtuálních počítačích hello hello clusteru, je nutné ji jeden virtuální počítač najednou.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-234">If you must upgrade hello OS image on hello virtual machines of hello cluster, you must do it one VM at a time.</span></span> <span data-ttu-id="ecc3a-235">Jste zodpovědní pro tento upgrade – v současné době není žádné automation pro tento.</span><span class="sxs-lookup"><span data-stu-id="ecc3a-235">You are responsible for this upgrade--there is currently no automation for this.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecc3a-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecc3a-236">Next steps</span></span>
* <span data-ttu-id="ecc3a-237">Zjistěte, jak toocustomize některé hello [nastavení prostředků infrastruktury clusteru prostředků infrastruktury služby](service-fabric-cluster-fabric-settings.md)</span><span class="sxs-lookup"><span data-stu-id="ecc3a-237">Learn how toocustomize some of hello [service fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md)</span></span>
* <span data-ttu-id="ecc3a-238">Zjistěte, jak příliš[škálování vašeho clusteru a odhlášení](service-fabric-cluster-scale-up-down.md)</span><span class="sxs-lookup"><span data-stu-id="ecc3a-238">Learn how too[scale your cluster in and out](service-fabric-cluster-scale-up-down.md)</span></span>
* <span data-ttu-id="ecc3a-239">Další informace o [upgradů aplikací](service-fabric-application-upgrade.md)</span><span class="sxs-lookup"><span data-stu-id="ecc3a-239">Learn about [application upgrades](service-fabric-application-upgrade.md)</span></span>

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG

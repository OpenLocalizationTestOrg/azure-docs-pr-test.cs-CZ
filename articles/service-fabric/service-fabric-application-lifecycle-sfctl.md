---
title: "aplikace Azure Service Fabric aaaManage pomocí příkazového řádku Azure Service Fabric"
description: "Zjistěte, jak toodeploy a odebrat aplikace ze Azure Service Fabric clusteru pomocí příkazového řádku Azure Service Fabric"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: d9f98cee1d70f71a2aab68ff556956619910e4fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a><span data-ttu-id="07447-103">Spravovat aplikace Azure Service Fabric pomocí příkazového řádku Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="07447-103">Manage an Azure Service Fabric application by using Azure Service Fabric CLI</span></span>

<span data-ttu-id="07447-104">Zjistěte, jak toocreate a odstranění aplikace, které jsou spuštěny v clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="07447-104">Learn how toocreate and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07447-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07447-105">Prerequisites</span></span>

* <span data-ttu-id="07447-106">Nainstalujte Service Fabric rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="07447-106">Install Service Fabric CLI.</span></span> <span data-ttu-id="07447-107">Pak vyberte cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="07447-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="07447-108">Další informace najdete v tématu [začít pracovat s Service Fabric rozhraní příkazového řádku](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="07447-108">For more information, see [Get started with Service Fabric CLI](service-fabric-cli.md).</span></span>

* <span data-ttu-id="07447-109">Máte Service Fabric aplikace balíčku připravené toobe nasazení.</span><span class="sxs-lookup"><span data-stu-id="07447-109">Have a Service Fabric application package ready toobe deployed.</span></span> <span data-ttu-id="07447-110">Další informace o tom, tooauthor a balíček aplikace, přečtěte si informace o hello [model aplikace Service Fabric](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="07447-110">For more information about how tooauthor and package an application, read about hello [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="07447-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="07447-111">Overview</span></span>

<span data-ttu-id="07447-112">toodeploy novou aplikaci, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="07447-112">toodeploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="07447-113">Nahrajte úložišti aplikace Service Fabric toohello balíček bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="07447-113">Upload an application package toohello Service Fabric image store.</span></span>
2. <span data-ttu-id="07447-114">Zřídit typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="07447-114">Provision an application type.</span></span>
3. <span data-ttu-id="07447-115">Zadejte a vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="07447-115">Specify and create an application.</span></span>
4. <span data-ttu-id="07447-116">Zadejte a vytvoření služeb.</span><span class="sxs-lookup"><span data-stu-id="07447-116">Specify and create services.</span></span>

<span data-ttu-id="07447-117">tooremove existující aplikaci, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="07447-117">tooremove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="07447-118">Odstraňte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="07447-118">Delete hello application.</span></span>
2. <span data-ttu-id="07447-119">Unprovision hello související typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="07447-119">Unprovision hello associated application type.</span></span>
3. <span data-ttu-id="07447-120">Odstraňte hello obsahu úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="07447-120">Delete hello image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="07447-121">Nasazení nové aplikace</span><span class="sxs-lookup"><span data-stu-id="07447-121">Deploy a new application</span></span>

<span data-ttu-id="07447-122">toodeploy novou aplikaci, dokončení hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="07447-122">toodeploy a new application, complete hello following tasks:</span></span>

### <a name="upload-a-new-application-package-toohello-image-store"></a><span data-ttu-id="07447-123">Nahrát nové úložiště bitové kopie balíčku toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="07447-123">Upload a new application package toohello image store</span></span>

<span data-ttu-id="07447-124">Předtím, než vytvoříte aplikaci, nahrajte balíček aplikace hello, toohello úložiště bitových kopií Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="07447-124">Before you create an application, upload hello application package toohello Service Fabric image store.</span></span>

<span data-ttu-id="07447-125">Například, pokud je balíčku aplikace v hello `app_package_dir` directory hello použijte následující příkazy tooupload hello adresáře:</span><span class="sxs-lookup"><span data-stu-id="07447-125">For example, if your application package is in hello `app_package_dir` directory, use hello following commands tooupload hello directory:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir
```

<span data-ttu-id="07447-126">U velkých balíčků aplikací, můžete zadat hello `--show-progress` možnost toodisplay hello průběh nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="07447-126">For large application packages, you can specify hello `--show-progress` option toodisplay hello progress of hello upload.</span></span>

### <a name="provision-hello-application-type"></a><span data-ttu-id="07447-127">Typ aplikace hello zřizování</span><span class="sxs-lookup"><span data-stu-id="07447-127">Provision hello application type</span></span>

<span data-ttu-id="07447-128">Po dokončení nahrávání hello zřídit hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="07447-128">When hello upload is finished, provision hello application.</span></span> <span data-ttu-id="07447-129">tooprovision hello aplikace hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07447-129">tooprovision hello application, use hello following command:</span></span>

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="07447-130">Hello hodnota `application-type-build-path` je název hello hello adresáře, kde jste nahráli balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="07447-130">hello value for `application-type-build-path` is hello name of hello directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="07447-131">Vytvoření aplikace z typ aplikace</span><span class="sxs-lookup"><span data-stu-id="07447-131">Create an application from an application type</span></span>

<span data-ttu-id="07447-132">Po zřízení hello aplikace, použijte následující příkaz tooname hello a vytvoření aplikace:</span><span class="sxs-lookup"><span data-stu-id="07447-132">After you provision hello application, use hello following command tooname and create your application:</span></span>

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="07447-133">`app-name`je název hello má toouse pro instanci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="07447-133">`app-name` is hello name that you want toouse for hello application instance.</span></span> <span data-ttu-id="07447-134">Manifest dříve zřízené aplikace můžete získat další parametry.</span><span class="sxs-lookup"><span data-stu-id="07447-134">You can get additional parameters from the previously provisioned application manifest.</span></span>

<span data-ttu-id="07447-135">název aplikace Hello musí začínat hello předponu `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="07447-135">hello application name must start with hello prefix `fabric:/`.</span></span>

### <a name="create-services-for-hello-new-application"></a><span data-ttu-id="07447-136">Vytvoření služby pro novou aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="07447-136">Create services for hello new application</span></span>

<span data-ttu-id="07447-137">Po vytvoření aplikace služby vytvořte z aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="07447-137">After you have created an application, create services from hello application.</span></span> <span data-ttu-id="07447-138">V následujícím příkladu hello vytvoříme nové bezstavové služby z naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="07447-138">In hello following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="07447-139">Hello služby, které můžete vytvořit z aplikace jsou definovány v service manifest v balíčku dříve zřízené aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="07447-139">hello services that you can create from an application are defined in a service manifest in hello previously provisioned application package.</span></span>

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="07447-140">Ověřte stav a nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="07447-140">Verify application deployment and health</span></span>

<span data-ttu-id="07447-141">tooverify všechno, co je v pořádku, použijte následující příkazy stavu hello:</span><span class="sxs-lookup"><span data-stu-id="07447-141">tooverify everything is healthy, use hello following health commands:</span></span>

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

<span data-ttu-id="07447-142">tooverify, že služba hello je v pořádku, použijte podobné příkazy tooretrieve hello stavu hello služby a aplikace:</span><span class="sxs-lookup"><span data-stu-id="07447-142">tooverify that hello service is healthy, use similar commands tooretrieve hello health of both hello service and the application:</span></span>

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

<span data-ttu-id="07447-143">V pořádku služeb a aplikací mít `HealthState` hodnotu `Ok`.</span><span class="sxs-lookup"><span data-stu-id="07447-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="07447-144">Odeberte stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="07447-144">Remove an existing application</span></span>

<span data-ttu-id="07447-145">tooremove aplikace, dokončení hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="07447-145">tooremove an application, complete hello following tasks:</span></span>

### <a name="delete-hello-application"></a><span data-ttu-id="07447-146">Odstranit aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="07447-146">Delete hello application</span></span>

<span data-ttu-id="07447-147">toodelete hello aplikace hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07447-147">toodelete hello application, use hello following command:</span></span>

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a><span data-ttu-id="07447-148">Zrušit zřízení typu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="07447-148">Unprovision hello application type</span></span>

<span data-ttu-id="07447-149">Po odstranění aplikace hello, můžete zrušit zřízení typu aplikace hello již nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="07447-149">After you delete hello application, you can unprovision hello application type if you no longer need it.</span></span> <span data-ttu-id="07447-150">toounprovision hello typ aplikace, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07447-150">toounprovision hello application type, use hello following command:</span></span>

```azurecli
sfctl application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="07447-151">Hello zadejte název a typ verze musí odpovídat hello názvem a verzí v manifestu dříve zřízené aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="07447-151">hello type name and type version must match hello name and version in hello previously provisioned application manifest.</span></span>

### <a name="delete-hello-application-package"></a><span data-ttu-id="07447-152">Odstranění balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="07447-152">Delete hello application package</span></span>

<span data-ttu-id="07447-153">Po mít zrušil zajišťování hello typu aplikace, můžete odstranit balíček aplikace hello z úložiště bitových kopií hello již nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="07447-153">After you have unprovisioned hello application type, you can delete hello application package from hello image store if you no longer need it.</span></span> <span data-ttu-id="07447-154">Odstraňování balíčky aplikací pomáhá uvolnění místa na disku.</span><span class="sxs-lookup"><span data-stu-id="07447-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="07447-155">balíček aplikace hello toodelete z úložiště bitových kopií hello, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07447-155">toodelete hello application package from hello image store, use hello following command:</span></span>

```azurecli
sfctl store delete --content-path app_package_dir
```

<span data-ttu-id="07447-156">`content-path`musí být název hello hello adresáře, který jste nahráli při vytváření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="07447-156">`content-path` must be hello name of hello directory that you uploaded when you created hello application.</span></span>

## <a name="upgrade-application"></a><span data-ttu-id="07447-157">Upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="07447-157">Upgrade application</span></span>

<span data-ttu-id="07447-158">Po vytvoření aplikace, můžete opakovat hello stejnou sadu kroků tooprovision druhou verzi vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="07447-158">After creating your application, you can repeat hello same set of steps tooprovision a second version of your application.</span></span> <span data-ttu-id="07447-159">Potom se upgradu aplikace Service Fabric můžete přejít toorunning hello druhá verze aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="07447-159">Then, with a Service Fabric application upgrade you can transition toorunning hello second version of hello application.</span></span> <span data-ttu-id="07447-160">Další informace naleznete v dokumentaci k hello na [upgradů aplikací Service Fabric](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="07447-160">For more information, see hello documentation on [Service Fabric application upgrades](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="07447-161">tooperform upgradu první zřídit hello příští verze aplikace pomocí hello hello stejné příkazy jako dříve:</span><span class="sxs-lookup"><span data-stu-id="07447-161">tooperform an upgrade, first provision hello next version of hello application using hello same commands as before:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
```

<span data-ttu-id="07447-162">Doporučuje se pak tooperform monitorovaných automatický upgrade, spusťte hello upgrade spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07447-162">It is recommended then tooperform a monitored automatic upgrade, launch hello upgrade by running hello following command:</span></span>

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

<span data-ttu-id="07447-163">Upgrady přepsat existující parametry s jakoukoli sadu je zadán.</span><span class="sxs-lookup"><span data-stu-id="07447-163">Upgrades override existing parameters with whatever set is specified.</span></span> <span data-ttu-id="07447-164">Parametry aplikačního mají být předány jako argumenty toohello příkaz pro upgrade, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="07447-164">Application parameters should be passed as arguments toohello upgrade command, if necessary.</span></span> <span data-ttu-id="07447-165">Parametry aplikačního by měl být zakódován jako objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="07447-165">Application parameters should be encoded as a JSON object.</span></span>

<span data-ttu-id="07447-166">tooretrieve žádné parametry dříve zadán, můžete použít hello `sfctl application info` příkaz.</span><span class="sxs-lookup"><span data-stu-id="07447-166">tooretrieve any parameters previously specified, you can use hello `sfctl application info` command.</span></span>

<span data-ttu-id="07447-167">Po upgradu aplikace v průběhu hello stav se dá načíst pomocí `sfctl application upgrade-status` příkaz.</span><span class="sxs-lookup"><span data-stu-id="07447-167">When an application upgrade is in progress, hello status can be retrieved using the `sfctl application upgrade-status` command.</span></span>

<span data-ttu-id="07447-168">Navíc pokud upgradu v průběhu a je třeba toobe zrušen, můžete použít hello `sfctl application upgrade-rollback` tooroll zpět hello upgradu.</span><span class="sxs-lookup"><span data-stu-id="07447-168">Finally, if an upgrade is in progress and needs toobe canceled, you can use hello `sfctl application upgrade-rollback` tooroll back hello upgrade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07447-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07447-169">Next steps</span></span>

* [<span data-ttu-id="07447-170">Základy služby Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="07447-170">Service Fabric CLI basics</span></span>](service-fabric-cli.md)
* [<span data-ttu-id="07447-171">Začínáme se službou Service Fabric v systému Linux</span><span class="sxs-lookup"><span data-stu-id="07447-171">Getting started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="07447-172">Spuštění upgradu aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="07447-172">Launching a Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

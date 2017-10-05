---
title: "Spravovat aplikace Azure Service Fabric pomocí příkazového řádku Azure Service Fabric"
description: "Zjistěte, jak nasadit a odebrání aplikace z clusteru služby Azure Service Fabric pomocí příkazového řádku Azure Service Fabric"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: c3a2eb3e6e54f952ef963bb2a0292d9ad7b53bc5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a><span data-ttu-id="7131c-103">Spravovat aplikace Azure Service Fabric pomocí příkazového řádku Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="7131c-103">Manage an Azure Service Fabric application by using Azure Service Fabric CLI</span></span>

<span data-ttu-id="7131c-104">Naučte se vytvářet a odstraňovat aplikace, které jsou spuštěny v clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="7131c-104">Learn how to create and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7131c-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7131c-105">Prerequisites</span></span>

* <span data-ttu-id="7131c-106">Nainstalujte Service Fabric rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="7131c-106">Install Service Fabric CLI.</span></span> <span data-ttu-id="7131c-107">Pak vyberte cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="7131c-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="7131c-108">Další informace najdete v tématu [začít pracovat s Service Fabric rozhraní příkazového řádku](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7131c-108">For more information, see [Get started with Service Fabric CLI](service-fabric-cli.md).</span></span>

* <span data-ttu-id="7131c-109">Máte připravený k nasazení balíčku aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="7131c-109">Have a Service Fabric application package ready to be deployed.</span></span> <span data-ttu-id="7131c-110">Další informace o tom, jak vytvořit a balíček aplikace, přečtěte si informace o [model aplikace Service Fabric](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="7131c-110">For more information about how to author and package an application, read about the [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="7131c-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="7131c-111">Overview</span></span>

<span data-ttu-id="7131c-112">Pokud chcete nasadit novou aplikaci, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="7131c-112">To deploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="7131c-113">Nahrajte balíček aplikace do úložiště bitové kopie Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="7131c-113">Upload an application package to the Service Fabric image store.</span></span>
2. <span data-ttu-id="7131c-114">Zřídit typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-114">Provision an application type.</span></span>
3. <span data-ttu-id="7131c-115">Zadejte a vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-115">Specify and create an application.</span></span>
4. <span data-ttu-id="7131c-116">Zadejte a vytvoření služeb.</span><span class="sxs-lookup"><span data-stu-id="7131c-116">Specify and create services.</span></span>

<span data-ttu-id="7131c-117">Odebrat existující aplikaci, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="7131c-117">To remove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="7131c-118">Odstraňte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7131c-118">Delete the application.</span></span>
2. <span data-ttu-id="7131c-119">Zrušit zřízení typu přidružené aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7131c-119">Unprovision the associated application type.</span></span>
3. <span data-ttu-id="7131c-120">Odstranění obsahu úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="7131c-120">Delete the image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="7131c-121">Nasazení nové aplikace</span><span class="sxs-lookup"><span data-stu-id="7131c-121">Deploy a new application</span></span>

<span data-ttu-id="7131c-122">Pokud chcete nasadit novou aplikaci, proveďte následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="7131c-122">To deploy a new application, complete the following tasks:</span></span>

### <a name="upload-a-new-application-package-to-the-image-store"></a><span data-ttu-id="7131c-123">Nahrát nový balíček aplikace do úložiště bitové kopie</span><span class="sxs-lookup"><span data-stu-id="7131c-123">Upload a new application package to the image store</span></span>

<span data-ttu-id="7131c-124">Než vytvoříte aplikaci, nahrajte balíček aplikace do úložiště bitové kopie Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="7131c-124">Before you create an application, upload the application package to the Service Fabric image store.</span></span>

<span data-ttu-id="7131c-125">Například, pokud probíhá balíčku aplikace `app_package_dir` adresáře, použijte následující příkazy pro nahrání adresáři:</span><span class="sxs-lookup"><span data-stu-id="7131c-125">For example, if your application package is in the `app_package_dir` directory, use the following commands to upload the directory:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir
```

<span data-ttu-id="7131c-126">U velkých balíčků aplikací, můžete zadat `--show-progress` možnosti zobrazíte průběh nahrávání.</span><span class="sxs-lookup"><span data-stu-id="7131c-126">For large application packages, you can specify the `--show-progress` option to display the progress of the upload.</span></span>

### <a name="provision-the-application-type"></a><span data-ttu-id="7131c-127">Zřízení typu aplikace</span><span class="sxs-lookup"><span data-stu-id="7131c-127">Provision the application type</span></span>

<span data-ttu-id="7131c-128">Po dokončení nahrávání se zřídit aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-128">When the upload is finished, provision the application.</span></span> <span data-ttu-id="7131c-129">Zřídit aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7131c-129">To provision the application, use the following command:</span></span>

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="7131c-130">Hodnota `application-type-build-path` je název adresáře, kde jste nahráli balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-130">The value for `application-type-build-path` is the name of the directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="7131c-131">Vytvoření aplikace z typ aplikace</span><span class="sxs-lookup"><span data-stu-id="7131c-131">Create an application from an application type</span></span>

<span data-ttu-id="7131c-132">Po zřízení aplikace, použijte následující příkaz k pojmenování a vytvoření aplikace:</span><span class="sxs-lookup"><span data-stu-id="7131c-132">After you provision the application, use the following command to name and create your application:</span></span>

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="7131c-133">`app-name`je název, který chcete použít pro instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-133">`app-name` is the name that you want to use for the application instance.</span></span> <span data-ttu-id="7131c-134">Manifest dříve zřízené aplikace můžete získat další parametry.</span><span class="sxs-lookup"><span data-stu-id="7131c-134">You can get additional parameters from the previously provisioned application manifest.</span></span>

<span data-ttu-id="7131c-135">Název aplikace musí začínat předponu `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="7131c-135">The application name must start with the prefix `fabric:/`.</span></span>

### <a name="create-services-for-the-new-application"></a><span data-ttu-id="7131c-136">Vytvoření nové aplikace služby</span><span class="sxs-lookup"><span data-stu-id="7131c-136">Create services for the new application</span></span>

<span data-ttu-id="7131c-137">Po vytvoření aplikace, vytvořte z aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="7131c-137">After you have created an application, create services from the application.</span></span> <span data-ttu-id="7131c-138">V následujícím příkladu vytvoříme nové bezstavové služby z naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-138">In the following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="7131c-139">Služby, které můžete vytvořit z aplikace jsou definovány v service manifest v balíčku dříve zřízené aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-139">The services that you can create from an application are defined in a service manifest in the previously provisioned application package.</span></span>

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="7131c-140">Ověřte stav a nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="7131c-140">Verify application deployment and health</span></span>

<span data-ttu-id="7131c-141">Pokud chcete ověřit, že všechno, co je v pořádku, použijte následující příkazy stavu:</span><span class="sxs-lookup"><span data-stu-id="7131c-141">To verify everything is healthy, use the following health commands:</span></span>

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

<span data-ttu-id="7131c-142">Pokud chcete ověřit, že služba je v pořádku, použijte příkazy podobné načíst stav služby a aplikace:</span><span class="sxs-lookup"><span data-stu-id="7131c-142">To verify that the service is healthy, use similar commands to retrieve the health of both the service and the application:</span></span>

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

<span data-ttu-id="7131c-143">V pořádku služeb a aplikací mít `HealthState` hodnotu `Ok`.</span><span class="sxs-lookup"><span data-stu-id="7131c-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="7131c-144">Odeberte stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="7131c-144">Remove an existing application</span></span>

<span data-ttu-id="7131c-145">Chcete-li odebrat, proveďte následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="7131c-145">To remove an application, complete the following tasks:</span></span>

### <a name="delete-the-application"></a><span data-ttu-id="7131c-146">Odstranit aplikaci</span><span class="sxs-lookup"><span data-stu-id="7131c-146">Delete the application</span></span>

<span data-ttu-id="7131c-147">Pokud chcete odstranit aplikaci, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7131c-147">To delete the application, use the following command:</span></span>

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a><span data-ttu-id="7131c-148">Zrušit zřízení typu aplikace</span><span class="sxs-lookup"><span data-stu-id="7131c-148">Unprovision the application type</span></span>

<span data-ttu-id="7131c-149">Po odstranění aplikace, můžete zrušit zřízení typu aplikace již nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="7131c-149">After you delete the application, you can unprovision the application type if you no longer need it.</span></span> <span data-ttu-id="7131c-150">Chcete-li zrušit zřízení typu aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7131c-150">To unprovision the application type, use the following command:</span></span>

```azurecli
sfctl application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="7131c-151">Zadejte název a typ verze musí odpovídat názvem a verzí v manifestu dříve zřízené aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-151">The type name and type version must match the name and version in the previously provisioned application manifest.</span></span>

### <a name="delete-the-application-package"></a><span data-ttu-id="7131c-152">Odstranění balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="7131c-152">Delete the application package</span></span>

<span data-ttu-id="7131c-153">Po mít zrušil zajišťování typu aplikace, můžete odstranit balíček aplikace z úložiště bitových kopií již nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="7131c-153">After you have unprovisioned the application type, you can delete the application package from the image store if you no longer need it.</span></span> <span data-ttu-id="7131c-154">Odstraňování balíčky aplikací pomáhá uvolnění místa na disku.</span><span class="sxs-lookup"><span data-stu-id="7131c-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="7131c-155">K odstranění balíčku aplikace z úložiště bitových kopií, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7131c-155">To delete the application package from the image store, use the following command:</span></span>

```azurecli
sfctl store delete --content-path app_package_dir
```

<span data-ttu-id="7131c-156">`content-path`musí být název adresáře, který jste nahráli při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-156">`content-path` must be the name of the directory that you uploaded when you created the application.</span></span>

## <a name="upgrade-application"></a><span data-ttu-id="7131c-157">Upgrade aplikace</span><span class="sxs-lookup"><span data-stu-id="7131c-157">Upgrade application</span></span>

<span data-ttu-id="7131c-158">Po vytvoření aplikace, můžete opakovat stejnou sadu návod ke zřízení druhou verzi vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-158">After creating your application, you can repeat the same set of steps to provision a second version of your application.</span></span> <span data-ttu-id="7131c-159">Upgradu aplikace Service Fabric potom můžete přejít na druhý verzí aplikace.</span><span class="sxs-lookup"><span data-stu-id="7131c-159">Then, with a Service Fabric application upgrade you can transition to running the second version of the application.</span></span> <span data-ttu-id="7131c-160">Další informace najdete v dokumentaci na [upgradů aplikací Service Fabric](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="7131c-160">For more information, see the documentation on [Service Fabric application upgrades](service-fabric-application-upgrade.md).</span></span>

<span data-ttu-id="7131c-161">Pokud chcete provést upgrade, nejdříve zřídit příští verze aplikace, jako předtím pomocí stejné příkazy:</span><span class="sxs-lookup"><span data-stu-id="7131c-161">To perform an upgrade, first provision the next version of the application using the same commands as before:</span></span>

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
```

<span data-ttu-id="7131c-162">Doporučujeme pak k provedení sledované automatického upgradu, spusťte upgrade spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="7131c-162">It is recommended then to perform a monitored automatic upgrade, launch the upgrade by running the following command:</span></span>

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

<span data-ttu-id="7131c-163">Upgrady přepsat existující parametry s jakoukoli sadu je zadán.</span><span class="sxs-lookup"><span data-stu-id="7131c-163">Upgrades override existing parameters with whatever set is specified.</span></span> <span data-ttu-id="7131c-164">Parametry aplikačního mají být předány jako argumenty pro příkaz pro upgrade, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="7131c-164">Application parameters should be passed as arguments to the upgrade command, if necessary.</span></span> <span data-ttu-id="7131c-165">Parametry aplikačního by měl být zakódován jako objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="7131c-165">Application parameters should be encoded as a JSON object.</span></span>

<span data-ttu-id="7131c-166">Pokud chcete načíst všechny dříve zadané parametry, můžete použít `sfctl application info` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7131c-166">To retrieve any parameters previously specified, you can use the `sfctl application info` command.</span></span>

<span data-ttu-id="7131c-167">Když probíhá upgrade aplikace, stav se dá načíst pomocí `sfctl application upgrade-status` příkaz.</span><span class="sxs-lookup"><span data-stu-id="7131c-167">When an application upgrade is in progress, the status can be retrieved using the `sfctl application upgrade-status` command.</span></span>

<span data-ttu-id="7131c-168">Nakonec, pokud je v průběhu a musí být zrušena upgrade, můžete použít `sfctl application upgrade-rollback` vrácení upgradu.</span><span class="sxs-lookup"><span data-stu-id="7131c-168">Finally, if an upgrade is in progress and needs to be canceled, you can use the `sfctl application upgrade-rollback` to roll back the upgrade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7131c-169">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7131c-169">Next steps</span></span>

* [<span data-ttu-id="7131c-170">Základy služby Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="7131c-170">Service Fabric CLI basics</span></span>](service-fabric-cli.md)
* [<span data-ttu-id="7131c-171">Začínáme se službou Service Fabric v systému Linux</span><span class="sxs-lookup"><span data-stu-id="7131c-171">Getting started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="7131c-172">Spuštění upgradu aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="7131c-172">Launching a Service Fabric application upgrade</span></span>](service-fabric-application-upgrade.md)

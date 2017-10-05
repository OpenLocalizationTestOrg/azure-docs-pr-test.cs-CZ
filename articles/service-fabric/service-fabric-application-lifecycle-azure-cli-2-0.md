---
title: "Spravovat aplikace Azure Service Fabric pomocí Azure CLI 2.0"
description: "Zjistěte, jak nasadit a odebrání aplikace z clusteru služby Azure Service Fabric pomocí Azure CLI 2.0."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: 5728339236e3819b301e428f9d7a8add08f02b3e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-cli-20"></a><span data-ttu-id="bb26c-103">Spravovat aplikace Azure Service Fabric pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bb26c-103">Manage an Azure Service Fabric application by using Azure CLI 2.0</span></span>

<span data-ttu-id="bb26c-104">Naučte se vytvářet a odstraňovat aplikace, které jsou spuštěny v clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bb26c-104">Learn how to create and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb26c-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bb26c-105">Prerequisites</span></span>

* <span data-ttu-id="bb26c-106">Nainstalujte rozhraní příkazového řádku Azure 2.0.</span><span class="sxs-lookup"><span data-stu-id="bb26c-106">Install Azure CLI 2.0.</span></span> <span data-ttu-id="bb26c-107">Pak vyberte cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bb26c-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="bb26c-108">Další informace najdete v tématu [Začínáme s Azure CLI 2.0](service-fabric-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="bb26c-108">For more information, see [Get started with Azure CLI 2.0](service-fabric-azure-cli-2-0.md).</span></span>

* <span data-ttu-id="bb26c-109">Máte připravený k nasazení balíčku aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bb26c-109">Have a Service Fabric application package ready to be deployed.</span></span> <span data-ttu-id="bb26c-110">Další informace o tom, jak vytvořit a balíček aplikace, přečtěte si informace o [model aplikace Service Fabric](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="bb26c-110">For more information about how to author and package an application, read about the [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="bb26c-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="bb26c-111">Overview</span></span>

<span data-ttu-id="bb26c-112">Pokud chcete nasadit novou aplikaci, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="bb26c-112">To deploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="bb26c-113">Nahrajte balíček aplikace do úložiště bitové kopie Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bb26c-113">Upload an application package to the Service Fabric image store.</span></span>
2. <span data-ttu-id="bb26c-114">Zřídit typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb26c-114">Provision an application type.</span></span>
3. <span data-ttu-id="bb26c-115">Zadejte a vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb26c-115">Specify and create an application.</span></span>
4. <span data-ttu-id="bb26c-116">Zadejte a vytvoření služeb.</span><span class="sxs-lookup"><span data-stu-id="bb26c-116">Specify and create services.</span></span>

<span data-ttu-id="bb26c-117">Odebrat existující aplikaci, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="bb26c-117">To remove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="bb26c-118">Odstraňte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bb26c-118">Delete the application.</span></span>
2. <span data-ttu-id="bb26c-119">Zrušit zřízení typu přidružené aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bb26c-119">Unprovision the associated application type.</span></span>
3. <span data-ttu-id="bb26c-120">Odstranění obsahu úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="bb26c-120">Delete the image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="bb26c-121">Nasazení nové aplikace</span><span class="sxs-lookup"><span data-stu-id="bb26c-121">Deploy a new application</span></span>

<span data-ttu-id="bb26c-122">Chcete-li nasadit novou aplikaci, proveďte následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="bb26c-122">To deploy a new application, complete the following tasks.</span></span>

### <a name="upload-a-new-application-package-to-the-image-store"></a><span data-ttu-id="bb26c-123">Nahrát nový balíček aplikace do úložiště bitové kopie</span><span class="sxs-lookup"><span data-stu-id="bb26c-123">Upload a new application package to the image store</span></span>

<span data-ttu-id="bb26c-124">Než vytvoříte aplikaci, nahrajte balíček aplikace do úložiště bitové kopie Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bb26c-124">Before you create an application, upload the application package to the Service Fabric image store.</span></span> 

<span data-ttu-id="bb26c-125">Například, pokud probíhá balíčku aplikace `app_package_dir` adresáře, použijte následující příkazy pro nahrání adresáři:</span><span class="sxs-lookup"><span data-stu-id="bb26c-125">For example, if your application package is in the `app_package_dir` directory, use the following commands to upload the directory:</span></span>

```azurecli
az sf application upload --path ~/app_package_dir
```

<span data-ttu-id="bb26c-126">U velkých balíčků aplikací, můžete zadat `--show-progress` možnosti zobrazíte průběh nahrávání.</span><span class="sxs-lookup"><span data-stu-id="bb26c-126">For large application packages, you can specify the `--show-progress` option to display the progress of the upload.</span></span>

### <a name="provision-the-application-type"></a><span data-ttu-id="bb26c-127">Zřízení typu aplikace</span><span class="sxs-lookup"><span data-stu-id="bb26c-127">Provision the application type</span></span>

<span data-ttu-id="bb26c-128">Po dokončení nahrávání se zřídit aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb26c-128">When the upload is finished, provision the application.</span></span> <span data-ttu-id="bb26c-129">Zřídit aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb26c-129">To provision the application, use the following command:</span></span>

```azurecli
az sf application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="bb26c-130">Hodnota `application-type-build-path` je název adresáře, kde jste nahráli balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb26c-130">The value for `application-type-build-path` is the name of the directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="bb26c-131">Vytvoření aplikace z typ aplikace</span><span class="sxs-lookup"><span data-stu-id="bb26c-131">Create an application from an application type</span></span>

<span data-ttu-id="bb26c-132">Po zřízení aplikace, použijte následující příkaz k pojmenování a vytvoření aplikace:</span><span class="sxs-lookup"><span data-stu-id="bb26c-132">After you provision the application, use the following command to name and create your application:</span></span>

```azurecli
az sf application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="bb26c-133">`app-name`je název, který chcete použít pro instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb26c-133">`app-name` is the name that you want to use for the application instance.</span></span> <span data-ttu-id="bb26c-134">Manifest dříve zřízené aplikace můžete získat další parametry.</span><span class="sxs-lookup"><span data-stu-id="bb26c-134">You can get additional parameters from the previously provisioned application manifest.</span></span>

<span data-ttu-id="bb26c-135">Název aplikace musí začínat předponu `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="bb26c-135">The application name must start with the prefix `fabric:/`.</span></span>

### <a name="create-services-for-the-new-application"></a><span data-ttu-id="bb26c-136">Vytvoření nové aplikace služby</span><span class="sxs-lookup"><span data-stu-id="bb26c-136">Create services for the new application</span></span>

<span data-ttu-id="bb26c-137">Po vytvoření aplikace, vytvořte z aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="bb26c-137">After you have created an application, create services from the application.</span></span> <span data-ttu-id="bb26c-138">V následujícím příkladu vytvoříme nové bezstavové služby z naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb26c-138">In the following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="bb26c-139">Služby, které můžete vytvořit z aplikace jsou definovány v service manifest v balíčku dříve zřízené aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb26c-139">The services that you can create from an application are defined in a service manifest in the previously provisioned application package.</span></span>

```azurecli
az sf service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="bb26c-140">Ověřte stav a nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="bb26c-140">Verify application deployment and health</span></span>

<span data-ttu-id="bb26c-141">Pokud chcete ověřit, že byly úspěšně nasazeny aplikace a služby, zkontrolujte, jestli jsou uvedený aplikace a služby:</span><span class="sxs-lookup"><span data-stu-id="bb26c-141">To verify that an application and service were successfully deployed, check that the application and service are listed:</span></span>

```azurecli
az sf application list
az sf service list --application-list TestApp
```

<span data-ttu-id="bb26c-142">Pokud chcete ověřit, že služba je v pořádku, použijte příkazy podobné načíst stav služby a aplikace:</span><span class="sxs-lookup"><span data-stu-id="bb26c-142">To verify that the service is healthy, use similar commands to retrieve the health of both the service and the application:</span></span>

```azurecli
az sf application health --application-id TestApp
az sf service health --service-id TestApp/TestSvc
```

<span data-ttu-id="bb26c-143">V pořádku služeb a aplikací mít `HealthState` hodnotu `Ok`.</span><span class="sxs-lookup"><span data-stu-id="bb26c-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="bb26c-144">Odeberte stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="bb26c-144">Remove an existing application</span></span>

<span data-ttu-id="bb26c-145">Chcete-li odebrat, proveďte následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="bb26c-145">To remove an application, complete the following tasks.</span></span>

### <a name="delete-the-application"></a><span data-ttu-id="bb26c-146">Odstranit aplikaci</span><span class="sxs-lookup"><span data-stu-id="bb26c-146">Delete the application</span></span>

<span data-ttu-id="bb26c-147">Pokud chcete odstranit aplikaci, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb26c-147">To delete the application, use the following command:</span></span>

```azurecli
az sf application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a><span data-ttu-id="bb26c-148">Zrušit zřízení typu aplikace</span><span class="sxs-lookup"><span data-stu-id="bb26c-148">Unprovision the application type</span></span>

<span data-ttu-id="bb26c-149">Po odstranění aplikace, můžete zrušit zřízení typu aplikace již nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="bb26c-149">After you delete the application, you can unprovision the application type if you no longer need it.</span></span> <span data-ttu-id="bb26c-150">Chcete-li zrušit zřízení typu aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb26c-150">To unprovision the application type, use the following command:</span></span>

```azurecli
az sf application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="bb26c-151">Zadejte název a typ verze musí odpovídat názvem a verzí v manifestu dříve zřízené aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb26c-151">The type name and type version must match the name and version in the previously provisioned application manifest.</span></span>

### <a name="delete-the-application-package"></a><span data-ttu-id="bb26c-152">Odstranění balíčku aplikace</span><span class="sxs-lookup"><span data-stu-id="bb26c-152">Delete the application package</span></span>

<span data-ttu-id="bb26c-153">Po mít zrušil zajišťování typu aplikace, můžete odstranit balíček aplikace z úložiště bitových kopií již nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="bb26c-153">After you have unprovisioned the application type, you can delete the application package from the image store if you no longer need it.</span></span> <span data-ttu-id="bb26c-154">Odstraňování balíčky aplikací pomáhá uvolnění místa na disku.</span><span class="sxs-lookup"><span data-stu-id="bb26c-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="bb26c-155">K odstranění balíčku aplikace z úložiště bitových kopií, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bb26c-155">To delete the application package from the image store, use the following command:</span></span>

```azurecli
az sf application package-delete --content-path app_package_dir
```

<span data-ttu-id="bb26c-156">`content-path`musí být název adresáře, který jste nahráli při vytváření aplikace.</span><span class="sxs-lookup"><span data-stu-id="bb26c-156">`content-path` must be the name of the directory that you uploaded when you created the application.</span></span>

## <a name="related-articles"></a><span data-ttu-id="bb26c-157">Související články</span><span class="sxs-lookup"><span data-stu-id="bb26c-157">Related articles</span></span>

* [<span data-ttu-id="bb26c-158">Začínáme s platformou Service Fabric a Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bb26c-158">Get started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="bb26c-159">Začínáme se Service Fabric XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="bb26c-159">Get started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)

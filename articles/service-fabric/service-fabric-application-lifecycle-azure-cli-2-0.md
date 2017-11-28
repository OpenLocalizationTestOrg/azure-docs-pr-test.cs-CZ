---
title: "aplikace Azure Service Fabric aaaManage pomocí Azure CLI 2.0"
description: "Zjistěte, jak clusteru toodeploy a odebrat aplikace ze Azure Service Fabric pomocí Azure CLI 2.0."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ae1ba19513978b0f95ffb65d5f1f7a21ed5f2894
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-cli-20"></a><span data-ttu-id="5a8d6-103">Spravovat aplikace Azure Service Fabric pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5a8d6-103">Manage an Azure Service Fabric application by using Azure CLI 2.0</span></span>

<span data-ttu-id="5a8d6-104">Zjistěte, jak toocreate a odstranění aplikace, které jsou spuštěny v clusteru služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-104">Learn how toocreate and delete applications that are running in an Azure Service Fabric cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a8d6-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5a8d6-105">Prerequisites</span></span>

* <span data-ttu-id="5a8d6-106">Nainstalujte rozhraní příkazového řádku Azure 2.0.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-106">Install Azure CLI 2.0.</span></span> <span data-ttu-id="5a8d6-107">Pak vyberte cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-107">Then, select your Service Fabric cluster.</span></span> <span data-ttu-id="5a8d6-108">Další informace najdete v tématu [Začínáme s Azure CLI 2.0](service-fabric-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="5a8d6-108">For more information, see [Get started with Azure CLI 2.0](service-fabric-azure-cli-2-0.md).</span></span>

* <span data-ttu-id="5a8d6-109">Máte Service Fabric aplikace balíčku připravené toobe nasazení.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-109">Have a Service Fabric application package ready toobe deployed.</span></span> <span data-ttu-id="5a8d6-110">Další informace o tom, tooauthor a balíček aplikace, přečtěte si informace o hello [model aplikace Service Fabric](service-fabric-application-model.md).</span><span class="sxs-lookup"><span data-stu-id="5a8d6-110">For more information about how tooauthor and package an application, read about hello [Service Fabric application model](service-fabric-application-model.md).</span></span>

## <a name="overview"></a><span data-ttu-id="5a8d6-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="5a8d6-111">Overview</span></span>

<span data-ttu-id="5a8d6-112">toodeploy novou aplikaci, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-112">toodeploy a new application, complete these steps:</span></span>

1. <span data-ttu-id="5a8d6-113">Nahrajte úložišti aplikace Service Fabric toohello balíček bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-113">Upload an application package toohello Service Fabric image store.</span></span>
2. <span data-ttu-id="5a8d6-114">Zřídit typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-114">Provision an application type.</span></span>
3. <span data-ttu-id="5a8d6-115">Zadejte a vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-115">Specify and create an application.</span></span>
4. <span data-ttu-id="5a8d6-116">Zadejte a vytvoření služeb.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-116">Specify and create services.</span></span>

<span data-ttu-id="5a8d6-117">tooremove existující aplikaci, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-117">tooremove an existing application, complete these steps:</span></span>

1. <span data-ttu-id="5a8d6-118">Odstraňte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-118">Delete hello application.</span></span>
2. <span data-ttu-id="5a8d6-119">Unprovision hello související typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-119">Unprovision hello associated application type.</span></span>
3. <span data-ttu-id="5a8d6-120">Odstraňte hello obsahu úložiště bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-120">Delete hello image store content.</span></span>

## <a name="deploy-a-new-application"></a><span data-ttu-id="5a8d6-121">Nasazení nové aplikace</span><span class="sxs-lookup"><span data-stu-id="5a8d6-121">Deploy a new application</span></span>

<span data-ttu-id="5a8d6-122">toodeploy novou aplikaci, dokončení hello následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-122">toodeploy a new application, complete hello following tasks.</span></span>

### <a name="upload-a-new-application-package-toohello-image-store"></a><span data-ttu-id="5a8d6-123">Nahrát nové úložiště bitové kopie balíčku toohello aplikace</span><span class="sxs-lookup"><span data-stu-id="5a8d6-123">Upload a new application package toohello image store</span></span>

<span data-ttu-id="5a8d6-124">Předtím, než vytvoříte aplikaci, nahrajte balíček aplikace hello, toohello úložiště bitových kopií Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-124">Before you create an application, upload hello application package toohello Service Fabric image store.</span></span> 

<span data-ttu-id="5a8d6-125">Například, pokud je balíčku aplikace v hello `app_package_dir` directory hello použijte následující příkazy tooupload hello adresáře:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-125">For example, if your application package is in hello `app_package_dir` directory, use hello following commands tooupload hello directory:</span></span>

```azurecli
az sf application upload --path ~/app_package_dir
```

<span data-ttu-id="5a8d6-126">U velkých balíčků aplikací, můžete zadat hello `--show-progress` možnost toodisplay hello průběh nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-126">For large application packages, you can specify hello `--show-progress` option toodisplay hello progress of hello upload.</span></span>

### <a name="provision-hello-application-type"></a><span data-ttu-id="5a8d6-127">Typ aplikace hello zřizování</span><span class="sxs-lookup"><span data-stu-id="5a8d6-127">Provision hello application type</span></span>

<span data-ttu-id="5a8d6-128">Po dokončení nahrávání hello zřídit hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-128">When hello upload is finished, provision hello application.</span></span> <span data-ttu-id="5a8d6-129">tooprovision hello aplikace hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-129">tooprovision hello application, use hello following command:</span></span>

```azurecli
az sf application provision --application-type-build-path app_package_dir
```

<span data-ttu-id="5a8d6-130">Hello hodnota `application-type-build-path` je název hello hello adresáře, kde jste nahráli balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-130">hello value for `application-type-build-path` is hello name of hello directory where you uploaded your application package.</span></span>

### <a name="create-an-application-from-an-application-type"></a><span data-ttu-id="5a8d6-131">Vytvoření aplikace z typ aplikace</span><span class="sxs-lookup"><span data-stu-id="5a8d6-131">Create an application from an application type</span></span>

<span data-ttu-id="5a8d6-132">Po zřízení hello aplikace, použijte následující příkaz tooname hello a vytvoření aplikace:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-132">After you provision hello application, use hello following command tooname and create your application:</span></span>

```azurecli
az sf application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

<span data-ttu-id="5a8d6-133">`app-name`je název hello má toouse pro instanci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-133">`app-name` is hello name that you want toouse for hello application instance.</span></span> <span data-ttu-id="5a8d6-134">Další parametry můžete získat z manifestu dříve zřízené aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-134">You can get additional parameters from hello previously provisioned application manifest.</span></span>

<span data-ttu-id="5a8d6-135">název aplikace Hello musí začínat hello předponu `fabric:/`.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-135">hello application name must start with hello prefix `fabric:/`.</span></span>

### <a name="create-services-for-hello-new-application"></a><span data-ttu-id="5a8d6-136">Vytvoření služby pro novou aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="5a8d6-136">Create services for hello new application</span></span>

<span data-ttu-id="5a8d6-137">Po vytvoření aplikace služby vytvořte z aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-137">After you have created an application, create services from hello application.</span></span> <span data-ttu-id="5a8d6-138">V následujícím příkladu hello vytvoříme nové bezstavové služby z naší aplikace.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-138">In hello following example, we create a new stateless service from our application.</span></span> <span data-ttu-id="5a8d6-139">Hello služby, které můžete vytvořit z aplikace jsou definovány v service manifest v balíčku dříve zřízené aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-139">hello services that you can create from an application are defined in a service manifest in hello previously provisioned application package.</span></span>

```azurecli
az sf service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a><span data-ttu-id="5a8d6-140">Ověřte stav a nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="5a8d6-140">Verify application deployment and health</span></span>

<span data-ttu-id="5a8d6-141">tooverify, že byly úspěšně nasazeny aplikace a služby, zkontrolujte, že jsou uvedeny hello aplikace a služby:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-141">tooverify that an application and service were successfully deployed, check that hello application and service are listed:</span></span>

```azurecli
az sf application list
az sf service list --application-list TestApp
```

<span data-ttu-id="5a8d6-142">tooverify, že služba hello je v pořádku, použijte podobné příkazy tooretrieve hello stavu hello služby a aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-142">tooverify that hello service is healthy, use similar commands tooretrieve hello health of both hello service and hello application:</span></span>

```azurecli
az sf application health --application-id TestApp
az sf service health --service-id TestApp/TestSvc
```

<span data-ttu-id="5a8d6-143">V pořádku služeb a aplikací mít `HealthState` hodnotu `Ok`.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-143">Healthy services and applications have a `HealthState` value of `Ok`.</span></span>

## <a name="remove-an-existing-application"></a><span data-ttu-id="5a8d6-144">Odeberte stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="5a8d6-144">Remove an existing application</span></span>

<span data-ttu-id="5a8d6-145">tooremove aplikace, dokončení hello následující úlohy.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-145">tooremove an application, complete hello following tasks.</span></span>

### <a name="delete-hello-application"></a><span data-ttu-id="5a8d6-146">Odstranit aplikaci hello</span><span class="sxs-lookup"><span data-stu-id="5a8d6-146">Delete hello application</span></span>

<span data-ttu-id="5a8d6-147">toodelete hello aplikace hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-147">toodelete hello application, use hello following command:</span></span>

```azurecli
az sf application delete --application-id TestEdApp
```

### <a name="unprovision-hello-application-type"></a><span data-ttu-id="5a8d6-148">Zrušit zřízení typu aplikace hello</span><span class="sxs-lookup"><span data-stu-id="5a8d6-148">Unprovision hello application type</span></span>

<span data-ttu-id="5a8d6-149">Po odstranění aplikace hello, můžete zrušit zřízení typu aplikace hello již nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-149">After you delete hello application, you can unprovision hello application type if you no longer need it.</span></span> <span data-ttu-id="5a8d6-150">toounprovision hello typ aplikace, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-150">toounprovision hello application type, use hello following command:</span></span>

```azurecli
az sf application unprovision --application-type-name TestAppTye --application-type-version 1.0
```

<span data-ttu-id="5a8d6-151">Hello zadejte název a typ verze musí odpovídat hello názvem a verzí v manifestu dříve zřízené aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-151">hello type name and type version must match hello name and version in hello previously provisioned application manifest.</span></span>

### <a name="delete-hello-application-package"></a><span data-ttu-id="5a8d6-152">Odstranění balíčku aplikace hello</span><span class="sxs-lookup"><span data-stu-id="5a8d6-152">Delete hello application package</span></span>

<span data-ttu-id="5a8d6-153">Po mít zrušil zajišťování hello typu aplikace, můžete odstranit balíček aplikace hello z úložiště bitových kopií hello již nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-153">After you have unprovisioned hello application type, you can delete hello application package from hello image store if you no longer need it.</span></span> <span data-ttu-id="5a8d6-154">Odstraňování balíčky aplikací pomáhá uvolnění místa na disku.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-154">Deleting application packages helps reclaim disk space.</span></span> 

<span data-ttu-id="5a8d6-155">balíček aplikace hello toodelete z úložiště bitových kopií hello, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5a8d6-155">toodelete hello application package from hello image store, use hello following command:</span></span>

```azurecli
az sf application package-delete --content-path app_package_dir
```

<span data-ttu-id="5a8d6-156">`content-path`musí být název hello hello adresáře, který jste nahráli při vytváření aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5a8d6-156">`content-path` must be hello name of hello directory that you uploaded when you created hello application.</span></span>

## <a name="related-articles"></a><span data-ttu-id="5a8d6-157">Související články</span><span class="sxs-lookup"><span data-stu-id="5a8d6-157">Related articles</span></span>

* [<span data-ttu-id="5a8d6-158">Začínáme s platformou Service Fabric a Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5a8d6-158">Get started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="5a8d6-159">Začínáme se Service Fabric XPlat CLI</span><span class="sxs-lookup"><span data-stu-id="5a8d6-159">Get started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)

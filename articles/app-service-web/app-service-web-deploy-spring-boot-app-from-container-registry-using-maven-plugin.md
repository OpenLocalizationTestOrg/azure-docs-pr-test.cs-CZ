---
title: "aaaHow toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy pružiny spuštění aplikace v tooAzure registru kontejner Azure App Service"
description: "Tento kurz vás provede, když hello kroky toodeploy pružiny spuštění aplikace v tooAzure tooAzure registru kontejner Azure App Service pomocí modulu plug-in Maven."
services: 
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 55b95e310c9ee186a6d77d941c5a620c2e259d8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-maven-plugin-for-azure-web-apps-toodeploy-a-spring-boot-app-in-azure-container-registry-tooazure-app-service"></a><span data-ttu-id="875f7-103">Jak toouse hello Maven modulu plug-in pro Azure Web Apps toodeploy pružiny spuštění aplikace v tooAzure registru kontejner Azure App Service</span><span class="sxs-lookup"><span data-stu-id="875f7-103">How toouse hello Maven Plugin for Azure Web Apps toodeploy a Spring Boot app in Azure Container Registry tooAzure App Service</span></span>

<span data-ttu-id="875f7-104">Hello  **[pružiny Framework]**  oblíbených rozhraní open source, které pomáhá vytvářet webové, mobilní a aplikacích API vývojáře v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="875f7-104">hello **[Spring Framework]** is a popular open-source framework that helps Java developers create web, mobile, and API applications.</span></span> <span data-ttu-id="875f7-105">Tento kurz používá ukázkovou aplikaci vytvořený [pružiny spouštěcí], konvence přístupu při použití pružiny tooget rychle začít.</span><span class="sxs-lookup"><span data-stu-id="875f7-105">This tutorial uses a sample app created using [Spring Boot], a convention-driven approach for using Spring tooget started quickly.</span></span>

<span data-ttu-id="875f7-106">Tento článek ukazuje, jak toodeploy tooAzure aplikace pružiny spouštěcí ukázkový kontejner registru a pak použijte hello Maven modulu plug-in pro Azure Web Apps toodeploy tooAzure vaše aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="875f7-106">This article demonstrates how toodeploy a sample Spring Boot application tooAzure Container Registry, and then use hello Maven Plugin for Azure Web Apps toodeploy your application tooAzure App Service.</span></span>

> [!NOTE]
>
> <span data-ttu-id="875f7-107">Hello Maven modul plug-in pro Azure Web Apps je aktuálně k dispozici jako náhled.</span><span class="sxs-lookup"><span data-stu-id="875f7-107">hello Maven Plugin for Azure Web Apps is currently available as a preview.</span></span> <span data-ttu-id="875f7-108">Teď je podporován pouze publikování FTP, i když další funkce, které jsou naplánované pro budoucí hello.</span><span class="sxs-lookup"><span data-stu-id="875f7-108">For now, only FTP publishing is supported, although additional features are planned for hello future.</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="875f7-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="875f7-109">Prerequisites</span></span>

<span data-ttu-id="875f7-110">Pořadí toocomplete hello kroky v tomto kurzu je třeba toohave hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="875f7-110">In order toocomplete hello steps in this tutorial, you need toohave hello following prerequisites:</span></span>

* <span data-ttu-id="875f7-111">Předplatné Azure; Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN] nebo si zaregistrovat [bezplatný účet Azure].</span><span class="sxs-lookup"><span data-stu-id="875f7-111">An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].</span></span>
* <span data-ttu-id="875f7-112">Hello [rozhraní příkazového řádku Azure (CLI)].</span><span class="sxs-lookup"><span data-stu-id="875f7-112">hello [Azure Command-Line Interface (CLI)].</span></span>
* <span data-ttu-id="875f7-113">Aktuální [Java Development Kit (JDK)], verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="875f7-113">An up-to-date [Java Development Kit (JDK)], version 1.7 or later.</span></span>
* <span data-ttu-id="875f7-114">Apache na [Maven] sestavení nástroj (verze 3).</span><span class="sxs-lookup"><span data-stu-id="875f7-114">Apache's [Maven] build tool (Version 3).</span></span>
* <span data-ttu-id="875f7-115">A [Git] klienta.</span><span class="sxs-lookup"><span data-stu-id="875f7-115">A [Git] client.</span></span>
* <span data-ttu-id="875f7-116">A [Docker] klienta.</span><span class="sxs-lookup"><span data-stu-id="875f7-116">A [Docker] client.</span></span>

> [!NOTE]
>
> <span data-ttu-id="875f7-117">Z důvodu toohello virtualizace požadavky tohoto kurzu nelze sledovat hello kroky v tomto článku na virtuálním počítači; fyzický počítač musí používat s funkcemi virtualizace.</span><span class="sxs-lookup"><span data-stu-id="875f7-117">Due toohello virtualization requirements of this tutorial, you cannot follow hello steps in this article on a virtual machine; you must use a physical computer with virtualization features enabled.</span></span>
>

## <a name="clone-hello-sample-spring-boot-on-docker-web-app"></a><span data-ttu-id="875f7-118">Klon hello ukázku pružiny spouštěcí Docker webové aplikace</span><span class="sxs-lookup"><span data-stu-id="875f7-118">Clone hello sample Spring Boot on Docker web app</span></span>

<span data-ttu-id="875f7-119">V této části klonovat kontejnerizované pružiny spouštěcí aplikace a otestovat ji místně.</span><span class="sxs-lookup"><span data-stu-id="875f7-119">In this section, you clone a containerized Spring Boot application and test it locally.</span></span>

1. <span data-ttu-id="875f7-120">Otevřete příkazový řádek nebo okno terminálu a vytvářet místní adresář toohold pružiny spouštěcí aplikace a změnit adresář toothat; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-120">Open a command prompt or terminal window and create a local directory toohold your Spring Boot application, and change toothat directory; for example:</span></span>
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   <span data-ttu-id="875f7-121">--nebo--</span><span class="sxs-lookup"><span data-stu-id="875f7-121">-- or --</span></span>
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. <span data-ttu-id="875f7-122">Klon hello [pružiny spouštěcí na Docker Začínáme] ukázkový projekt do adresáře hello jste vytvořili; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-122">Clone hello [Spring Boot on Docker Getting Started] sample project into hello directory you created; for example:</span></span>
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
   ```

1. <span data-ttu-id="875f7-123">Změnit adresář toohello dokončit projekt; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-123">Change directory toohello completed project; for example:</span></span>
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. <span data-ttu-id="875f7-124">Vytvořit soubor JAR hello pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-124">Build hello JAR file using Maven; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="875f7-125">Po vytvoření webové aplikace hello spusťte hello webové aplikace pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-125">When hello web app has been created, start hello web app using Maven; for example:</span></span>
   ```shell
   mvn spring-boot:run
   ```

1. <span data-ttu-id="875f7-126">Test webové aplikace hello procházení tooit místně pomocí webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="875f7-126">Test hello web app by browsing tooit locally using a web browser.</span></span> <span data-ttu-id="875f7-127">Můžete například použít následující příkaz, pokud máte k dispozici curl hello:</span><span class="sxs-lookup"><span data-stu-id="875f7-127">For example, you could use hello following command if you have curl available:</span></span>
   ```shell
   curl http://localhost:8080
   ```

1. <span data-ttu-id="875f7-128">Měli byste vidět hello následující zpráva: **Hello, World Docker**</span><span class="sxs-lookup"><span data-stu-id="875f7-128">You should see hello following message displayed: **Hello Docker World**</span></span>

   ![Procházet ukázkovou aplikaci místně][SB01]

## <a name="create-an-azure-service-principal"></a><span data-ttu-id="875f7-130">Vytvořit objekt služby Azure</span><span class="sxs-lookup"><span data-stu-id="875f7-130">Create an Azure service principal</span></span>

<span data-ttu-id="875f7-131">V této části vytvoříte Azure instanční objekt, který hello Maven modulu plug-in používá při nasazení vaší tooAzure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="875f7-131">In this section, you create an Azure service principal that hello Maven plugin uses when deploying your container tooAzure.</span></span>

1. <span data-ttu-id="875f7-132">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="875f7-132">Open a command prompt.</span></span>

1. <span data-ttu-id="875f7-133">Přihlaste se k účtu Azure pomocí hello rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="875f7-133">Sign into your Azure account by using hello Azure CLI:</span></span>
   ```azurecli
   az login
   ```
   <span data-ttu-id="875f7-134">Postup hello pokyny toocomplete hello přihlášení.</span><span class="sxs-lookup"><span data-stu-id="875f7-134">Follow hello instructions toocomplete hello sign-in process.</span></span>

1. <span data-ttu-id="875f7-135">Vytvořte objekt služby Azure:</span><span class="sxs-lookup"><span data-stu-id="875f7-135">Create an Azure service principal:</span></span>
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   <span data-ttu-id="875f7-136">Kde `uuuuuuuu` je hello uživatelské jméno a `pppppppp` je hello heslo pro hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="875f7-136">Where `uuuuuuuu` is hello user name and `pppppppp` is hello password for hello service principal.</span></span>

1. <span data-ttu-id="875f7-137">Azure odpoví JSON, která se podobá hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="875f7-137">Azure responds with JSON that resembles hello following example:</span></span>
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > <span data-ttu-id="875f7-138">Při konfiguraci vašeho kontejneru tooAzure hello Maven modulu plug-in toodeploy použijete hello hodnoty z této odpovědi JSON.</span><span class="sxs-lookup"><span data-stu-id="875f7-138">You will use hello values from this JSON response when you configure hello Maven plugin toodeploy your container tooAzure.</span></span> <span data-ttu-id="875f7-139">Hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, a `tttttttt` jsou zástupné hodnoty, které jsou používány toomake tento příklad je snazší toomap tyto hodnoty tootheir příslušných prvky při konfiguraci vašeho Maven `settings.xml` další soubor v hello oddíl.</span><span class="sxs-lookup"><span data-stu-id="875f7-139">hello `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, and `tttttttt` are placeholder values, which are used in this example toomake it easier toomap these values tootheir respective elements when you configure your Maven `settings.xml` file in hello next section.</span></span>
   >
   >

## <a name="create-an-azure-container-registry-using-hello-azure-cli"></a><span data-ttu-id="875f7-140">Vytvoření registru kontejneru služby Azure pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="875f7-140">Create an Azure Container Registry using hello Azure CLI</span></span>

1. <span data-ttu-id="875f7-141">Otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="875f7-141">Open a command prompt.</span></span>

1. <span data-ttu-id="875f7-142">Přihlaste se tooyour účet Azure:</span><span class="sxs-lookup"><span data-stu-id="875f7-142">Log in tooyour Azure account:</span></span>
   ```azurecli
   az login
   ```

1. <span data-ttu-id="875f7-143">Vytvořte skupinu prostředků pro hello prostředky Azure, které budete používat v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="875f7-143">Create a resource group for hello Azure resources you will use in this article:</span></span>
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   <span data-ttu-id="875f7-144">Nahraďte `wingtiptoysresources` v tomto příkladu s jedinečným názvem skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="875f7-144">Replace `wingtiptoysresources` in this example with a unique name for your resource group.</span></span>

1. <span data-ttu-id="875f7-145">Vytvořte kontejner privátní Azure registru ve skupině prostředků hello pružiny spouštěcí aplikace:</span><span class="sxs-lookup"><span data-stu-id="875f7-145">Create a private Azure container registry in hello resource group for your Spring Boot app:</span></span> 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   <span data-ttu-id="875f7-146">Nahraďte `wingtiptoysregistry` v tomto příkladu se jedinečný název vašeho kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="875f7-146">Replace `wingtiptoysregistry` in this example with a unique name for your container registry.</span></span>

1. <span data-ttu-id="875f7-147">Načtení hello hesla pro vaše registru kontejneru:</span><span class="sxs-lookup"><span data-stu-id="875f7-147">Retrieve hello password for your container registry:</span></span>
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   <span data-ttu-id="875f7-148">Azure bude odpovídat pomocí hesla; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-148">Azure will respond with your password; for example:</span></span>
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-tooyour-maven-settings"></a><span data-ttu-id="875f7-149">Přidat kontejner Azure registru a nastavení Maven hlavní tooyour služba Azure</span><span class="sxs-lookup"><span data-stu-id="875f7-149">Add your Azure container registry and Azure service principal tooyour Maven settings</span></span>

1. <span data-ttu-id="875f7-150">Otevřete váš Maven `settings.xml` soubor v textovém editoru; může být tento soubor v cestě jako hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="875f7-150">Open your Maven `settings.xml` file in a text editor; this file might be in a path like hello following examples:</span></span>
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. <span data-ttu-id="875f7-151">Přidejte nastavení registru kontejner Azure přístup z předchozí části tohoto článku toohello hello `<servers>` kolekce v hello *souborech settings.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-151">Add your Azure Container Registry access settings from hello previous section of this article toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   <span data-ttu-id="875f7-152">Kde:</span><span class="sxs-lookup"><span data-stu-id="875f7-152">Where:</span></span>
   <span data-ttu-id="875f7-153">Element</span><span class="sxs-lookup"><span data-stu-id="875f7-153">Element</span></span> | <span data-ttu-id="875f7-154">Popis</span><span class="sxs-lookup"><span data-stu-id="875f7-154">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="875f7-155">Obsahuje název vaší privátní kontejner Azure registru hello.</span><span class="sxs-lookup"><span data-stu-id="875f7-155">Contains hello name of your private Azure container registry.</span></span>
   `<username>` | <span data-ttu-id="875f7-156">Obsahuje název vaší privátní kontejner Azure registru hello.</span><span class="sxs-lookup"><span data-stu-id="875f7-156">Contains hello name of your private Azure container registry.</span></span>
   `<password>` | <span data-ttu-id="875f7-157">Obsahuje hello heslo, které jste získali v předchozí části tohoto článku hello.</span><span class="sxs-lookup"><span data-stu-id="875f7-157">Contains hello password you retrieved in hello previous section of this article.</span></span>

1. <span data-ttu-id="875f7-158">Přidat nastavení hlavní služby Azure z předcházející části tohoto článku toohello `<servers>` kolekce v hello *souborech settings.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-158">Add your Azure service principal settings from an earlier section of this article toohello `<servers>` collection in hello *settings.xml* file; for example:</span></span>

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   <span data-ttu-id="875f7-159">Kde:</span><span class="sxs-lookup"><span data-stu-id="875f7-159">Where:</span></span>
   <span data-ttu-id="875f7-160">Element</span><span class="sxs-lookup"><span data-stu-id="875f7-160">Element</span></span> | <span data-ttu-id="875f7-161">Popis</span><span class="sxs-lookup"><span data-stu-id="875f7-161">Description</span></span>
   ---|---|---
   `<id>` | <span data-ttu-id="875f7-162">Určuje jedinečný název, který Maven používá toolook vaše nastavení zabezpečení při nasazení tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="875f7-162">Specifies a unique name which Maven uses toolook up your security settings when you deploy your web app tooAzure.</span></span>
   `<client>` | <span data-ttu-id="875f7-163">Obsahuje hello `appId` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="875f7-163">Contains hello `appId` value from your service principal.</span></span>
   `<tenant>` | <span data-ttu-id="875f7-164">Obsahuje hello `tenant` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="875f7-164">Contains hello `tenant` value from your service principal.</span></span>
   `<key>` | <span data-ttu-id="875f7-165">Obsahuje hello `password` hodnotu z instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="875f7-165">Contains hello `password` value from your service principal.</span></span>
   `<environment>` | <span data-ttu-id="875f7-166">Definuje hello cílové cloudu Azure prostředí, což je `AZURE` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="875f7-166">Defines hello target Azure cloud environment, which is `AZURE` in this example.</span></span> <span data-ttu-id="875f7-167">(Úplný seznam prostředí je k dispozici v hello [Maven modul plug-in pro Azure Web Apps] dokumentace)</span><span class="sxs-lookup"><span data-stu-id="875f7-167">(A full list of environments is available in hello [Maven Plugin for Azure Web Apps] documentation)</span></span>

1. <span data-ttu-id="875f7-168">Uložte a zavřete hello *souborech settings.xml* souboru.</span><span class="sxs-lookup"><span data-stu-id="875f7-168">Save and close hello *settings.xml* file.</span></span>

## <a name="build-your-docker-container-image-and-push-it-tooyour-azure-container-registry"></a><span data-ttu-id="875f7-169">Sestavení vaší Docker kontejneru bitové kopie a poslat ho tooyour kontejner Azure registru</span><span class="sxs-lookup"><span data-stu-id="875f7-169">Build your Docker container image and push it tooyour Azure container registry</span></span>

1. <span data-ttu-id="875f7-170">Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí (např.) "*C:\SpringBoot\gs-spring-boot-docker\complete*"nebo"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") a otevřete hello *pom.xml* soubor s textový editor.</span><span class="sxs-lookup"><span data-stu-id="875f7-170">Navigate toohello completed project directory for your Spring Boot application, (e.g. "*C:\SpringBoot\gs-spring-boot-docker\complete*" or "*/users/robert/SpringBoot/gs-spring-boot-docker/complete*"), and open hello *pom.xml* file with a text editor.</span></span>

1. <span data-ttu-id="875f7-171">Aktualizace hello `<properties>` kolekce v hello *pom.xml* soubor s hodnotou server hello přihlášení pro vaši Azure kontejneru registru z předchozí části hello tohoto kurzu; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-171">Update hello `<properties>` collection in hello *pom.xml* file with hello login server value for your Azure Container Registry from hello previous section of this tutorial; for example:</span></span>

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   <span data-ttu-id="875f7-172">Kde:</span><span class="sxs-lookup"><span data-stu-id="875f7-172">Where:</span></span>
   <span data-ttu-id="875f7-173">Element</span><span class="sxs-lookup"><span data-stu-id="875f7-173">Element</span></span> | <span data-ttu-id="875f7-174">Popis</span><span class="sxs-lookup"><span data-stu-id="875f7-174">Description</span></span>
   ---|---|---
   `<azure.containerRegistry>` | <span data-ttu-id="875f7-175">Určuje název hello vaší privátní kontejner Azure registru.</span><span class="sxs-lookup"><span data-stu-id="875f7-175">Specifies hello name of your private Azure container registry.</span></span>
   `<docker.image.prefix>` | <span data-ttu-id="875f7-176">Určuje adresu URL registru privátní kontejner Azure, které je odvozeno připojením hello ". azurecr.io" toohello název vaší privátní kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="875f7-176">Specifies hello URL of your private Azure container registry, which is derived by appending ".azurecr.io" toohello name of your private container registry.</span></span>

1. <span data-ttu-id="875f7-177">Ověřte, že `<plugin>` pro modul plug-in Docker hello ve vaší *pom.xml* soubor obsahuje hello správné vlastnosti pro hello přihlašovací adresu a registru název serveru hello v předchozím kroku v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="875f7-177">Verify that `<plugin>` for hello Docker plugin in your *pom.xml* file contains hello correct properties for hello login server address and registry name from hello previous step in this tutorial.</span></span> <span data-ttu-id="875f7-178">Například:</span><span class="sxs-lookup"><span data-stu-id="875f7-178">For example:</span></span>

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   <span data-ttu-id="875f7-179">Kde:</span><span class="sxs-lookup"><span data-stu-id="875f7-179">Where:</span></span>
   <span data-ttu-id="875f7-180">Element</span><span class="sxs-lookup"><span data-stu-id="875f7-180">Element</span></span> | <span data-ttu-id="875f7-181">Popis</span><span class="sxs-lookup"><span data-stu-id="875f7-181">Description</span></span>
   ---|---|---
   `<serverId>` | <span data-ttu-id="875f7-182">Určuje hello vlastnost, která obsahuje název vaší privátní kontejner Azure registru.</span><span class="sxs-lookup"><span data-stu-id="875f7-182">Specifies hello property which contains name of your private Azure container registry.</span></span>
   `<registryUrl>` | <span data-ttu-id="875f7-183">Určuje hello vlastnost, která obsahuje adresu URL hello vaší privátní kontejner Azure registru.</span><span class="sxs-lookup"><span data-stu-id="875f7-183">Specifies hello property which contains hello URL of your private Azure container registry.</span></span>

1. <span data-ttu-id="875f7-184">Přejděte toohello dokončit adresáři projektu pro vaši aplikaci pružiny spouštěcí a spusťte následující příkaz toorebuild hello aplikace hello a push hello kontejneru tooyour kontejner Azure registru:</span><span class="sxs-lookup"><span data-stu-id="875f7-184">Navigate toohello completed project directory for your Spring Boot application and run hello following command toorebuild hello application and push hello container tooyour Azure container registry:</span></span>

   ```
   mvn package docker:build -DpushImage 
   ```

1. <span data-ttu-id="875f7-185">Volitelné: Procházet toohello [portál Azure] a ověřte, zda je image kontejner Docker s názvem **gs pružiny spouštěcí docker** v registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="875f7-185">OPTIONAL: Browse toohello [Azure portal] and verify that there is Docker container image named **gs-spring-boot-docker** in your container registry.</span></span>

   ![Ověřte kontejneru na portálu Azure][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-tooazure"></a><span data-ttu-id="875f7-187">Přizpůsobení pom.xml, pak sestavení a nasazení vaší tooAzure kontejneru</span><span class="sxs-lookup"><span data-stu-id="875f7-187">Customize your pom.xml, then build and deploy your container tooAzure</span></span>

<span data-ttu-id="875f7-188">Otevřete hello `pom.xml` souboru pro vaši aplikaci spouštěcí Spring v textovém editoru a najděte hello `<plugin>` element pro `azure-webapp-maven-plugin`.</span><span class="sxs-lookup"><span data-stu-id="875f7-188">Open hello `pom.xml` file for your Spring Boot application in a text editor, and then locate hello `<plugin>` element for `azure-webapp-maven-plugin`.</span></span> <span data-ttu-id="875f7-189">Tento element by měl vypadat hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="875f7-189">This element should resemble hello following example:</span></span>

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

<span data-ttu-id="875f7-190">Existuje několik hodnot, které lze upravit pro modul plug-in hello Maven a podrobný popis pro každou z těchto elementů je k dispozici v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="875f7-190">There are several values that you can modify for hello Maven plugin, and a detailed description for each of these elements is available in hello [Maven Plugin for Azure Web Apps] documentation.</span></span> <span data-ttu-id="875f7-191">Která se ale nutné dodat, existuje několik hodnot, které jsou vhodné zvýraznění v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="875f7-191">That being said, there are several values that are worth highlighting in this article:</span></span>

<span data-ttu-id="875f7-192">Element</span><span class="sxs-lookup"><span data-stu-id="875f7-192">Element</span></span> | <span data-ttu-id="875f7-193">Popis</span><span class="sxs-lookup"><span data-stu-id="875f7-193">Description</span></span>
---|---|---
`<version>` | <span data-ttu-id="875f7-194">Určuje verzi hello hello [Maven modul plug-in pro Azure Web Apps].</span><span class="sxs-lookup"><span data-stu-id="875f7-194">Specifies hello version of hello [Maven Plugin for Azure Web Apps].</span></span> <span data-ttu-id="875f7-195">Měli byste zkontrolovat hello verze uvedené v hello [Maven centrální úložiště](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) hello tooensure, který používáte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="875f7-195">You should check hello version listed in hello [Maven Central Respository](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) tooensure that you are using hello latest version.</span></span>
`<authentication>` | <span data-ttu-id="875f7-196">Určuje hello ověřovací informace pro Azure, který v tomto příkladu obsahuje `<serverId>` elementu, který obsahuje `azure-auth`; Maven používá tuto hodnotu toolook hlavní hodnot hello služba Azure ve vašem Maven *souborech settings.xml* souboru, který jste definovali v předcházející části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="875f7-196">Specifies hello authentication information for Azure, which in this example contains a `<serverId>` element that contains `azure-auth`; Maven uses that value toolook up hello Azure service principal values in your Maven *settings.xml* file, which you defined in an earlier section of this article.</span></span>
`<resourceGroup>` | <span data-ttu-id="875f7-197">Určuje hello cílová skupina prostředků, který je `wingtiptoysresources` v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="875f7-197">Specifies hello target resource group, which is `wingtiptoysresources` in this example.</span></span> <span data-ttu-id="875f7-198">Skupina prostředků Hello se vytvoří během nasazení, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="875f7-198">hello resource group will be created during deployment if it does not already exist.</span></span>
`<appName>` | <span data-ttu-id="875f7-199">Určuje název cílového hello pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="875f7-199">Specifies hello target name for your web app.</span></span> <span data-ttu-id="875f7-200">V tomto příkladu je název cílové hello `maven-linux-app-${maven.build.timestamp}`, kde hello `${maven.build.timestamp}` tímto konfliktem tooavoid příklad se připojí přípona.</span><span class="sxs-lookup"><span data-stu-id="875f7-200">In this example, hello target name is `maven-linux-app-${maven.build.timestamp}`, where hello `${maven.build.timestamp}` suffix is appended in this example tooavoid conflict.</span></span> <span data-ttu-id="875f7-201">(hello časové razítko je volitelný, můžete zadat všechny jedinečné řetězce pro název aplikace hello.)</span><span class="sxs-lookup"><span data-stu-id="875f7-201">(hello timestamp is optional; you can specify any unique string for hello app name.)</span></span>
`<region>` | <span data-ttu-id="875f7-202">Určuje hello cílová oblast, která v tomto příkladu je `westus`.</span><span class="sxs-lookup"><span data-stu-id="875f7-202">Specifies hello target region, which in this example is `westus`.</span></span> <span data-ttu-id="875f7-203">(Úplný seznam je v hello [Maven modul plug-in pro Azure Web Apps] dokumentaci.)</span><span class="sxs-lookup"><span data-stu-id="875f7-203">(A full list is in hello [Maven Plugin for Azure Web Apps] documentation.)</span></span>
`<containerSettings>` | <span data-ttu-id="875f7-204">Určuje hello vlastnosti, které obsahují hello název a adresu URL vašeho kontejneru.</span><span class="sxs-lookup"><span data-stu-id="875f7-204">Specifies hello properties which contain hello name and URL of your container.</span></span>
`<appSettings>` | <span data-ttu-id="875f7-205">Určuje nastavení jedinečné pro Maven toouse při nasazování tooAzure vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="875f7-205">Specifies any unique settings for Maven toouse when deploying your web app tooAzure.</span></span> <span data-ttu-id="875f7-206">V tomto příkladu `<property>` element obsahuje dvojici název – hodnota podřízených elementů, které určují hello port pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="875f7-206">In this example, a `<property>` element contains a name/value pair of child elements that specify hello port for your app.</span></span>

> [!NOTE]
>
> <span data-ttu-id="875f7-207">číslo portu Hello nastavení toochange hello v tomto příkladu jsou nezbytné, pouze pokud měníte hello port z výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="875f7-207">hello settings toochange hello port number in this example are only necessary when you are changing hello port from hello default.</span></span>
>

1. <span data-ttu-id="875f7-208">Z hello příkazový řádek nebo okno terminálu, který jste dříve používali, znovu sestavit pomocí nástroje Maven Pokud jste provedli jakékoli změny toohello soubor JAR hello *pom.xml* souboru; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-208">From hello command prompt or terminal window that you were using earlier, rebuild hello JAR file using Maven if you made any changes toohello *pom.xml* file; for example:</span></span>
   ```shell
   mvn clean package
   ```

1. <span data-ttu-id="875f7-209">Nasadit tooAzure vaší webové aplikace pomocí nástroje Maven; například:</span><span class="sxs-lookup"><span data-stu-id="875f7-209">Deploy your web app tooAzure by using Maven; for example:</span></span>
   ```shell
   mvn azure-webapp:deploy
   ```

<span data-ttu-id="875f7-210">Maven nasadí tooAzure vaší webové aplikace; Pokud webová aplikace hello již neexistuje, bude vytvořen.</span><span class="sxs-lookup"><span data-stu-id="875f7-210">Maven will deploy your web app tooAzure; if hello web app does not already exist, it will be created.</span></span>

> [!NOTE]
>
> <span data-ttu-id="875f7-211">Pokud hello oblast, kterou zadáte v hello `<region>` element vaše *pom.xml* soubor nemá dostatek serverů, které jsou k dispozici po spuštění nasazení, může dojít k chybě toohello podobně jako následující příklad:</span><span class="sxs-lookup"><span data-stu-id="875f7-211">If hello region which you specify in hello `<region>` element of your *pom.xml* file does not have enough servers available when you start your deployment, you might see an error similar toohello following example:</span></span>
>
> ```
> [INFO] Start deploying tooWeb App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed tooexecute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> <span data-ttu-id="875f7-212">V takovém případě můžete zadat že jinou oblast a znovu spusťte hello Maven příkaz toodeploy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="875f7-212">If this happens, you can specify another region and re-run hello Maven command toodeploy your application.</span></span>
>
>

<span data-ttu-id="875f7-213">Pokud váš web nasazen, bude možné toomanage ho pomocí hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="875f7-213">When your web has been deployed, you will be able toomanage it by using hello [Azure portal].</span></span>

* <span data-ttu-id="875f7-214">Webové aplikace se objeví v **App Services**:</span><span class="sxs-lookup"><span data-stu-id="875f7-214">Your web app will be listed in **App Services**:</span></span>

   ![Webové aplikace, které jsou uvedené na portálu Azure App Services][AP01]

* <span data-ttu-id="875f7-216">A hello adresu URL pro webové aplikace se objeví v hello **přehled** pro webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="875f7-216">And hello URL for your web app will be listed in hello **Overview** for your web app:</span></span>

   ![Určení hello adresu URL pro webovou aplikaci][AP02]

## <a name="next-steps"></a><span data-ttu-id="875f7-218">Další kroky</span><span class="sxs-lookup"><span data-stu-id="875f7-218">Next steps</span></span>

<span data-ttu-id="875f7-219">Další informace o hello různých technologií, které jsou popsané v tomto článku najdete hello následující články:</span><span class="sxs-lookup"><span data-stu-id="875f7-219">For more information about hello various technologies discussed in this article, see hello following articles:</span></span>

* <span data-ttu-id="875f7-220">[Maven modul plug-in pro Azure Web Apps]</span><span class="sxs-lookup"><span data-stu-id="875f7-220">[Maven Plugin for Azure Web Apps]</span></span>

* [<span data-ttu-id="875f7-221">Přihlaste se tooAzure z hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="875f7-221">Log in tooAzure from hello Azure CLI</span></span>](/azure/xplat-cli-connect)

* [<span data-ttu-id="875f7-222">Vytvořit objekt služby Azure pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="875f7-222">Create an Azure service principal with Azure CLI 2.0</span></span>](/cli/azure/create-an-azure-service-principal-azure-cli)

* [<span data-ttu-id="875f7-223">Referenční příručka k nastavení maven</span><span class="sxs-lookup"><span data-stu-id="875f7-223">Maven Settings Reference</span></span>](https://maven.apache.org/settings.html)

* <span data-ttu-id="875f7-224">[Modul plug-in docker pro Maven]</span><span class="sxs-lookup"><span data-stu-id="875f7-224">[Docker plugin for Maven]</span></span>

<!-- URL List -->

[rozhraní příkazového řádku Azure (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[portál Azure]: https://portal.azure.com/
[Maven modul plug-in pro Azure Web Apps]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using hello Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[Modul plug-in docker pro Maven]: https://github.com/spotify/docker-maven-plugin
[bezplatný účet Azure]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[výhody pro předplatitele MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[pružiny spouštěcí]: http://projects.spring.io/spring-boot/
[pružiny spouštěcí na Docker Začínáme]: https://github.com/spring-guides/gs-spring-boot-docker
[pružiny Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png

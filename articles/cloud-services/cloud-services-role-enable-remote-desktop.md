---
title: "aaaEnable vzdálené plochy v cloudové službě Azure | Microsoft Docs"
description: "Jak tooconfigure vaše azure cloudové služby aplikace tooallow připojení ke vzdálené ploše"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: d3110ee8-6526-4585-aba5-d0bc9a713e9b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: b3c0180bf5ad29cb77e5303ccbd6f9ccc44b7b0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="fb07b-103">Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="fb07b-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="fb07b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fb07b-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="fb07b-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="fb07b-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="fb07b-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb07b-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="fb07b-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb07b-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)

<span data-ttu-id="fb07b-108">Můžete povolit připojení ke vzdálené ploše v příslušné roli během vývoje začleněním modulů hello vzdálené plochy v definice služby nebo můžete zvolit tooenable vzdálené plochy prostřednictvím hello rozšíření vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="fb07b-108">You can enable a Remote Desktop connection in your role during development by including hello Remote Desktop modules in your service definition or you can choose tooenable Remote Desktop through hello Remote Desktop Extension.</span></span> <span data-ttu-id="fb07b-109">Hello žádoucí je toouse hello vzdálené plochy rozšíření jako Vzdálená plocha můžete povolit i po nasazení aplikace hello bez nutnosti tooredeploy vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="fb07b-109">hello preferred approach is toouse hello Remote Desktop extension as you can enable Remote Desktop even after hello application is deployed without having tooredeploy your application.</span></span>

## <a name="configure-remote-desktop-from-hello-azure-classic-portal"></a><span data-ttu-id="fb07b-110">Konfigurace vzdálené plochy z hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="fb07b-110">Configure Remote Desktop from hello Azure classic portal</span></span>
<span data-ttu-id="fb07b-111">Hello portál Azure classic používá přístup vzdálené plochy rozšíření hello, takže Vzdálená plocha můžete povolit i po nasazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fb07b-111">hello Azure classic portal uses hello Remote Desktop Extension approach so you can enable Remote Desktop even after hello application is deployed.</span></span> <span data-ttu-id="fb07b-112">Hello **konfigurace** stránka pro cloudové služby vám umožní tooenable vzdálené plochy, změna hello místní účet správce používá tooconnect toohello virtuální počítače, hello certifikátu používané v ověřování a nastavte hello Datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="fb07b-112">hello **Configure** page for your cloud service allows you tooenable Remote Desktop, change hello local Administrator account used tooconnect toohello virtual machines, hello certificate used in authentication and set hello expiration date.</span></span>

1. <span data-ttu-id="fb07b-113">Klikněte na tlačítko **cloudové služby**, klikněte na název hello hello cloudové služby a pak klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="fb07b-113">Click **Cloud Services**, click hello name of hello cloud service, and then click **Configure**.</span></span>
2. <span data-ttu-id="fb07b-114">Klikněte na tlačítko hello **vzdáleného** tlačítko dole v hello.</span><span class="sxs-lookup"><span data-stu-id="fb07b-114">Click hello **Remote** button at hello bottom.</span></span>

    ![Cloudové služby vzdálené](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > <span data-ttu-id="fb07b-116">Všechny instance role se restartuje, při prvním povolení služby Vzdálená plocha a klikněte na tlačítko OK (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="fb07b-116">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="fb07b-117">tooprevent restartu hello certifikátů používaných tooencrypt hello heslo musí být nainstalován na hello role.</span><span class="sxs-lookup"><span data-stu-id="fb07b-117">tooprevent a reboot, hello certificate used tooencrypt hello password must be installed on hello role.</span></span> <span data-ttu-id="fb07b-118">tooprevent restartování, [nahrát certifikát pro cloudové služby hello](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) a pak se vraťte toothis dialogu.</span><span class="sxs-lookup"><span data-stu-id="fb07b-118">tooprevent a restart, [upload a certificate for hello cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return toothis dialog.</span></span>

3. <span data-ttu-id="fb07b-119">V **role**, vyberte roli hello tooupdate nebo vyberte **všechny** u všech rolí.</span><span class="sxs-lookup"><span data-stu-id="fb07b-119">In **Roles**, select hello role you want tooupdate or select **All** for all roles.</span></span>
4. <span data-ttu-id="fb07b-120">Proveďte některou z následujících změn hello.</span><span class="sxs-lookup"><span data-stu-id="fb07b-120">Make any of hello following changes:</span></span>

   * <span data-ttu-id="fb07b-121">tooenable vzdálené plochy, vyberte hello **povolení vzdálené plochy** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="fb07b-121">tooenable Remote Desktop, select hello **Enable Remote Desktop** check box.</span></span> <span data-ttu-id="fb07b-122">toodisable vzdálené plochy, hello zrušte zaškrtnutí políčka.</span><span class="sxs-lookup"><span data-stu-id="fb07b-122">toodisable Remote Desktop, clear hello check box.</span></span>
   * <span data-ttu-id="fb07b-123">Vytvořte účtu toouse připojení ke vzdálené ploše toohello instancí rolí.</span><span class="sxs-lookup"><span data-stu-id="fb07b-123">Create an account toouse in Remote Desktop connections toohello role instances.</span></span>
   * <span data-ttu-id="fb07b-124">Aktualizace hesla hello pro existující účet hello.</span><span class="sxs-lookup"><span data-stu-id="fb07b-124">Update hello password for hello existing account.</span></span>
   * <span data-ttu-id="fb07b-125">Vyberte toouse odeslaný certifikát pro ověřování (pomocí certifikátu hello nahrávání **nahrát** na hello **certifikáty** stránky) nebo vytvořit nový certifikát.</span><span class="sxs-lookup"><span data-stu-id="fb07b-125">Select an uploaded certificate toouse for authentication (upload hello certificate using **Upload** on hello **Certificates** page) or create a new certificate.</span></span>
   * <span data-ttu-id="fb07b-126">Změňte datum vypršení platnosti hello hello konfigurace vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="fb07b-126">Change hello expiration date for hello Remote Desktop configuration.</span></span>

5. <span data-ttu-id="fb07b-127">Po dokončení aktualizace vaše konfigurace, klikněte na tlačítko **OK** (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="fb07b-127">When you finish your configuration updates, click **OK** (checkmark).</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="fb07b-128">Vzdálené do instance rolí</span><span class="sxs-lookup"><span data-stu-id="fb07b-128">Remote into role instances</span></span>
<span data-ttu-id="fb07b-129">Jakmile povolíte vzdálené plochy na hello role můžete vzdáleného do instance role pomocí různých nástrojů.</span><span class="sxs-lookup"><span data-stu-id="fb07b-129">Once Remote Desktop is enabled on hello roles you can remote into a role instance through various tools.</span></span>

<span data-ttu-id="fb07b-130">instance role tooconnect tooa z hello portál Azure classic:</span><span class="sxs-lookup"><span data-stu-id="fb07b-130">tooconnect tooa role instance from hello Azure classic portal:</span></span>

1. <span data-ttu-id="fb07b-131">Klikněte na tlačítko **instance** tooopen hello **instance** stránky.</span><span class="sxs-lookup"><span data-stu-id="fb07b-131">Click **Instances** tooopen hello **Instances** page.</span></span>
2. <span data-ttu-id="fb07b-132">Vyberte instanci role, která má nakonfigurované připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="fb07b-132">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="fb07b-133">Klikněte na tlačítko **Connect**a postupujte podle pokynů hello pokyny tooopen hello plochy.</span><span class="sxs-lookup"><span data-stu-id="fb07b-133">Click **Connect**, and follow hello instructions tooopen hello desktop.</span></span>
4. <span data-ttu-id="fb07b-134">Klikněte na tlačítko **otevřete** a potom **připojit** toostart hello připojení vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="fb07b-134">Click **Open** and then **Connect** toostart hello Remote Desktop connection.</span></span>

### <a name="use-visual-studio-tooremote-into-a-role-instance"></a><span data-ttu-id="fb07b-135">Pomocí sady Visual Studio tooremote do role instance</span><span class="sxs-lookup"><span data-stu-id="fb07b-135">Use Visual Studio tooremote into a role instance</span></span>
<span data-ttu-id="fb07b-136">V sadě Visual Studio Průzkumníka serveru:</span><span class="sxs-lookup"><span data-stu-id="fb07b-136">In Visual Studio, Server Explorer:</span></span>

1. <span data-ttu-id="fb07b-137">Rozbalte hello **Azure** > **cloudové služby** > **[název cloudové služby]** uzlu.</span><span class="sxs-lookup"><span data-stu-id="fb07b-137">Expand hello **Azure** > **Cloud Services** > **[cloud service name]** node.</span></span>
2. <span data-ttu-id="fb07b-138">Rozbalte **pracovní** nebo **produkční**.</span><span class="sxs-lookup"><span data-stu-id="fb07b-138">Expand either **Staging** or **Production**.</span></span>
3. <span data-ttu-id="fb07b-139">Rozbalte hello jednotlivé role.</span><span class="sxs-lookup"><span data-stu-id="fb07b-139">Expand hello individual role.</span></span>
4. <span data-ttu-id="fb07b-140">Pravým tlačítkem na jednu instancí role hello, klikněte na tlačítko **připojit pomocí vzdálené plochy...** a pak zadejte hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="fb07b-140">Right-click one of hello role instances, click **Connect using Remote Desktop...**, and then enter hello user name and password.</span></span>

![Vzdálená plocha Průzkumníka serveru](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-tooget-hello-rdp-file"></a><span data-ttu-id="fb07b-142">Použít soubor RDP hello tooget prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb07b-142">Use PowerShell tooget hello RDP file</span></span>
<span data-ttu-id="fb07b-143">Můžete použít hello [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) soubor RDP hello tooretrieve rutiny.</span><span class="sxs-lookup"><span data-stu-id="fb07b-143">You can use hello [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet tooretrieve hello RDP file.</span></span> <span data-ttu-id="fb07b-144">Pak můžete soubor RDP hello s připojení ke vzdálené ploše tooaccess hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="fb07b-144">You can then use hello RDP file with Remote Desktop Connection tooaccess hello cloud service.</span></span>

### <a name="programmatically-download-hello-rdp-file-through-hello-service-management-rest-api"></a><span data-ttu-id="fb07b-145">Prostřednictvím kódu programu stáhněte soubor RDP hello prostřednictvím hello REST API pro správu služby</span><span class="sxs-lookup"><span data-stu-id="fb07b-145">Programmatically download hello RDP file through hello Service Management REST API</span></span>
<span data-ttu-id="fb07b-146">Můžete použít hello [stáhnout soubor RDP](https://msdn.microsoft.com/library/jj157183.aspx) soubor RDP hello toodownload operace REST.</span><span class="sxs-lookup"><span data-stu-id="fb07b-146">You can use hello [Download RDP File](https://msdn.microsoft.com/library/jj157183.aspx) REST operation toodownload hello RDP file.</span></span>

## <a name="tooconfigure-remote-desktop-in-hello-service-definition-file"></a><span data-ttu-id="fb07b-147">tooconfigure vzdálené plochy v souboru definice služby hello</span><span class="sxs-lookup"><span data-stu-id="fb07b-147">tooconfigure Remote Desktop in hello service definition file</span></span>
<span data-ttu-id="fb07b-148">Tato metoda vám umožní tooenable vzdálené plochy pro hello aplikací během vývoje.</span><span class="sxs-lookup"><span data-stu-id="fb07b-148">This method allows you tooenable Remote Desktop for hello application during development.</span></span> <span data-ttu-id="fb07b-149">Tento přístup vyžaduje šifrované hesla ukládat v konfiguraci služby souboru a všechny aktualizace toohello konfigurace vzdálené plochy by vyžadovaly opětovné nasazení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fb07b-149">This approach requires encrypted passwords be stored in your service configuration file and any updates toohello remote desktop configuration would require a redeployment of hello application.</span></span> <span data-ttu-id="fb07b-150">Pokud chcete, aby tooavoid na základě těchto downsides, měli byste použít vzdálené plochy rozšíření hello přístup popsané výše.</span><span class="sxs-lookup"><span data-stu-id="fb07b-150">If you want tooavoid these downsides you should use hello remote desktop extension based approach described above.</span></span>  

<span data-ttu-id="fb07b-151">Visual Studio můžete použít příliš[povolit připojení ke vzdálené ploše](../vs-azure-tools-remote-desktop-roles.md) hello služby definice souboru přístup.</span><span class="sxs-lookup"><span data-stu-id="fb07b-151">You can use Visual Studio too[enable a remote desktop connection](../vs-azure-tools-remote-desktop-roles.md) using hello service definition file approach.</span></span>  
<span data-ttu-id="fb07b-152">níže uvedené kroky Hello popisují hello změny potřebné toohello služby modelu soubory tooenable vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="fb07b-152">hello steps below describe hello changes needed toohello service model files tooenable remote desktop.</span></span> <span data-ttu-id="fb07b-153">Visual Studio se tyto změny automaticky provede při publikování.</span><span class="sxs-lookup"><span data-stu-id="fb07b-153">Visual Studio will automatically makes these changes when publishing.</span></span>

### <a name="set-up-hello-connection-in-hello-service-model"></a><span data-ttu-id="fb07b-154">Nastavit připojení k hello v modelu služby hello</span><span class="sxs-lookup"><span data-stu-id="fb07b-154">Set up hello connection in hello service model</span></span>
<span data-ttu-id="fb07b-155">Použití hello **importy** element tooimport hello **RemoteAccess** modul a hello **RemoteForwarder** modulu toohello [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) souboru.</span><span class="sxs-lookup"><span data-stu-id="fb07b-155">Use hello **Imports** element tooimport hello **RemoteAccess** module and hello **RemoteForwarder** module toohello [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) file.</span></span>

<span data-ttu-id="fb07b-156">Hello souboru definice služby by měl být podobné toohello následující příklad s hello `<Imports>` elementu přidány.</span><span class="sxs-lookup"><span data-stu-id="fb07b-156">hello service definition file should be similar toohello following example with hello `<Imports>` element added.</span></span>

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
<span data-ttu-id="fb07b-157">Hello [souboru ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) soubor by měl být podobné toohello následující ukázka, Všimněte si hello `<ConfigurationSettings>` a `<Certificates>` elementy.</span><span class="sxs-lookup"><span data-stu-id="fb07b-157">hello [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) file should be similar toohello following example, note hello `<ConfigurationSettings>` and `<Certificates>` elements.</span></span> <span data-ttu-id="fb07b-158">Hello zadaný certifikát musí být [nahrán toohello Cloudová služba](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="fb07b-158">hello Certificate specified must be [uploaded toohello cloud service](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a><span data-ttu-id="fb07b-159">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fb07b-159">Additional Resources</span></span>
<span data-ttu-id="fb07b-160">[Jak tooConfigure cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="fb07b-160">[How tooConfigure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>

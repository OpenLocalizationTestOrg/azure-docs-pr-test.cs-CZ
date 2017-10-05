---
title: "Povolení vzdálené plochy v cloudové službě Azure | Microsoft Docs"
description: "Postup konfigurace aplikace služby azure cloud umožňující připojení ke vzdálené ploše"
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
ms.openlocfilehash: 413e72e9a39fcde84f56bfc61a6bc72dbadf1c97
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="7913b-103">Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure</span><span class="sxs-lookup"><span data-stu-id="7913b-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7913b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7913b-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="7913b-105">Portál Azure Classic</span><span class="sxs-lookup"><span data-stu-id="7913b-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="7913b-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7913b-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="7913b-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7913b-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)

<span data-ttu-id="7913b-108">Můžete povolit připojení ke vzdálené ploše v příslušné roli během vývoje zahrnutím moduly vzdálené plochy v definice služby nebo můžete povolit vzdálené plochy prostřednictvím vzdálené plochy rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7913b-108">You can enable a Remote Desktop connection in your role during development by including the Remote Desktop modules in your service definition or you can choose to enable Remote Desktop through the Remote Desktop Extension.</span></span> <span data-ttu-id="7913b-109">Použití rozšíření vzdálené plochy, protože vzdálená plocha můžete povolit i poté, co je aplikace nasazena, aniž by museli znovu nasaďte aplikaci je žádoucí.</span><span class="sxs-lookup"><span data-stu-id="7913b-109">The preferred approach is to use the Remote Desktop extension as you can enable Remote Desktop even after the application is deployed without having to redeploy your application.</span></span>

## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a><span data-ttu-id="7913b-110">Konfigurace vzdálené plochy z portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="7913b-110">Configure Remote Desktop from the Azure classic portal</span></span>
<span data-ttu-id="7913b-111">Portál Azure classic používá vzdálené plochy rozšíření přístup, takže Vzdálená plocha můžete povolit i poté, co je aplikace nasazená.</span><span class="sxs-lookup"><span data-stu-id="7913b-111">The Azure classic portal uses the Remote Desktop Extension approach so you can enable Remote Desktop even after the application is deployed.</span></span> <span data-ttu-id="7913b-112">**Konfigurace** stránka pro cloudové služby umožňuje povolit vzdálené plochy, změňte místní účet správce používá k připojení k virtuálním počítačům, certifikát používané v ověřování a nastavit datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="7913b-112">The **Configure** page for your cloud service allows you to enable Remote Desktop, change the local Administrator account used to connect to the virtual machines, the certificate used in authentication and set the expiration date.</span></span>

1. <span data-ttu-id="7913b-113">Klikněte na tlačítko **cloudové služby**, klikněte na název cloudové služby a pak klikněte na tlačítko **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="7913b-113">Click **Cloud Services**, click the name of the cloud service, and then click **Configure**.</span></span>
2. <span data-ttu-id="7913b-114">Klikněte **vzdáleného** tlačítko dole.</span><span class="sxs-lookup"><span data-stu-id="7913b-114">Click the **Remote** button at the bottom.</span></span>

    ![Cloudové služby vzdálené](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > <span data-ttu-id="7913b-116">Všechny instance role se restartuje, při prvním povolení služby Vzdálená plocha a klikněte na tlačítko OK (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="7913b-116">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="7913b-117">Abyste zabránili restartování, musí být nainstalovaný certifikát použitý k šifrování hesla v roli.</span><span class="sxs-lookup"><span data-stu-id="7913b-117">To prevent a reboot, the certificate used to encrypt the password must be installed on the role.</span></span> <span data-ttu-id="7913b-118">Abyste zabránili restartování, [nahrát certifikát pro cloudové služby](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) a pak se vraťte do tohoto dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7913b-118">To prevent a restart, [upload a certificate for the cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return to this dialog.</span></span>

3. <span data-ttu-id="7913b-119">V **role**, vyberte roli, kterou chcete aktualizovat nebo vyberte **všechny** u všech rolí.</span><span class="sxs-lookup"><span data-stu-id="7913b-119">In **Roles**, select the role you want to update or select **All** for all roles.</span></span>
4. <span data-ttu-id="7913b-120">Proveďte některou z následujících změn:</span><span class="sxs-lookup"><span data-stu-id="7913b-120">Make any of the following changes:</span></span>

   * <span data-ttu-id="7913b-121">Chcete-li povolit vzdálená plocha, vyberte **povolení vzdálené plochy** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="7913b-121">To enable Remote Desktop, select the **Enable Remote Desktop** check box.</span></span> <span data-ttu-id="7913b-122">Zakázání vzdálené plochy, zrušte zaškrtnutí políčka.</span><span class="sxs-lookup"><span data-stu-id="7913b-122">To disable Remote Desktop, clear the check box.</span></span>
   * <span data-ttu-id="7913b-123">Vytvořte účet, který chcete použít v připojení ke vzdálené ploše na instancí role.</span><span class="sxs-lookup"><span data-stu-id="7913b-123">Create an account to use in Remote Desktop connections to the role instances.</span></span>
   * <span data-ttu-id="7913b-124">Aktualizace hesla pro existující účet.</span><span class="sxs-lookup"><span data-stu-id="7913b-124">Update the password for the existing account.</span></span>
   * <span data-ttu-id="7913b-125">Vyberte odeslaný certifikát bude používat pro ověření (nahrát certifikát pomocí **nahrát** na **certifikáty** stránky) nebo vytvořit nový certifikát.</span><span class="sxs-lookup"><span data-stu-id="7913b-125">Select an uploaded certificate to use for authentication (upload the certificate using **Upload** on the **Certificates** page) or create a new certificate.</span></span>
   * <span data-ttu-id="7913b-126">Změňte na datum vypršení platnosti konfigurace vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="7913b-126">Change the expiration date for the Remote Desktop configuration.</span></span>

5. <span data-ttu-id="7913b-127">Po dokončení aktualizace vaše konfigurace, klikněte na tlačítko **OK** (zaškrtnutí).</span><span class="sxs-lookup"><span data-stu-id="7913b-127">When you finish your configuration updates, click **OK** (checkmark).</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="7913b-128">Vzdálené do instance rolí</span><span class="sxs-lookup"><span data-stu-id="7913b-128">Remote into role instances</span></span>
<span data-ttu-id="7913b-129">Jakmile povolíte vzdálené plochy na role můžete vzdálené do instance role pomocí různých nástrojů.</span><span class="sxs-lookup"><span data-stu-id="7913b-129">Once Remote Desktop is enabled on the roles you can remote into a role instance through various tools.</span></span>

<span data-ttu-id="7913b-130">Chcete-li se připojit k instanci role z klasického portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="7913b-130">To connect to a role instance from the Azure classic portal:</span></span>

1. <span data-ttu-id="7913b-131">Klikněte na tlačítko **instance** otevřete **instance** stránky.</span><span class="sxs-lookup"><span data-stu-id="7913b-131">Click **Instances** to open the **Instances** page.</span></span>
2. <span data-ttu-id="7913b-132">Vyberte instanci role, která má nakonfigurované připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="7913b-132">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="7913b-133">Klikněte na tlačítko **Connect**a postupujte podle pokynů na ploše otevřete.</span><span class="sxs-lookup"><span data-stu-id="7913b-133">Click **Connect**, and follow the instructions to open the desktop.</span></span>
4. <span data-ttu-id="7913b-134">Klikněte na tlačítko **otevřete** a potom **Connect** spustit připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="7913b-134">Click **Open** and then **Connect** to start the Remote Desktop connection.</span></span>

### <a name="use-visual-studio-to-remote-into-a-role-instance"></a><span data-ttu-id="7913b-135">Použijte sadu Visual Studio na vzdálený do role instance</span><span class="sxs-lookup"><span data-stu-id="7913b-135">Use Visual Studio to remote into a role instance</span></span>
<span data-ttu-id="7913b-136">V sadě Visual Studio Průzkumníka serveru:</span><span class="sxs-lookup"><span data-stu-id="7913b-136">In Visual Studio, Server Explorer:</span></span>

1. <span data-ttu-id="7913b-137">Rozbalte **Azure** > **cloudové služby** > **[název cloudové služby]** uzlu.</span><span class="sxs-lookup"><span data-stu-id="7913b-137">Expand the **Azure** > **Cloud Services** > **[cloud service name]** node.</span></span>
2. <span data-ttu-id="7913b-138">Rozbalte **pracovní** nebo **produkční**.</span><span class="sxs-lookup"><span data-stu-id="7913b-138">Expand either **Staging** or **Production**.</span></span>
3. <span data-ttu-id="7913b-139">Rozbalte jednotlivé role.</span><span class="sxs-lookup"><span data-stu-id="7913b-139">Expand the individual role.</span></span>
4. <span data-ttu-id="7913b-140">Pravým tlačítkem na jednu z instancí role, klikněte na tlačítko **připojit pomocí vzdálené plochy...** a pak zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="7913b-140">Right-click one of the role instances, click **Connect using Remote Desktop...**, and then enter the user name and password.</span></span>

![Vzdálená plocha Průzkumníka serveru](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-to-get-the-rdp-file"></a><span data-ttu-id="7913b-142">Chcete-li získat soubor RDP pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="7913b-142">Use PowerShell to get the RDP file</span></span>
<span data-ttu-id="7913b-143">Můžete použít [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) rutiny se načíst soubor RDP.</span><span class="sxs-lookup"><span data-stu-id="7913b-143">You can use the [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet to retrieve the RDP file.</span></span> <span data-ttu-id="7913b-144">Pak můžete soubor RDP s připojení ke vzdálené ploše pro přístup ke cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="7913b-144">You can then use the RDP file with Remote Desktop Connection to access the cloud service.</span></span>

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a><span data-ttu-id="7913b-145">Prostřednictvím kódu programu stáhněte soubor RDP prostřednictvím REST API pro správu služby</span><span class="sxs-lookup"><span data-stu-id="7913b-145">Programmatically download the RDP file through the Service Management REST API</span></span>
<span data-ttu-id="7913b-146">Můžete použít [stáhnout soubor RDP](https://msdn.microsoft.com/library/jj157183.aspx) operace REST stáhnout soubor RDP.</span><span class="sxs-lookup"><span data-stu-id="7913b-146">You can use the [Download RDP File](https://msdn.microsoft.com/library/jj157183.aspx) REST operation to download the RDP file.</span></span>

## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a><span data-ttu-id="7913b-147">Konfigurace vzdálené plochy v souboru definice služby</span><span class="sxs-lookup"><span data-stu-id="7913b-147">To configure Remote Desktop in the service definition file</span></span>
<span data-ttu-id="7913b-148">Tato metoda umožňuje povolení vzdálené plochy pro aplikací během vývoje.</span><span class="sxs-lookup"><span data-stu-id="7913b-148">This method allows you to enable Remote Desktop for the application during development.</span></span> <span data-ttu-id="7913b-149">Tento přístup vyžaduje šifrované hesla ukládat v konfiguraci služby souboru a všechny aktualizace konfigurace vzdálené plochy by vyžadovaly opětovné nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="7913b-149">This approach requires encrypted passwords be stored in your service configuration file and any updates to the remote desktop configuration would require a redeployment of the application.</span></span> <span data-ttu-id="7913b-150">Pokud chcete, aby se zabránilo tyto downsides, měli byste použít vzdálené plochy rozšíření na základě popsané výše.</span><span class="sxs-lookup"><span data-stu-id="7913b-150">If you want to avoid these downsides you should use the remote desktop extension based approach described above.</span></span>  

<span data-ttu-id="7913b-151">Můžete použít Visual Studio [povolit připojení ke vzdálené ploše](../vs-azure-tools-remote-desktop-roles.md) přístup souboru definice služby.</span><span class="sxs-lookup"><span data-stu-id="7913b-151">You can use Visual Studio to [enable a remote desktop connection](../vs-azure-tools-remote-desktop-roles.md) using the service definition file approach.</span></span>  
<span data-ttu-id="7913b-152">Níže uvedené kroky popisují změny potřebné pro soubory modelu služby k povolení služby Vzdálená plocha.</span><span class="sxs-lookup"><span data-stu-id="7913b-152">The steps below describe the changes needed to the service model files to enable remote desktop.</span></span> <span data-ttu-id="7913b-153">Visual Studio se tyto změny automaticky provede při publikování.</span><span class="sxs-lookup"><span data-stu-id="7913b-153">Visual Studio will automatically makes these changes when publishing.</span></span>

### <a name="set-up-the-connection-in-the-service-model"></a><span data-ttu-id="7913b-154">Nastavení připojení v modelu služby</span><span class="sxs-lookup"><span data-stu-id="7913b-154">Set up the connection in the service model</span></span>
<span data-ttu-id="7913b-155">Použití **importy** elementu, který chcete importovat **RemoteAccess** modulu a **RemoteForwarder** modulu [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) souboru.</span><span class="sxs-lookup"><span data-stu-id="7913b-155">Use the **Imports** element to import the **RemoteAccess** module and the **RemoteForwarder** module to the [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) file.</span></span>

<span data-ttu-id="7913b-156">Soubor definice služby by mělo být podobné v následujícím příkladu se `<Imports>` elementu přidány.</span><span class="sxs-lookup"><span data-stu-id="7913b-156">The service definition file should be similar to the following example with the `<Imports>` element added.</span></span>

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
<span data-ttu-id="7913b-157">[Souboru ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) soubor by měl být podobně jako v následujícím příkladu, Upozorňujeme `<ConfigurationSettings>` a `<Certificates>` elementy.</span><span class="sxs-lookup"><span data-stu-id="7913b-157">The [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) file should be similar to the following example, note the `<ConfigurationSettings>` and `<Certificates>` elements.</span></span> <span data-ttu-id="7913b-158">Zadaný certifikát musí být [nahrán do cloudové služby](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="7913b-158">The Certificate specified must be [uploaded to the cloud service](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).</span></span>

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


## <a name="additional-resources"></a><span data-ttu-id="7913b-159">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7913b-159">Additional Resources</span></span>
<span data-ttu-id="7913b-160">[Postup konfigurace cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="7913b-160">[How to Configure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>

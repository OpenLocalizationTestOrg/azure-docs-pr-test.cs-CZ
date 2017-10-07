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
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Povolit připojení ke vzdálené ploše pro roli v cloudové služby Azure

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Portál Azure Classic](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

Můžete povolit připojení ke vzdálené ploše v příslušné roli během vývoje začleněním modulů hello vzdálené plochy v definice služby nebo můžete zvolit tooenable vzdálené plochy prostřednictvím hello rozšíření vzdálené plochy. Hello žádoucí je toouse hello vzdálené plochy rozšíření jako Vzdálená plocha můžete povolit i po nasazení aplikace hello bez nutnosti tooredeploy vaší aplikace.

## <a name="configure-remote-desktop-from-hello-azure-classic-portal"></a>Konfigurace vzdálené plochy z hello portál Azure classic
Hello portál Azure classic používá přístup vzdálené plochy rozšíření hello, takže Vzdálená plocha můžete povolit i po nasazení aplikace hello. Hello **konfigurace** stránka pro cloudové služby vám umožní tooenable vzdálené plochy, změna hello místní účet správce používá tooconnect toohello virtuální počítače, hello certifikátu používané v ověřování a nastavte hello Datum vypršení platnosti.

1. Klikněte na tlačítko **cloudové služby**, klikněte na název hello hello cloudové služby a pak klikněte na tlačítko **konfigurace**.
2. Klikněte na tlačítko hello **vzdáleného** tlačítko dole v hello.

    ![Cloudové služby vzdálené](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > Všechny instance role se restartuje, při prvním povolení služby Vzdálená plocha a klikněte na tlačítko OK (zaškrtnutí). tooprevent restartu hello certifikátů používaných tooencrypt hello heslo musí být nainstalován na hello role. tooprevent restartování, [nahrát certifikát pro cloudové služby hello](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) a pak se vraťte toothis dialogu.

3. V **role**, vyberte roli hello tooupdate nebo vyberte **všechny** u všech rolí.
4. Proveďte některou z následujících změn hello.

   * tooenable vzdálené plochy, vyberte hello **povolení vzdálené plochy** zaškrtávací políčko. toodisable vzdálené plochy, hello zrušte zaškrtnutí políčka.
   * Vytvořte účtu toouse připojení ke vzdálené ploše toohello instancí rolí.
   * Aktualizace hesla hello pro existující účet hello.
   * Vyberte toouse odeslaný certifikát pro ověřování (pomocí certifikátu hello nahrávání **nahrát** na hello **certifikáty** stránky) nebo vytvořit nový certifikát.
   * Změňte datum vypršení platnosti hello hello konfigurace vzdálené plochy.

5. Po dokončení aktualizace vaše konfigurace, klikněte na tlačítko **OK** (zaškrtnutí).

## <a name="remote-into-role-instances"></a>Vzdálené do instance rolí
Jakmile povolíte vzdálené plochy na hello role můžete vzdáleného do instance role pomocí různých nástrojů.

instance role tooconnect tooa z hello portál Azure classic:

1. Klikněte na tlačítko **instance** tooopen hello **instance** stránky.
2. Vyberte instanci role, která má nakonfigurované připojení ke vzdálené ploše.
3. Klikněte na tlačítko **Connect**a postupujte podle pokynů hello pokyny tooopen hello plochy.
4. Klikněte na tlačítko **otevřete** a potom **připojit** toostart hello připojení vzdálené plochy.

### <a name="use-visual-studio-tooremote-into-a-role-instance"></a>Pomocí sady Visual Studio tooremote do role instance
V sadě Visual Studio Průzkumníka serveru:

1. Rozbalte hello **Azure** > **cloudové služby** > **[název cloudové služby]** uzlu.
2. Rozbalte **pracovní** nebo **produkční**.
3. Rozbalte hello jednotlivé role.
4. Pravým tlačítkem na jednu instancí role hello, klikněte na tlačítko **připojit pomocí vzdálené plochy...** a pak zadejte hello uživatelské jméno a heslo.

![Vzdálená plocha Průzkumníka serveru](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-tooget-hello-rdp-file"></a>Použít soubor RDP hello tooget prostředí PowerShell
Můžete použít hello [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) soubor RDP hello tooretrieve rutiny. Pak můžete soubor RDP hello s připojení ke vzdálené ploše tooaccess hello cloudové služby.

### <a name="programmatically-download-hello-rdp-file-through-hello-service-management-rest-api"></a>Prostřednictvím kódu programu stáhněte soubor RDP hello prostřednictvím hello REST API pro správu služby
Můžete použít hello [stáhnout soubor RDP](https://msdn.microsoft.com/library/jj157183.aspx) soubor RDP hello toodownload operace REST.

## <a name="tooconfigure-remote-desktop-in-hello-service-definition-file"></a>tooconfigure vzdálené plochy v souboru definice služby hello
Tato metoda vám umožní tooenable vzdálené plochy pro hello aplikací během vývoje. Tento přístup vyžaduje šifrované hesla ukládat v konfiguraci služby souboru a všechny aktualizace toohello konfigurace vzdálené plochy by vyžadovaly opětovné nasazení aplikace hello. Pokud chcete, aby tooavoid na základě těchto downsides, měli byste použít vzdálené plochy rozšíření hello přístup popsané výše.  

Visual Studio můžete použít příliš[povolit připojení ke vzdálené ploše](../vs-azure-tools-remote-desktop-roles.md) hello služby definice souboru přístup.  
níže uvedené kroky Hello popisují hello změny potřebné toohello služby modelu soubory tooenable vzdálené plochy. Visual Studio se tyto změny automaticky provede při publikování.

### <a name="set-up-hello-connection-in-hello-service-model"></a>Nastavit připojení k hello v modelu služby hello
Použití hello **importy** element tooimport hello **RemoteAccess** modul a hello **RemoteForwarder** modulu toohello [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) souboru.

Hello souboru definice služby by měl být podobné toohello následující příklad s hello `<Imports>` elementu přidány.

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
Hello [souboru ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) soubor by měl být podobné toohello následující ukázka, Všimněte si hello `<ConfigurationSettings>` a `<Certificates>` elementy. Hello zadaný certifikát musí být [nahrán toohello Cloudová služba](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

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


## <a name="additional-resources"></a>Další zdroje
[Jak tooConfigure cloudové služby](cloud-services-how-to-configure.md)
[cloudových služeb časté otázky – vzdálené plochy](cloud-services-faq.md)

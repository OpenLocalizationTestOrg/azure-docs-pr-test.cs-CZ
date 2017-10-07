---
title: "aaaAzure nasazení aplikace Service Fabric | Microsoft Docs"
description: "Použijte hello toodeploy FabricClient rozhraní API a odebrat aplikace v Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/07/2017
ms.author: ryanwi
ms.openlocfilehash: b2986b71c461f3e785ba16ec1b827fe47ad852fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a>Nasazení a odebírat aplikace pomocí FabricClient
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [Rozhraní API FabricClient](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Jednou [typu aplikaci vytvořen balíček][10], je připraven k nasazení do clusteru služby Azure Service Fabric. Nasazení zahrnuje hello následující tři kroky:

1. Nahrát úložiště bitových kopií toohello balíčku aplikace hello
2. Registrace typu aplikace hello
3. Vytvoření instance aplikace hello

Jakmile je aplikace nasazená a instance běží v clusteru hello, můžete odstranit hello instanci aplikace a její typ aplikace. toocompletely odebrat aplikaci z clusteru hello zahrnuje hello následující kroky:

1. Odebrat (nebo odstranit) hello spuštěna instance aplikace
2. Zrušení registrace typu aplikace hello, pokud již nepotřebujete
3. Odebrání balíčku aplikace hello z úložiště bitových kopií hello

Pokud používáte [Visual Studio pro nasazování a ladění aplikací](service-fabric-publish-app-remote-cluster.md) na vaše místní vývojový cluster všechny předchozí kroky hello se zpracovávají automaticky pomocí skriptu prostředí PowerShell.  Tento skript se nachází v hello *skripty* složku projekt aplikace hello. Tento článek obsahuje pozadí na co skriptu je to, aby mohli provést hello stejné operace mimo Visual Studio. 
 
## <a name="connect-toohello-cluster"></a>Připojte toohello cluster
Připojte toohello cluster tak, že vytvoříte [FabricClient](/dotnet/api/system.fabric.fabricclient) instance předtím, než spustíte jakoukoli hello příklady kódu v tomto článku. Příklady připojování tooa místního vývojového clusteru nebo vzdálený cluster nebo clusteru zabezpečené pomocí Azure Active Directory, X509 certifikáty nebo služby Windows Active Directory najdete v tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis). tooconnect toohello místního vývojového clusteru, spusťte následující hello:

```csharp
// Connect toohello local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-hello-application-package"></a>Nahrání balíčku aplikace hello
Předpokládejme, že sestavení a balíček aplikace s názvem *Moje_aplikace* v sadě Visual Studio. Ve výchozím nastavení je název typu aplikace hello uvedené v hello ApplicationManifest.xml "MyApplicationType".  Hello balíček aplikace, která obsahuje manifest hello nezbytné aplikace, služby manifesty a balíčků kódu/config/data, se nachází v *C:\Users\&lt; uživatelské jméno&gt;\Documents\Visual Studio 2017\Projects\ MyApplication\MyApplication\pkg\Debug*.

Odesílání balíčku aplikace hello vloží ho do umístění, která je přístupná pomocí hello interní komponenty Service Fabric. Service Fabric ověřuje balíčku aplikace hello během registrace hello balíčku aplikace hello. Pokud však chcete tooverify hello balíčku aplikace místně (tj. před nahráním), použít hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) rutiny.

Hello [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) rozhraní API ukládání balíčku aplikace hello, toohello úložiště bitových kopií clusteru. 

Pokud balíček aplikace hello je velký nebo má mnoho souborů, můžete [je komprimovat](service-fabric-package-apps.md#compress-a-package) a zkopírujte ho toohello image store pomocí prostředí PowerShell. Hello komprese snižuje velikost hello a hello počet souborů.

V tématu [pochopit hello image store připojovací řetězec](service-fabric-image-store-connection-string.md) doplňující informace o hello úložiště image store a image uložit připojovací řetězec.

## <a name="register-hello-application-package"></a>Registrace balíčku aplikace hello
v manifestu aplikace hello k dispozici pro použití při registraci balíčku aplikace hello deklarována Hello typ a verze aplikace. Hello systému přečte nahraný v předchozím kroku hello hello balíček, ověří hello balíčku, zpracuje hello obsah balíčku a zkopíruje hello zpracovat balíček tooan systému interní umístění.  

Hello [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) rozhraní API registry hello typu aplikace v clusteru hello a zpřístupní ji pro nasazení.

Hello [GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) rozhraní API poskytuje informace o všech typech aplikací byl úspěšně zaregistrován. Toodetermine toto rozhraní API můžete použít, pokud se provádí registraci hello.

## <a name="create-an-application-instance"></a>Vytvoření instance aplikace
Můžete vytvořit instanci aplikace z jakéhokoli typu aplikace, který byl úspěšně zaregistrován pomocí hello [CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) rozhraní API. Hello názvu každé aplikace musí začínat hello *"fabric:"* scheme a musí být jedinečný pro každou instanci aplikace (v rámci clusteru). Všechny výchozí služby definované v manifestu aplikace hello typu hello cílové aplikace jsou také vytvořit.

Pro danou verzi typu zaregistrovanou aplikaci lze vytvořit více instancí aplikace. Každá instance aplikace spouští v izolaci, s vlastní pracovní adresář a sadu procesy.

toosee, které s názvem služby a aplikace běží v clusteru hello, spusťte hello [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) a [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) rozhraní API.

## <a name="create-a-service-instance"></a>Vytvoření instance služby
Můžete vytvořit instanci služby z typu služby pomocí hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) rozhraní API.  Pokud služba hello je deklarován jako výchozí služba v manifestu aplikace hello, je vytvořena při vytváření instance aplikace hello instance hello služby.  Volání hello [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) rozhraní API pro službu, která je již vytvořena instance vrátí k výjimce typu FabricException obsahující chybový kód s hodnotou FabricErrorCode.ServiceAlreadyExists.

## <a name="remove-a-service-instance"></a>Odebrat instanci služby
Pokud instance služby již nepotřebujete, můžete jej odebrat z hello spuštěna instance aplikace pomocí volání hello [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) rozhraní API.  

> [!WARNING]
> Tato operace se nedá vrátit a nelze ji obnovit stav služby.

## <a name="remove-an-application-instance"></a>Odebrání instance aplikace
Instance aplikace už je potřeba, trvale odstraníte ho podle názvu pomocí hello [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) rozhraní API. [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) automaticky odstraní všechny služby, které patří toohello aplikace také a trvale odebrat všechny služby stavu.

> [!WARNING]
> Tato operace se nedá vrátit a nelze ji obnovit stav aplikace.

## <a name="unregister-an-application-type"></a>Typ aplikace se zrušit registraci
Pokud konkrétní verzi typ aplikace se už nepotřebuje, by měl zrušit registraci této konkrétní verze typu aplikace hello pomocí hello [Unregister-ServiceFabricApplicationType](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) rozhraní API. Zrušení registrace nepoužívané verze aplikace typy verzích prostoru úložiště používá úložiště bitových kopií hello. Verze typu aplikace může být zrušit registraci, dokud instance žádné aplikace. u této verze typu aplikace hello a nebyl upgradován čekající aplikace odkazují na tuto verzi typ aplikace hello.

## <a name="remove-an-application-package-from-hello-image-store"></a>Balíček aplikace odebrat z úložiště bitových kopií hello
Balíček aplikace už je potřeba, musíte jej odstranit ze hello image store toofree až systémové prostředky pomocí hello [RemoveApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) rozhraní API.

## <a name="troubleshooting"></a>Řešení potíží
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopírování ServiceFabricApplicationPackage požádá o parametr ImageStoreConnectionString
prostředí Service Fabric SDK Hello byste již měli mít hello správný, nastavit výchozí hodnoty. Ale v případě potřeby pro všechny příkazy hello parametr ImageStoreConnectionString musí odpovídat hello hodnotu této hello používá cluster Service Fabric. Hello parametr ImageStoreConnectionString můžete najít v manifestu clusteru hello načten pomocí hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) a Get-ImageStoreConnectionStringFromClusterManifest příkazy:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

Hello **Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell text hello, je použít tooget hello image uložit připojovací řetězec.  tooimport hello SDK modul, spusťte:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


Parametr ImageStoreConnectionString Hello se nachází v manifestu clusteru hello:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

V tématu [pochopit hello image store připojovací řetězec](service-fabric-image-store-connection-string.md) doplňující informace o hello úložiště image store a image uložit připojovací řetězec.

### <a name="deploy-large-application-package"></a>Nasazení balíčku velké aplikace
Problém: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) rozhraní API časového limitu pro balíček rozsáhlé aplikace (pořadí GB).
Vyzkoušejte:
- Zadat delší časový limit pro [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) metody s `timeout` parametr. Ve výchozím nastavení je hello časový limit 30 minut.
- Zkontrolujte hello síťové připojení mezi zdrojovým počítačem a clusteru. Pokud jde o pomalé připojení hello, zvažte použití na počítači s lepší síťové připojení.
Pokud hello klientský počítač je v jiné oblasti než hello clusteru, zvažte použití klientský počítač v oblasti blíže nebo stejné jako hello cluster.
- Zkontrolujte, pokud jste nedosáhli externí omezení. Například v případě je úložiště image hello nakonfigurované toouse azure storage, může nahrávání omezeny.

Problém: Nahrávání balíček úspěšně dokončit, ale [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) rozhraní API časového limitu. Vyzkoušejte:
- [Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitových kopií.
Hello komprese snižuje velikost hello a musíte provést hello počet souborů, která zase snižuje hello objem provozu a že Service Fabric fungovat. operace nahrávání Hello může být pomalejší (obzvláště pokud zahrnete hello komprese čas), ale registrace a zrušení registrace typu aplikace hello je rychlejší.
- Zadat delší časový limit pro [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) rozhraní API pomocí `timeout` parametr.

### <a name="deploy-application-package-with-many-files"></a>Nasazení balíčku aplikace s mnoho souborů
Problém: [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) časového limitu pro balíček aplikace s mnoha souborech (pořadí tisíc).
Vyzkoušejte:
- [Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitových kopií. Hello komprese snižuje hello počet souborů.
- Zadat delší časový limit pro [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) s `timeout` parametr.

## <a name="code-example"></a>Příklad kódu
Hello následující příklad zkopíruje úložišti aplikace toohello balíček bitové kopie, zřídí hello typu aplikace, vytvoří instanci aplikace, vytvoří instanci služby, odebere instanci aplikace hello, zrušení zřizuje hello typ aplikace, a balíček aplikace hello odstraní z úložiště bitových kopií hello.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2017\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
    string serviceType = "Stateless1Type";

    // Connect toohello cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy hello application package tooa location in hello image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied too{0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy tooImage Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision hello application.  "MyApplicationV1" is hello folder in hello image store where hello application package is located. 
    // hello application type with name "MyApplicationType" and version "1.0.0" (both are found in hello application manifest) 
    // is now registered in hello cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create hello application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create hello stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create hello service instance.  If hello service is declared as a default service in hello ApplicationManifest.xml,
    // hello service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from hello application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision hello application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete hello application package from a location in hello image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a>Další kroky
[Upgrade aplikace Service Fabric](service-fabric-application-upgrade.md)

[Úvod stavu Service Fabric](service-fabric-health-introduction.md)

[Diagnostika a řešení služby Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Model aplikace v Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md

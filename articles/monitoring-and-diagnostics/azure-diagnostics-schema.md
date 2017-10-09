---
title: "aaaAzure diagnostiky verze schématu konfigurace rozšíření a historie | Microsoft Docs"
description: "Čítače výkonu relevantní toocollecting ve virtuálních počítačích Azure, škálovatelné sady virtuálních počítačů, Service Fabric a cloudové služby."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/16/2017
ms.author: robb
ms.openlocfilehash: 854ad118f660810aa38703670284794d658142c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-extention-configuration-schema-versions-and-history"></a>Verze rozšíření schéma konfigurace Azure Diagnostics a historie
Tato stránka indexy verze schématu rozšíření Azure Diagnostics dodávána jako součást hello Microsoft Azure SDK.  

> [!NOTE]
> Hello rozšíření diagnostiky Azure je, že součást hello používá toocollect čítače výkonu a další statistiky z:
> - Azure Virtual Machines 
> - Škálovací sady virtuálních počítačů
> - Service Fabric 
> - Cloud Services 
> - Network Security Groups (Skupiny zabezpečení sítě)
> 
> Tato stránka je pouze relevantní, pokud používáte některou z těchto služeb.

Hello rozšíření diagnostiky Azure se používá s dalšími produkty Microsoftu diagnostiky jako monitorování Azure, Application Insights a analýzy protokolů. Další informace najdete v části [přehled nástrojů pro monitorování Microsoft](monitoring-overview.md).

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a>Azure SDK a Diagnostika verze přesouvání grafu  

|Azure SDK verze | Verze rozšíření diagnostiky | Model|  
|------------------|-------------------------------|------|  
|1.x               |1.0                            |modul plug-in|  
|2.0 - 2.4         |1.0                            |modul plug-in|  
|2.5               |1.2                            |Rozšíření|  
|2.6               |1.3                            |"|  
|2.7               |1.4                            |"|  
|2.8               |1.5                            |"|  
|2.9               |1.6                            |"|
|2.96              |1.7                            |"|
|2.96              |1.8                            |"|
|2.96              |1.8.1                          |"|
|2.96              |1.9                            |"|



 Azure Diagnostics verze 1.0 dodávané nejprve v modulu plug-in model – což znamená, že při instalaci sady Azure SDK hello jste získali hello verzi Azure diagnostics dodávané s ním.  

 Od verze 2.5 SDK (diagnostics verze 1.2), Azure diagnostics došlo k tooan rozšíření modelu. Hello nástroje tooutilize nové funkce by byly jenom k dispozici v novější SDK služby Azure, ale žádné služby pomocí nástroje Azure diagnostics by vyzvedávat nejnovější verzi přenosů hello přímo z Azure. Například každý, kdo stále pomocí sady SDK, 2.5 by být načítání hello nejnovější verze uvedené v předchozí tabulce hello, bez ohledu na to používajících hello novější funkce.  

## <a name="schemas-index"></a>Index schémat.  
Různé verze diagnostiky Azure používají schémata jinou konfiguraci. 

[Schéma konfigurace diagnostiky 1.0](azure-diagnostics-schema-1dot0.md)  

[Schéma konfigurace diagnostiky 1.2](azure-diagnostics-schema-1dot2.md)  

[Diagnostika 1.3 a novější schéma konfigurace](azure-diagnostics-schema-1dot3-and-later.md)  

## <a name="version-history"></a>Historie verzí


### <a name="diagnostics-extension-19"></a>Rozšíření diagnostiky 1.9 
Byla přidána podpora Docker.


### <a name="diagnostics-extension-181"></a>Rozšíření diagnostiky 1.8.1 
Můžete zadat token SAS místo klíč účtu úložiště v privátní konfigurace hello. Pokud je zadaný SAS token, klíč účtu úložiště hello se ignoruje.


```json
{
    "storageAccountName": "diagstorageaccount",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    }
}
```

```xml
<PrivateConfig>
    <StorageAccount name="diagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    <SecondaryStorageAccounts>
        <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
</PrivateConfig>
```


### <a name="diagnostics-extension-18"></a>Rozšíření diagnostiky 1.8 
Přidání tooPublicConfig typ úložiště. Může být StorageType *tabulky*, *Blob*, *TableAndBlob*. *Tabulka* je výchozí hello.


```json
{
    "WadCfg": {
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```xml
<PublicConfig>
    <WadCfg />
    <StorageAccount>diagstorageaccount</StorageAccount>
    <StorageType>TableAndBlob</StorageType>
</PublicConfig>
```


### <a name="diagnostics-extension-17"></a>Rozšíření diagnostiky 1.7 
Přidání hello možnost tooroute tooEventHub.

### <a name="diagnostics-extension-15"></a>Rozšíření diagnostiky 1.5
Přidat hello jímky elementu a hello možnost toosend diagnostická data příliš[Application Insights](../application-insights/app-insights-cloudservices.md) díky tomu jednodušší toodiagnose problémy mezi vaší aplikace, jakož i úroveň hello systému a infrastruktury.

### <a name="azure-sdk-26-and-diagnostics-extension-13"></a>Azure SDK 2.6 a Diagnostika extension 1.3 
Pro cloudové služby projekty v sadě Visual Studio byly provedeny následující změny hello. (Tyto změny také použita toolater verze sady SDK pro Azure.)

* místní emulátoru Hello nyní podporuje diagnostiku. To znamená, že můžete shromažďovat diagnostická data a ujistěte se, že aplikace je vytvoření hello správné trasování při jste vývoj a testování v sadě Visual Studio. Hello připojovací řetězec `UseDevelopmentStorage=true` umožňuje shromažďování dat diagnostiky, když spouštíte projekt cloudové služby v sadě Visual Studio pomocí emulátoru úložiště Azure hello. Všechny diagnostická data se shromažďují v účtu úložiště hello (vývoj pro úložiště).
* Hello diagnostiky úložiště připojovací řetězce k účtu (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) je znovu uložené v souboru definice hello služby (.csdef). V Azure SDK 2.5 nebyl zadán účet úložiště diagnostiky hello v souboru diagnostics.wadcfgx hello.

Existují některé významné rozdíly mezi jak šlo hello připojovací řetězec v Azure SDK 2.4 a starší a jak to funguje v Azure SDK 2.6 nebo novější.

* Azure SDK 2.4 a starších verzích používal hello připojovací řetězec jako modul runtime hello diagnostiky modul plug-in tooget hello informace o účtu úložiště pro přenos diagnostické protokoly.
* Ve verzi 2.6 Azure SDK a později je rozšíření diagnostiky hello tooconfigure sady Visual Studio s informace o účtu úložiště odpovídající hello během publikování používají hello diagnostiky připojovací řetězec. připojovací řetězec Hello umožňuje definovat jiným účtům úložiště pro různé služby konfigurace, které Visual Studio budou používat při publikování. Ale vzhledem k tomu, že modul plug-in diagnostiky hello již není k dispozici (po Azure SDK 2.5), nelze povolit souboru .cscfg hello samostatně hello rozšíření diagnostiky. Máte tooenable hello rozšíření samostatně prostřednictvím nástrojů, jako je Visual Studio nebo prostředí PowerShell.
* toosimplify hello proces konfigurace rozšíření diagnostiky hello v prostředí PowerShell, výstup hello balíčku ze sady Visual Studio také obsahuje hello veřejné konfigurace XML pro hello rozšíření diagnostiky pro každou roli. Visual Studio použije hello diagnostiky připojovací řetězec toopopulate hello informace o účtu úložiště v konfiguraci veřejného hello. Hello veřejné konfigurační soubory jsou vytvořeny ve složce rozšíření hello a postupujte podle vzoru hello PaaSDiagnostics. <RoleName>. PubConfig.xml. Všechna nasazení na základě prostředí PowerShell můžete použít tento vzor toomap každý tooa konfigurace Role.
* Hello připojovací řetězec v souboru .cscfg hello používá také hello Azure portálu tooaccess hello diagnostická data, může se objevit v hello **monitorování** kartě hello připojovací řetězec je potřebné tooconfigure hello služby tooshow verbose data monitorování hello portálu.

#### <a name="migrating-projects-tooazure-sdk-26-and-later"></a>Migrace projekty tooAzure SDK 2.6 a novější
Při migraci z Azure SDK 2.5 tooAzure SDK 2.6 nebo novější, pokud jste měli účet úložiště diagnostiky zadané v souboru .wadcfgx hello, pak zůstane existuje. tootake výhod hello flexibilitu pomocí jiného úložiště účtů pro jiné úložiště, budete mít toomanually přidat hello připojovací řetězec tooyour projekt. Pokud jste migraci projektu z Azure SDK 2.4 nebo starší tooAzure SDK 2.6, pak hello diagnostiky, které se zachovají připojovací řetězce. Upozorňujeme však hello změny v tom, jak jsou připojovací řetězce považovány ve verzi 2.6 Azure SDK uvedeného v předchozí části hello.

#### <a name="how-visual-studio-determines-hello-diagnostics-storage-account"></a>Určuje, jak Visual Studio účet úložiště diagnostiky hello
* Pokud připojovací řetězec diagnostiky je zadané v souboru .cscfg hello, Visual Studio použije ji rozšíření diagnostiky hello tooconfigure při publikování a při generování souborů xml hello veřejné konfigurace během balení.
* Pokud žádný diagnostiky připojovací řetězec je zadané v souboru .cscfg hello, pak Visual Studio spadne zpět toousing hello úložiště účet zadaný v hello .wadcfgx tooconfigure hello diagnostiky příponu souboru při publikování a generování veřejného hello soubory xml konfigurace při balení.
* Hello diagnostiky připojovací řetězec v souboru .cscfg hello má přednost před hello účet úložiště v souboru .wadcfgx hello. Pokud diagnostiky připojovací řetězec zadaný v souboru .cscfg hello, pak Visual Studio použije tento a ignoruje hello účet úložiště v .wadcfgx.

#### <a name="what-does-hello-update-development-storage-connection-strings-checkbox-do"></a>Co hello "Aktualizovat připojovací řetězce s vývoj úložiště..." zaškrtávací políčko dělat?
Hello zaškrtávací políčko pro **aktualizace vývoj úložiště připojovací řetězce pro diagnostiku a ukládání do mezipaměti pomocí přihlašovacích údajů účtu úložiště Microsoft Azure při publikování tooMicrosoft Azure** nabízí pohodlný způsob tooupdate všechny vývoj úložiště připojovací řetězce k účtu s účtem úložiště Azure hello zadaný během publikování.

Předpokládejme například, zaškrtněte toto políčko a hello diagnostiky připojovací řetězec Určuje `UseDevelopmentStorage=true`. Při publikování tooAzure hello projektu sady Visual Studio automaticky aktualizovat hello diagnostiky připojovací řetězec s hello účet úložiště, který jste zadali v Průvodci publikovat hello. Pokud účet úložiště skutečné byl zadán jako hello diagnostiky připojovací řetězec, pak tento účet se ale používá místo.

### <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostické funkce rozdíly mezi Azure SDK 2.4 a starší a Azure SDK, 2.5 nebo novější
Pokud upgradujete projekt z Azure SDK 2.4 tooAzure SDK, 2.5 nebo novější, byste měli mít na paměti hello následující rozdíly funkce diagnostiky.

* **Rozhraní API pro konfiguraci jsou zastaralé** – Programová konfigurace diagnostiky je k dispozici v Azure SDK 2.4 nebo starší verze, ale je zastaralý v Azure SDK 2.5 nebo novější. Pokud vaše konfigurace diagnostiky je aktuálně definována v kódu, budete potřebovat tooreconfigure tato nastavení ručně v hello migrované projektu, pro práci tookeep diagnostiky. Hello diagnostiky konfiguračního souboru pro Azure SDK 2.4 je diagnostics.wadcfg a diagnostics.wadcfgx pro Azure SDK 2.5 nebo novější.
* **Diagnostika pro cloudové aplikace, služby se dá nakonfigurovat jenom na úrovni role hello, nikoliv na úrovni instance hello.**
* **Pokaždé, když nasadíte aplikaci, konfiguraci diagnostiky hello je aktualizovat** – to může způsobit problémy parita, pokud změníte konfiguraci diagnostiky z Průzkumníka serveru a pak znovu nasadit aplikace.
* **V Azure SDK 2.5 nebo novější, jsou nakonfigurované výpisy stavu systému v souboru konfigurace diagnostiky hello, není v kódu** – Pokud máte nakonfigurované v kódu výpisy stavu systému, budete mít toomanually přenos hello konfigurace z kódu toohello konfiguračního souboru protože výpisů stavu systému hello nejsou přenesených během migrace tooAzure hello SDK 2.6.


---
title: aaaPublish WebApplicationVM | Microsoft Docs
description: "Zjistěte, jak toodeploy webové aplikace tooa virtuálního počítače. Tento skript vytvoří hello požadované prostředky ve vašem předplatném Azure, pokud ještě neexistují."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: e4b52b620bebf44b87ddfc3b19c155bb65111814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publikování-WebApplicationVM (skript prostředí Windows PowerShell)
Nasadí webové aplikace tooa virtuálního počítače. skript Hello hello požadované prostředky vytvoří ve vašem předplatném Azure, pokud ještě neexistují.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Konfigurace
Hello cesta toohello JSON konfigurační soubor, který popisuje podrobnosti hello hello nasazení.

| Aliasy | None |
| --- | --- |
| Povinné? |Hodnota TRUE |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="subscriptionname"></a>Název_předplatného
Název Hello hello předplatné Azure, ve kterém chcete toocreate hello virtuálního počítače.

| Aliasy | None |
| --- | --- |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |Použije první předplatné hello v soubor odběru hello |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="webdeploypackage"></a>WebDeployPackage
Hello cesta toohello webové nasazení balíčku toopublish toohello virtuálního počítače. Tento balíček můžete vytvořit pomocí Průvodce publikování webu hello v sadě Visual Studio. V tématu [postupy: vytvoření balíčku pro nasazení webu v sadě Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

| Aliasy | None |
| --- | --- |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="allowuntrusted"></a>AllowUntrusted
V případě hodnoty true, povolí hello používání certifikátů, které nejsou podepsána důvěryhodnou kořenovou autoritou.

| Aliasy | None |
| --- | --- |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |False |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="vmpassword"></a>VMPassword
Hello pověření pro účet hello virtuálního počítače. Příklad: - VMPassword @{Name = "admin"; Heslo = "password"}

| Aliasy | None |
| --- | --- |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
Hello přihlašovací údaje pro databázi SQL hello v Azure. Příklad: - DatabaseServerPassword @{Name = "admin"; Heslo = "password"}

| Aliasy | None |
| --- | --- |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |None |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
V případě hodnoty true tiskové zprávy z hello skriptu toohello výstupního datového proudu.

| Aliasy | None |
| --- | --- |
| Povinné? |False |
| Pozice |S názvem |
| Výchozí hodnota |False |
| Přijímat kanálu vstup? |False |
| Přijímat zástupné znaky? |False |

## <a name="remarks"></a>Poznámky
Podrobné informace o tom, jak toouse hello skriptu toocreate vývojářů a testovací prostředí, najdete v části [tooDev tooPublish pomocí skriptů Windows PowerShell a testovací prostředí](vs-azure-tools-publishing-using-powershell-scripts.md).

konfigurační soubor JSON Hello Určuje podrobnosti hello co je toobe nasazení. Obsahuje hello informace, které jste zadali při vytváření projektu hello, jako je například hello název, skupinu vztahů, image virtuálního pevného disku a velikost hello virtuálního počítače. Také zahrnuje hello koncových bodů na hello virtuálního počítače, tooprovision databáze hello, pokud existuje a webové parametrů nasazení. Hello následující kód ukazuje konfigurační soubor JSON příklad:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Můžete upravit hello JSON konfigurační soubor toochange co je zřízený. Virtuálního počítače a cloudové služby jsou povinné, ale část databáze hello je volitelné.


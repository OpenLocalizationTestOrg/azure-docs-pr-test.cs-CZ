---
title: "aaaSecure a cluster se systémem Windows pomocí zabezpečení systému Windows | Microsoft Docs"
description: "Zjistěte, jak tooconfigure-uzly a klientský uzel zabezpečení v samostatné cluster se systémem Windows pomocí zabezpečení systému Windows."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 44f3011eb630357f342052a48d6c852b17dccec4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Zabezpečení samostatné clusteru v systému Windows pomocí zabezpečení systému Windows
tooprevent Neautorizováno cluster Service Fabric tooa přístup, je nutné zabezpečit hello clusteru. Zabezpečení je zvlášť důležité při hello cluster spouští úlohy v produkčním prostředí. Tento článek popisuje, jak tooconfigure-uzly a klientský uzel zabezpečení pomocí zabezpečení systému Windows v hello *souboru ClusterConfig.JSON* souboru.  Hello procesu odpovídá toohello konfigurace zabezpečení krok [vytvořit samostatnou clusteru se systémem Windows](service-fabric-cluster-creation-for-windows-server.md). Další informace o tom, jak Service Fabric používá zabezpečení systému Windows najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).

> [!NOTE]
> Výběr hello – uzly zabezpečení byste měli zvážit pečlivě protože neexistuje žádný upgrade clusteru z jednoho tooanother volbu zabezpečení. Výběr zabezpečení hello toochange, máte toorebuild hello úplné clusteru.
>
>

## <a name="configure-windows-security-using-gmsa"></a>Konfigurace zabezpečení systému Windows pomocí gMSA  
Ukázka Hello *ClusterConfig.gMSA.Windows.MultiMachine.JSON* konfigurační soubor stažené společně se sadou hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) samostatný cluster balíček obsahuje šablony pro konfiguraci zabezpečení systému Windows pomocí [skupinový účet spravované služby (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```  
"security": {  
            "WindowsIdentities": {  
                "ClustergMSAIdentity": "accountname@fqdn"  
                "ClusterSPN": "fqdn"  
                "ClientIdentities": [  
                    {  
                        "Identity": "domain\\username",  
                        "IsAdmin": true  
                    }  
                ]  
            }  
        }  
```  
  
| **Nastavení konfigurace** | **Popis** |  
| --- | --- |  
| WindowsIdentities |Obsahuje identity hello clusteru a klientů. |  
| ClustergMSAIdentity |Nakonfiguruje zabezpečení mezi uzly. Skupinu účet spravované služby. |  
| ClusterSPN |Plně kvalifikované domény hlavního názvu služby pro účet gMSA|  
| ClientIdentities |Nakonfiguruje klienta uzel zabezpečení. Pole klienta uživatelské účty. |  
| Identita |identita klienta Hello, uživatel domény. |  
| IsAdmin |Hodnota TRUE Určuje, že tento uživatel domény hello nemá přístup správce klienta, hodnotu false pro klientský přístup uživatele. |  
  
[Uzel toonode zabezpečení](service-fabric-cluster-security.md#node-to-node-security) je nakonfigurované nastavením **ClustergMSAIdentity** když service fabric potřebuje toorun pod gMSA. V pořadí toobuild vztahy důvěryhodnosti mezi uzly se musí být provedeny vědět navzájem. Můžete to provést dvěma způsoby: hello skupinový účet spravované služby zahrnující všechny uzly v clusteru hello nebo zadejte skupinu hello domény počítače, který obsahuje všechny uzly v clusteru hello. Důrazně doporučujeme používat hello [skupinový účet spravované služby (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) přístup, zejména pro větší clustery (víc než 10 uzlů) nebo pro clustery, které jsou pravděpodobně toogrow nebo zmenšit.  
Tento přístup nevyžaduje hello vytvoření skupiny domény, pro který byla udělena práva tooadd přístup Správce clusterů a odeberte členy. Tyto účty jsou také užitečná pro automatickou správu hesel. Další informace najdete v tématu [Začínáme s skupinové účty spravované služby](http://technet.microsoft.com/library/jj128431.aspx).  
 
[Zabezpečení klienta toonode](service-fabric-cluster-security.md#client-to-node-security) je konfigurován pomocí **ClientIdentities**. Pořadí tooestablish vztah důvěryhodnosti mezi klientem a hello clusteru musíte nakonfigurovat hello clusteru tooknow které identity klienta, které můžete důvěřovat. To můžete provést dvěma způsoby: Zadejte hello skupinu uživatele domény, které můžete připojit nebo zadat hello uzlu uživatele domény, které se můžou připojit. Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, které jsou připojené tooa cluster Service Fabric: správce a uživatele. Řízení přístupu poskytuje schopnost hello hello toolimit Správce clusteru přístup toocertain typy operací clusteru pro různé skupiny uživatelů, lepší zabezpečení hello clusteru.  Správci mají plný přístup toomanagement možnosti (včetně možnosti pro čtení i zápis). Uživatelé, ve výchozím nastavení, mají pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb. Další informace o řízení přístupu najdete v tématu [řízení přístupu podle rolí pro Service Fabric klienty](service-fabric-cluster-security-roles.md).  
 
v následujícím příkladu Hello **zabezpečení** části nakonfiguruje zabezpečení systému Windows pomocí gMSA a určuje, že hello počítačů v *ServiceFabric.clusterA.contoso.com* gMSA jsou součástí clusteru hello a že *CONTOSO\usera* má klient přístup správce:  
  
```  
"security": {  
    "WindowsIdentities": {  
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",  
        "ClusterSPN" : "clusterA.contoso.com",  
        "ClientIdentities": [{  
            "Identity": "CONTOSO\\usera",  
            "IsAdmin": true  
        }]  
    }  
}  
```  
  
## <a name="configure-windows-security-using-a-machine-group"></a>Konfigurace zabezpečení systému Windows ve skupině počítač  
Ukázka Hello *ClusterConfig.Windows.MultiMachine.JSON* konfigurační soubor stažené společně se sadou hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) samostatný cluster balíček obsahuje šablony pro konfiguraci zabezpečení systému Windows.  Zabezpečení systému Windows je nakonfigurovaný v hello **vlastnosti** části: 

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
                "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

| **Nastavení konfigurace** | **Popis** |
| --- | --- |
| ClusterCredentialType |**ClusterCredentialType** je nastaven příliš*Windows* Pokud ClusterIdentity Určuje název skupiny počítače Active Directory. |  
| ServerCredentialType |Nastavení příliš*Windows* tooenable zabezpečení systému Windows pro klienty.<br /><br />To znamená, že klienti hello hello clusteru a samotného clusteru hello běží v rámci domény služby Active Directory. |  
| WindowsIdentities |Obsahuje identity hello clusteru a klientů. |  
| ClusterIdentity |Použijte název skupiny počítače, domain\machinegroup, tooconfigure-uzly zabezpečení. |  
| ClientIdentities |Nakonfiguruje klienta uzel zabezpečení. Pole klienta uživatelské účty. |  
| Identita |Přidejte uživatele domény hello, doména\uživatelské jméno pro identitu klienta hello. |  
| IsAdmin |Toospecify tootrue sadu, která hello uživatele domény nemá přístup správce klienta, nebo hodnotu NEPRAVDA pro klientský přístup uživatele. |  

[Uzel toonode zabezpečení](service-fabric-cluster-security.md#node-to-node-security) se konfiguruje pomocí nastavení **ClusterIdentity** Pokud chcete, aby toouse skupinu počítače v doméně služby Active Directory. Další informace najdete v tématu [vytvořit skupinu počítače ve službě Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).

[Uzel Klient zabezpečení](service-fabric-cluster-security.md#client-to-node-security) se konfiguruje pomocí **ClientIdentities**. tooestablish vztah důvěryhodnosti mezi klientem a hello clusteru, je nutné nakonfigurovat hello clusteru tooknow hello klienta, které můžete důvěřovat identity, které hello clusteru. Můžete vytvořit vztah důvěryhodnosti v dvěma různými způsoby:

- Zadejte hello skupinu uživatele domény, které se můžou připojit.
- Zadejte hello uzlu uživatele domény, které se můžou připojit.

Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, které jsou připojené tooa cluster Service Fabric: správce a uživatele. Řízení přístupu umožňuje hello clusteru správce toolimit přístup toocertain typy operací clusteru pro různé skupiny uživatelů, takže hello clusteru bezpečnější.  Správci mají plný přístup toomanagement možnosti (včetně možnosti pro čtení i zápis). Uživatelé, ve výchozím nastavení, mají pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb.  

Následující příklad Hello **zabezpečení** části nakonfiguruje zabezpečení systému Windows, určuje, že hello počítačů v *ServiceFabric/clusterA.contoso.com* jsou součástí clusteru hello a určuje, že  *CONTOSO\usera* má klient přístup správce:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric nesmí být nasazen na řadiči domény. Ujistěte se, že souboru ClusterConfig.json zahrnovat hello IP adresu řadiče domény hello při použití skupiny počítačů nebo skupinový účet spravované služby (gMSA).
>
>

## <a name="next-steps"></a>Další kroky
Po dokončení konfigurace zabezpečení systému Windows v hello *souboru ClusterConfig.JSON* souboru, pokračovat v procesu vytváření clusteru hello v [vytvořit samostatnou clusteru se systémem Windows](service-fabric-cluster-creation-for-windows-server.md).

Další informace o tom, jak-uzly zabezpečení, klientský uzel zabezpečení a řízení přístupu na základě rolí, najdete v tématu [clusteru scénáře zabezpečení](service-fabric-cluster-security.md).

V tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md) příklady připojení pomocí prostředí PowerShell nebo FabricClient.

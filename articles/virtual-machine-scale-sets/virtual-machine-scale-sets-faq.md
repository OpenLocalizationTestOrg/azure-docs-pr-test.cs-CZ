---
title: "Nejčastější dotazy k sadách škálování virtuálních počítačů aaaAzure | Microsoft Docs"
description: "Získejte odpovědi toofrequently kladené dotazy týkající se sady škálování virtuálního počítače."
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: negat
ms.custom: na
ms.openlocfilehash: 0deb9e2bb79f87f17bbf748397b94dc53070cfbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a>Nejčastější dotazy k sadách škálování virtuálních počítačů Azure

Získejte odpovědi nastaví toofrequently kladené dotazy týkající se škálování virtuálních počítačů v Azure.

## <a name="autoscale"></a>Automatické škálování

### <a name="what-are-best-practices-for-azure-autoscale"></a>Jaké jsou doporučené postupy pro škálování Azure?

Osvědčené postupy pro škálování, najdete v části [osvědčené postupy pro automatické škálování virtuálních počítačů](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a>Kde metriky názvy pro automatické škálování, který používá metriky na hostiteli

Metriky názvy pro automatické škálování, který používá metriky na hostiteli, najdete v části [podporované metriky s Azure monitorování](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a>Existují jakékoli příklady automatické škálování podle o délce téma a fronty Azure Service Bus?

Ano. Příklady automatické škálování podle o délce téma a fronty Azure Service Bus najdete v tématu [běžné metriky automatického škálování Azure monitorování](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).

Pro fronty Service Bus použijte následující JSON hello:

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

Pro frontu úložiště použijte následující JSON hello:

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

Nahraďte ukázkové hodnoty prostředku identifikátory Uniform Resource (Identifier).


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a>Měli I škálování pomocí metriky na hostiteli nebo rozšíření diagnostiky?

Nastavení automatického škálování můžete vytvořit na metriky úrovni hostitele virtuálních počítačů toouse nebo metriky na základě operačního systému hosta.

Seznam podporovaných metriky, naleznete v části [běžné metriky automatického škálování Azure monitorování](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics). 

Úplný příklad pro sady škálování virtuálního počítače, naleznete v tématu [pokročilé škálování konfigurace pomocí šablony Resource Manageru pro sady škálování virtuálního počítače](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets). 

Ukázka Hello používá metrika procesoru hello úrovni hostitele a metriky počet zpráv.



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a>Jak nastavit pravidla výstrah na škálovací sadu virtuálních počítačů?

Výstrahy můžete vytvořit na metriky pro sady škálování virtuálního počítače pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure. Další informace najdete v tématu [Azure PowerShell monitorování rychlý start ukázky](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) a [Azure monitorování napříč platformami CLI rychlý start ukázky](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).

Hello TargetResourceId škálovací sadu virtuálních počítačů hello vypadá takto: 

/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.COMPUTE/virtualMachineScaleSets/yourvmssname

Všechny čítače výkonu virtuálních počítačů můžete vybrat jako metriky tooset hello upozornění. Další informace najdete v tématu [metriky hostovaného operačního systému pro virtuální počítače na bázi správce prostředků Windows](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) a [metriky hostovaného operačního systému pro virtuální počítače s Linuxem](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) v hello [běžné metriky automatického škálování Azure monitorování](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/)článku.

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a>Jak mám nastavit automatické škálování podle škálování virtuálních počítačů, nastavit pomocí prostředí PowerShell?

tooset až automatické škálování podle škálování virtuálních počítačů, nastavit pomocí prostředí PowerShell, najdete v příspěvku blogu hello [jak nastavit automatické škálování tooan tooadd škálování virtuálního počítače Azure](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).




## <a name="certificates"></a>Certifikáty

### <a name="how-do-i-securely-ship-a-certificate-toohello-vm-how-do-i-provision-a-virtual-machine-scale-set-toorun-a-website-where-hello-ssl-for-hello-website-is-shipped-securely-from-a-certificate-configuration-hello-common-certificate-rotation-operation-would-be-almost-hello-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-toodo-this"></a>Jak můžu bezpečně dodávat certifikát toohello virtuálního počítače? Jak můžu zřídit toorun sady škálování virtuálního počítače web hello SSL pro web hello je dodán bezpečně z konfigurace certifikátu? (hello běžné operace certifikátu otočení by být téměř hello stejné jako operace aktualizace konfigurace.) Máte příklad toodo to? 

toosecurely dodávat certifikát toohello virtuálních počítačů, může nainstalovat certifikát zákazníka přímo do úložiště certifikátů systému Windows z trezoru klíčů hello zákazníka.

Použijte následující JSON hello:

```json
"secrets": [
    {
        "sourceVault": {
            "id": "/subscriptions/{subscriptionid}/resourceGroups/myrg1/providers/Microsoft.KeyVault/vaults/mykeyvault1"
        },
        "vaultCertificates": [
            {
                "certificateUrl": "https://mykeyvault1.vault.azure.net/secrets/{secretname}/{secret-version}",
                "certificateStore": "certificateStoreName"
            }
        ]
    }
]
```

Kód Hello podporuje Windows a Linux.

Další informace najdete v tématu [vytvoření nebo aktualizace škálovací sady virtuálních počítačů](https://msdn.microsoft.com/library/mt589035.aspx).


### <a name="example-of-self-signed-certificate"></a>Příklad certifikát podepsaný svým držitelem

1.  Vytvořte certifikát podepsaný svým držitelem v trezoru klíčů.

    Použijte následující příkazy prostředí PowerShell hello:

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    Tento příkaz poskytuje hello vstup pro šablonu Azure Resource Manager hello.

    Příklad jak toocreate certifikát podepsaný svým držitelem v trezoru klíčů, najdete v části [scénáře zabezpečení clusteru Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).

2.  Změnit hello šablony Resource Manageru.

    Přidejte tuto vlastnost příliš**virtualMachineProfile**, jako součást hello škálovací sady virtuálních počítačů prostředků:

    ```json 
    "osProfile": {
        "computerNamePrefix": "[variables('namingInfix')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "secrets": [
            {
                "sourceVault": {
                    "id": "[resourceId('KeyVault', 'Microsoft.KeyVault/vaults', 'MikhegnVault')]"
                },
                "vaultCertificates": [
                    {
                        "certificateUrl": "https://mikhegnvault.vault.azure.net:443/secrets/VMSSCert/20709ca8faee4abb84bc6f4611b088a4",
                        "certificateStore": "My"
                    }
                ]
            }
        ]
    }
    ```
  

### <a name="can-i-specify-an-ssh-key-pair-toouse-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a>Můžete zadat toouse pár klíčů SSH pro ověřování SSH s měřítkem virtuální počítač Linux nastavte ze šablony Resource Manageru?  

Ano. Hello REST API pro **osProfile** je podobné toohello standardní virtuálních počítačů REST API. 

Zahrnout **osProfile** v šabloně:

```json 
"osProfile": {
    "computerName": "[variables('vmName')]",
    "adminUsername": "[parameters('adminUserName')]",
    "linuxConfiguration": {
        "disablePasswordAuthentication": "true",
        "ssh": {
            "publicKeys": [
                {
                    "path": "[variables('sshKeyPath')]",
                    "keyData": "[parameters('sshKeyData')]"
                }
            ]
        }
    }
}
```
 
Tento blok JSON se používá v [hello 101-vm-sshkey Githubu úvodní šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).
 
Hello profil operačního systému se také používá při [hello grelayhost.json Githubu rychlý start šablony](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).

Další informace najdete v tématu [vytvoření nebo aktualizace škálovací sady virtuálních počítačů](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).
  

### <a name="how-do-i-remove-deprecated-certificates"></a>Jak odstranit zastaralé certifikáty? 

tooremove zastaralé certifikáty, odeberte hello původní certifikát ze seznamu certifikátů hello trezoru. Ponechte všechny certifikáty hello chcete tooremain ve vašem počítači v seznamu hello. Hello certifikát nebude odstraněn ze všech virtuálních počítačů. Také nepřidá hello certifikát toonew virtuálních počítačů, které jsou vytvořené v sadě škálování virtuálního počítače hello. 

tooremove hello certifikát z existujících virtuálních počítačů, napsat vlastní skript rozšíření toomanually odebrat hello certifikáty z úložiště certifikátů.
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-hello-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-toostore-hello-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a>Jak I vložit existující veřejný klíč SSH do hello virtuálního počítače škálování sadu SSH vrstvy při zřizování? Chcete toostore hello SSH veřejné hodnoty klíče v Azure Key Vault a potom je použít v šabloně moje šablona Resource Manager.

Pokud zadáte hello virtuální počítače pouze pomocí veřejného klíče SSH, nepotřebujete tooput hello veřejné klíče v Key Vault. Veřejné klíče nejsou tajné.
 
Veřejné klíče SSH ve formátu prostého textu můžete zadat při vytváření virtuálního počítače s Linuxem.

```json
"linuxConfiguration": {
    "ssh": {
        "publicKeys": [
            {
                "path": "path",
                "keyData": "publickey"
            }
        ]
    }
```
 
Název elementu linuxConfiguration | Požaduje se | Typ | Popis
--- | --- | --- | --- |  ---
SSH | Ne | Kolekce | Určuje hello klíče konfigurace SSH pro operační systém Linux.
Cesta | Ano | Řetězec | Určuje, kde klíče SSH hello nebo certifikát by měl být umístěn cesta k souboru hello Linux
data klíče | Ano | Řetězec | Určuje kódování base64 veřejný klíč SSH

Příklad, naleznete v části [hello 101-vm-sshkey Githubu úvodní šablony](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-hello-same-key-vault-i-see-hello-following-message"></a>Při spuštění `Update-AzureRmVmss` po přidání více než jeden certifikát z hello stejný klíč trezoru, vidím hello následující zpráva:
 
>Update-AzureRmVmss: Tajný klíč seznam obsahuje opakované instance /subscriptions/ < my-subscription-id > / resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, což je zakázaná.
 
K tomu může dojít, pokud se pokusíte toore-přidat hello stejné místo použití nového certifikátu trezoru pro existující trezor zdroj hello trezoru. Hello `Add-AzureRmVmssSecret` příkaz nefunguje správně při přidávání dalších tajných klíčů.
 
tooadd další tajné klíče z hello stejné trezoru klíčů, seznam $vmss.properties.osProfile.secrets[0].vaultCertificates hello aktualizace.
 
Hello očekávané vstupní struktuře, najdete v části [vytvoření nebo aktualizaci virtuálního počítače nastavit](https://msdn.microsoft.com/library/azure/mt589035.aspx).
 
V hello virtuálního počítače škálování sadu objekt, který je v trezoru klíčů hello najít hello tajný klíč. Pak přidejte seznamu toohello odkaz (hello adresy URL a název tajný úložiště hello) certifikát přidružený k trezoru hello.

> [!NOTE] 
> V současné době nelze odebrat certifikáty z virtuálních počítačů pomocí hello rozhraní API sady škálování virtuálního počítače.
>

Nové virtuální počítače nebudou mít hello starý certifikát. Virtuální počítače, které jsou už nasazené, které mají certifikát hello však bude mít hello starý certifikát.
 
### <a name="can-i-push-certificates-toohello-virtual-machine-scale-set-without-providing-hello-password-when-hello-certificate-is-in-hello-secret-store"></a>Můžete posouvat škálování virtuálních počítačů toohello certifikáty nastavit bez nutnosti hello hesla, je-li hello certifikát v úložišti tajný hello?

Není nutné toohard kód hesla ve skriptech. Můžete dynamicky načíst hesla s oprávněními hello použijete skript nasazení toorun hello. Pokud máte skript, který certifikát se přesouvají z trezoru klíčů hello tajný úložiště, hello tajný úložiště `get certificate` příkaz také výstupy hello heslo souboru PFX hello.
 
### <a name="how-does-hello-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-hello-sourcevault-value-when-i-have-toospecify-hello-absolute-uri-for-a-certificate-by-using-hello-certificateurl-property"></a>Jak nastavit vlastnost tajné klíče hello virtualMachineProfile.osProfile pro škálování virtuálních počítačů pracovní? Proč musím hello sourceVault hodnotu, pokud toospecify hello absolutní identifikátor URI pro certifikát pomocí vlastnosti certificateUrl hello? 

Odkaz certifikát Vzdálená správa systému Windows (WinRM) se musí nacházet v hello tajných klíčů vlastnost hello profil operačního systému. 

účelem Hello označující trezoru zdroj hello je tooenforce přístup ovládacího prvku seznam (ACL) zásad, které existují v modelu Azure Cloud Service uživatele. Pokud není zadán hello zdroj trezoru, uživatelé, kteří nemají oprávnění toodeploy nebo přístup tajné klíče tooa trezoru klíčů by mít toothrough výpočetní prostředek zprostředkovatele (CRP). Seznamy ACL existují i pro prostředky, které nejsou k dispozici.

Pokud zadáte ID trezoru nesprávný zdrojový ale adresa URL platná trezoru klíčů, dojde k chybě při dotazování hello operaci.
 
### <a name="if-i-add-secrets-tooan-existing-virtual-machine-scale-set-are-hello-secrets-injected-into-existing-vms-or-only-into-new-ones"></a>Pokud lze přidat existující škálovací sadu virtuálních počítačů tooan tajné klíče, jsou tajné klíče hello vložit do existujících virtuálních počítačů, nebo pouze do nové? 

Certifikáty jsou přidány tooall virtuální počítače, i těch, které jsou již existující. Pokud vaše škálovací sady virtuálních počítačů upgradePolicy vlastnost se nastaví příliš**ruční**, hello certifikát bude přidán toohello virtuálních počítačů, když provádíte ruční aktualizace na hello virtuálních počítačů.
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a>Kde umístit certifikáty pro virtuální počítače s Linuxem?

jak zjistit, toodeploy certifikáty pro virtuální počítače s Linuxem, toolearn [nasadit certifikáty tooVMs z trezoru klíčů spravované zákazníkem](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).
  
### <a name="how-do-i-add-a-new-vault-certificate-tooa-new-certificate-object"></a>Jak přidat nový trezor certifikát tooa nový certifikát objekt?

tooadd trezoru certifikát tooan existující tajný klíč, viz následující příklad PowerShell text hello. Použijte pouze jeden objekt tajný.
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-toocertificates-if-you-reimage-a-vm"></a>Co se stane toocertificates Pokud obnovení z Image virtuálního počítače?

Pokud se obnovení z Image virtuálního počítače, certifikáty se odstraní. Obnovování odstranění hello celý disk operačního systému. 
 
### <a name="what-happens-if-you-delete-a-certificate-from-hello-key-vault"></a>Co se stane, když odstranit certifikát z trezoru klíčů hello?

Pokud z trezoru klíčů hello se odstraní hello tajný klíč, a pak spusťte `stop deallocate` pro všechny virtuální počítače a potom je znovu spusťte, dojde k chybě. dojde k chybě Hello, protože hello CRP vyžaduje tooretrieve hello tajné klíče z trezoru klíčů hello, ale ji nelze. V tomto scénáři můžete odstranit z modelu sady škálování virtuálního počítače hello hello certifikáty. 

součást CRP Hello není zachována tajné klíče zákazníků. Pokud spustíte `stop deallocate` pro všechny virtuální počítače ve škálovací sadu virtuálních počítačů hello hello mezipaměť je Odstraněná. V tomto scénáři se načítají tajné klíče z trezoru klíčů hello.

Tento problém není dojde při škálování, protože není v mezipaměti kopii hello tajný klíč v Azure Service Fabric (v modelu hello fabric jednoho klienta).
 
### <a name="why-do-i-have-toospecify-hello-exact-location-for-hello-certificate-url-httpsname-of-hello-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a>Proč musím toospecify hello přesné umístění pro adresa URL certifikátu hello (https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>), jak je uvedeno v [scénáře zabezpečení clusteru Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?
 
Hello dokumentace Azure Key Vault stavy tohoto rozhraní API REST získání tajného klíče by měla vrátit hello nejnovější verzi hello tajný klíč, pokud není určená verze hello hello.
 
Metoda | ADRESA URL
--- | ---
GET | https://mykeyvault.Vault.Azure.NET/secrets/ {tajný klíč name} / {tajný klíč version}? api-version = {api-version}

Nahraďte {*tajný klíč název*} s názvem hello a nahraďte {*tajný klíč verze*} verzí hello tajného klíče hello chcete tooretrieve. verzi tajného klíče Hello může být vyloučeny. V takovém případě se načte aktuální verze hello.
  
### <a name="why-do-i-have-toospecify-hello-certificate-version-when-i-use-key-vault"></a>Proč se při použití Key Vault mají toospecify hello certifikátu verze?

účelem Hello hello Key Vault požadavek toospecify hello certifikátu verze je toomake vymazat toohello uživatele co certifikát je nasazený na jejich virtuální počítače.

Pokud vytvoříte virtuální počítač a pak aktualizujte váš tajný klíč v trezoru klíčů hello, není nový certifikát hello stažené tooyour virtuálních počítačů. Ale virtuální počítače se tooreference a nové virtuální počítače získat hello nový sdílený tajný klíč. tooavoid to jsou požadované tooreference verzi tajného klíče.

### <a name="my-team-works-with-several-certificates-that-are-distributed-toous-as-cer-public-keys-what-is-hello-recommended-approach-for-deploying-these-certificates-tooa-virtual-machine-scale-set"></a>Můj tým pracuje s několik certifikátů, které jsou distribuovány toous jako .cer veřejných klíčů. Co je hello doporučenému přístupu pro nasazení škálovací sadu virtuálních počítačů tooa tyto certifikáty?

toodeploy .cer veřejné klíče tooa škálovací sadu virtuálních počítačů, můžete generovat soubor .pfx, který obsahuje pouze soubory s příponou CER. toodo tuto, použijte `X509ContentType = Pfx`. Například načíst soubor .cer hello jako objekt x509Certificate2 v C# nebo prostředí PowerShell a potom volejte metodu hello. 

Další informace najdete v tématu [X509Certificate.Export – metoda (X509ContentType, řetězec)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).

### <a name="i-do-not-see-an-option-for-users-toopass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a>Možnost uživatelé toopass v certifikátech nezobrazí jako řetězce formátu base64. Tato možnost k dispozici většina jiných poskytovatelů prostředků.

tooemulate předávání v certifikátu jako řetězec base64 můžete rozbalit hello nejnovější verzí adresy URL v šabloně Resource Manager. Zahrnout hello následující vlastnosti JSON v šabloně Resource Manager:

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-toowrap-certificates-in-json-objects-in-key-vaults"></a>V objektech JSON v trezorů klíčů jsou toowrap certifikáty?

Sady škálování virtuálního počítače a virtuální počítače musí být uzavřen certifikáty do objektů JSON. 

Také podporujeme hello typ obsahu application/x-pkcs12. Pokyny k používání application/x-pkcs12 najdete v tématu [certifikáty PFX v Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).
 
Aktuálně nepodporujeme souborů s příponou CER. soubory s příponou CER toouse, je exportovat do kontejnerů .pfx.



## <a name="compliance"></a>Dodržování předpisů

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a>Jsou kompatibilní se standardem PCI sadách škálování virtuálních počítačů?

Sady škálování virtuálního počítače jsou dynamické vrstvu rozhraní API nad hello CRP. Obě komponenty jsou součástí hello výpočetní platforma v hello Azure stromu služby.

Z hlediska dodržování předpisů sady škálování virtuálního počítače jsou základní součástí hello výpočetní platformě Azure. Tým, nástroje, procesy, metody nasazení, kontrolních mechanismů pro zabezpečení, za běhu (JIT) kompilace, monitorování, výstrahy a tak dále, sdílejí s hello CRP sám sebe. Sady škálování virtuálního počítače jsou odvětví platebních karet (PCI)-kompatibilní, protože hello CRP je součástí hello aktuální ověření PCI Data zabezpečení DSS (Standard).

Další informace najdete v tématu [hello Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).






## <a name="extensions"></a>Rozšíření

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a>Jak lze odstranit rozšíření sady škálování virtuálního počítače?

toodelete škálování virtuálního počítače nastavit rozšíření, hello použijte následující příklad PowerShell:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
Můžete najít hodnotu Název_rozšíření hello v `$vmss`.
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a>Je k dispozici, že že škálovací sady virtuálních počítačů příklad šablony, který se integruje s Operations Management Suite?

Pro škálování virtuálních počítačů, nastavte příklad šablony, který se integruje s Operations Management Suite, najdete v části hello druhém příkladu v [nasazení clusteru Azure Service Fabric a povolte monitorování pomocí analýzy protokolů](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).
   
### <a name="extensions-seem-toorun-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-toofail-what-can-i-do-toofix-this"></a>Rozšíření zdá se, že toorun paralelně na sady škálování virtuálního počítače. To způsobí, že moje toofail rozšíření vlastních skriptů. Jak toofix to?

toolearn o rozšíření sekvencování v sady škálování virtuálního počítače, najdete v části [rozšíření sekvencování v sady škálování virtuálního počítače Azure](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).
 
 
### <a name="how-do-i-reset-hello-password-for-vms-in-my-virtual-machine-scale-set"></a>Jak I reset hello heslo pro virtuální počítače v mé škálovací sadu virtuálních počítačů?

tooreset hello heslo pro virtuální počítače ve vaší škálování virtuálních počítačů nastavení, použijte rozšíření virtuálního počítače přístup. 

Použijte následující příklad PowerShell hello:

```powershell
$vmssName = "myvmss"
$vmssResourceGroup = "myvmssrg"
$publicConfig = @{"UserName" = "newuser"}
$privateConfig = @{"Password" = "********"}
 
$extName = "VMAccessAgent"
$publisher = "Microsoft.Compute"
$vmss = Get-AzureRmVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
$vmss = Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
Update-AzureRmVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName -VirtualMachineScaleSet $vmss
```
 
 
### <a name="how-do-i-add-an-extension-tooall-vms-in-my-virtual-machine-scale-set"></a>Jak přidat rozšíření tooall virtuální počítače v mé škálovací sadu virtuálních počítačů?

Pokud zásady aktualizace je nastaven příliš**automatické**, opětovného nasazení šablony hello novými vlastnostmi rozšíření hello aktualizuje všechny virtuální počítače.

Pokud zásady aktualizace je nastaven příliš**ruční**, nejdřív aktualizovat hello rozšíření a potom ručně aktualizovat všechny instance ve virtuálních počítačů.

  
### <a name="if-hello-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-hello-vms-not-match-hello-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-hello-scripts-that-are-currently-configured-on-hello-virtual-machine-scale-set-executed-or-are-hello-scripts-that-were-configured-when-hello-vm-was-first-created-used"></a>Pokud jsou aktualizovány hello rozšíření spojená se existující škálovací sadu virtuálních počítačů jsou existující virtuální počítače vliv? (To znamená, budou virtuální počítače hello *není* modelu sady škálování virtuálního počítače hello shodu?) Nebo se budou ignorovat? Pokud je existující počítač zacelené služby nebo obnovit z Image, jsou hello skripty, které jsou aktuálně nakonfigurované na škálovací sadu virtuálních počítačů hello provést, nebo se hello skripty, které byly nakonfigurovány při použití hello, ke které byl virtuální počítač nejprve vytvořen?

Pokud definice rozšíření hello v škálování virtuálních počítačů hello nastavený dojde k aktualizaci modelu a hello upgradePolicy je nastavena příliš**automatické**, aktualizuje hello virtuálních počítačů. Pokud je vlastnost upgradePolicy hello nastavena příliš**ruční**, rozšíření jsou označeny jako neodpovídá modelu hello. 

Pokud existující virtuální počítač zacelené služby, zobrazí se jako restartování a hello rozšíření nejsou znovu spusťte. Pokud ho je obnovit z Image, je třeba nahraďte hello zdrojové bitové kopie disku hello operačního systému. Všechny specializace z nejnovější modelu hello, jako je například rozšíření, jsou spuštěny.
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-tooan-azure-ad-domain"></a>Jak připojit k doméně tooan Azure AD sady škálování virtuálního počítače?

toojoin domény Azure Active Directory (Azure AD) tooan sady škálování virtuálního počítače, můžete definovat rozšíření. 

toodefine rozšíření, použijte vlastnost JsonADDomainExtension hello:

```json
"extensionProfile": {
    "extensions": [
        {
            "name": "joindomain",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "JsonADDomainExtension",
                "typeHandlerVersion": "1.3",
                "settings": {
                    "Name": "[parameters('domainName')]",
                    "OUPath": "[variables('ouPath')]",
                    "User": "[variables('domainAndUsername')]",
                    "Restart": "true",
                    "Options": "[variables('domainJoinOptions')]"
                },
                "protectedsettings": {
                    "Password": "[parameters('domainJoinPassword')]"
                }
            }
        }
    ]
}
```
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-tooinstall-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a>Moje rozšíření sady škálování virtuálního počítače se pokouší tooinstall něco, co vyžaduje restartování počítače. Například "commandToExecute": "powershell.exe - ExecutionPolicy Unrestricted Install-WindowsFeature – název služby FS-Resource-Manager – IncludeManagementTools"

Pokud vaše rozšíření sady škálování virtuálního počítače se pokouší tooinstall něco, co vyžaduje restartování počítače, můžete použít rozšíření Azure Automation konfigurace požadovaného stavu (DSC automatizace) hello. Pokud hello operační systém je Windows Server 2012 R2, Azure vrátí v instalačním programu hello Windows Management Framework (WMF) 5.0 restartování počítače a poté pokračuje v konfiguraci hello. 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a>Jak zapnout antimalwarových v mé škálovací sadu virtuálních počítačů?

tooturn na antimalware na vaše škálování virtuálních počítačů nastavení, použijte následující příklad PowerShell hello:

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve hello most recent version number of hello extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-tooexecute-a-custom-script-thats-hosted-in-a-private-storage-account-hello-script-runs-successfully-when-hello-storage-is-public-but-when-i-try-toouse-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a>Potřebuji tooexecute vlastní skript, který je hostován v účtu privátní úložiště. Hello skript spustí úspěšně hello úložiště je veřejný, ale když se pokouším toouse sdíleného přístupového podpisu (SAS), se nezdaří. Zobrazí se tato zpráva: "Chybějící povinné parametry pro platný podpis sdíleného přístupu". Odkaz + SAS funguje bez problémů z místní v prohlížeči.

tooexecute vlastní skript, který je hostován v účet privátní úložiště nastavit chráněné hello klíč účtu úložiště a s názvem. Další informace najdete v tématu [vlastní skript rozšíření pro Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).







## <a name="networking"></a>Sítě
 
### <a name="is-it-possible-tooassign-a-network-security-group-nsg-tooa-scale-set-so-that-it-will-apply-tooall-hello-vm-nics-in-hello-set"></a>Je možné tooassign nastavit stupnici tooa skupina zabezpečení sítě (NSG), takže bude se vztahovat tooall hello síťové adaptéry virtuálních počítačů v sadě hello?

Ano. Skupina zabezpečení sítě je možné použít přímo sady škálování tooa odkazem v části Networkinterfaceconfiguration hello hello profilu sítě. Příklad:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                 }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-hello-same-subscription-and-same-region"></a>Jakým způsobem prohození virtuálních IP adres pro sadách škálování virtuálních počítačů v hello stejné předplatné a stejné oblasti?

Pokud máte dva virtuální počítače, nastaví se nástroj pro vyrovnávání zatížení Azure front-end a jsou v hello stejném předplatném, oblasti, můžete zrušit přidělení hello veřejné IP adresy z každé z nich a přiřadit toohello jiné. V tématu [prohození virtuálních IP adres: modrá zelená nasazení ve službě Správce prostředků Azure](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) třeba. To neznamená zpoždění, i když jako hello jsou prostředky navrácena přidělené na úrovni sítě hello. Rychlejší možnost je toouse Azure Application Gateway se dvěma back-endové fondy a pravidel směrování. Alternativně může hostování vaší aplikace s [služby Azure App service](https://azure.microsoft.com/en-us/services/app-service/) která poskytuje podporu pro rychlé přepínání mezi sloty pracovní a provozní.
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-toouse-for-static-private-ip-address-allocation"></a>Jak určit rozsah privátní IP adresy toouse pro statického přidělování privátní IP adresu?

IP adresy jsou vybrány z podsítě, který určíte. 

Metoda přidělení Hello adres IP sady škálování virtuálního počítače je vždy "dynamická", ale který neznamená, že tyto IP adresy můžou změnit. V tomto případě "dynamická" pouze znamená, nezadávejte IP adresu hello v požadavku PUT. Zadejte hello statické nastavit pomocí hello podsítě. 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-tooan-existing-azure-virtual-network"></a>Nasazení virtuální počítač škálování sadu tooan existující virtuální síť Azure 

toodeploy škálování virtuálního počítače nastavit tooan existující virtuální síť Azure, najdete v části [nasadit virtuální počítač škálování sadu tooan existující virtuální síť](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet). 

### <a name="how-do-i-add-hello-ip-address-of-hello-first-vm-in-a-virtual-machine-scale-set-toohello-output-of-a-template"></a>Jak přidat hello IP adresu z hello první virtuální počítač v škálování virtuálního počítače nastavit toohello výstup šablony?

tooadd hello IP adresu hello první virtuální počítač v virtuálních počítačů škálování sadu toohello výstupu šablony, najdete v části [ARM: privátních IP adres získat VMSS](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a>Můžete použít sady škálování pomocí Accelerated sítě?

Ano. toouse accelerated sítě, nastavení enableAcceleratedNetworking tootrue ve vaší škálovací sady Networkinterfaceconfiguration. Například
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
        "name": "niconfig1",
        "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
                ]
            }
            }
        ]
        }
    }
    ]
}
```

### <a name="how-can-i-configure-hello-dns-servers-used-by-a-scale-set"></a>Konfigurování serverů DNS hello používá škálovací sada

toocreate sady škálování virtuálního počítače pomocí vlastní konfigurace DNS, přidejte dnsSettings JSON paketu toohello škálování nastavit Networkinterfaceconfiguration části. Příklad:
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-tooassign-a-public-ip-address-tooeach-vm"></a>Konfigurování škálování sadu tooassign tooeach veřejnou adresu IP virtuálního počítače

toocreate sady škálování virtuálního počítače, přiřadí tooeach veřejnou adresu IP virtuálního počítače, zkontrolujte, zda je 2017-03-30 hello rozhraní API verze hello Microsoft.Compute/virtualMAchineScaleSets prostředků a přidejte _publicipaddressconfiguration_ paketu JSON sady škálování toohello část konfigurace IP adresy. Příklad:

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-toowork-with-multiple-application-gateways"></a>Můžete nakonfigurovat sadu toowork škálování s více bran aplikace?

Ano. Můžete přidat hello id prostředku je pro více Application Gateway back-end adresy fondy toohello _applicationGatewayBackendAddressPools_ seznamu v hello _konfigurace IP adresy_ části škálovací sady profil sítě.

## <a name="scale"></a>Měřítko

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a>V případě, co by vytvořit škálování virtuálního počítače s méně než dva virtuální počítače?

Jedním z důvodů toocreate škálovací sady virtuálních počítačů s méně než dva virtuální počítače se nastavuje toouse hello elastické vlastnosti škálování virtuálních počítačů. Například můžete nasadit škálovací sadu virtuálních počítačů s nulový počet virtuálních počítačů toodefine infrastruktury bez placení virtuálního počítače s náklady. Potom po připravené toodeploy virtuální počítače zvýšit hello "kapacitu" hello virtuálního počítače škálování sadu toohello produkční počet instancí.

Dalším důvodem, které můžete vytvořit škálování virtuálních počítačů s méně než dva virtuální počítače je, pokud máte obavy, méně s dostupností než při použití s virtuálními počítači diskrétní sadu dostupnosti. Sady škálování virtuálního počítače zadejte způsob toowork s poskytujících blíže neurčené výpočetní jednotky, které jsou zastupitelných. Toto sjednocení je klíčovým rozdílem pro sady škálování virtuálního počítače a skupiny dostupnosti. Mnoho Bezstavová zatížení do not track jednotlivé jednotky. Pokud hodnota klesne hello zatížení, můžete snížit tooone výpočetní jednotka a pak škálovat toomany při zvyšuje zatížení hello.

### <a name="how-do-i-change-hello-number-of-vms-in-a-virtual-machine-scale-set"></a>Změna hello počet virtuálních počítačů sady škálování virtuálního počítače

toochange hello počet virtuální počítače ve škálovací sadě virtuálních počítačů najdete v části [změnit počet instancí hello škálovací sadu virtuálních počítačů](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a>Jak definuje vlastní výstrahy pro když se dosáhne určité prahové hodnoty?

Máte určitou volnost v tom, jak zpracovat výstrahy pro zadané prahové hodnoty. Například můžete definovat vlastní webhooky. Následující ukázka webhooku Hello je z šablony Resource Manageru:

```json
{
    "type": "Microsoft.Insights/autoscaleSettings",
    "apiVersion": "[variables('insightsApi')]",
    "name": "autoscale",
    "location": "[parameters('resourceLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
    ],
    "properties": {
        "name": "autoscale",
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
        "enabled": true,
        "notifications": [
            {
                "operation": "Scale",
                "email": {
                    "sendToSubscriptionAdministrator": true,
                    "sendToSubscriptionCoAdministrators": true,
                    "customEmails": [
                        "youremail@address.com"
                    ]
                },
                "webhooks": [
                    {
                        "serviceUri": "https://events.pagerduty.com/integration/0b75b57246814149b4d87fa6e1273687/enqueue",
                        "properties": {
                            "key1": "custommetric",
                            "key2": "scalevmss"
                        }
                    }
                ]
            }
        ],
```

V tomto příkladu výstrahu přejde tooPagerduty.com, když je dosaženo prahové hodnoty.



## <a name="patching-and-operations"></a>Operace a oprav

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a>Vytvoření sad v existující skupinu prostředků škálování

Vytváření sad škálování v existující prostředek skupiny ještě není možné z hello portálu Azure, ale můžete zadat existující skupinu prostředků, když nasazení škálování nastavit z šablony Azure Resource Manager. Můžete také zadat existující skupinu prostředků, při vytváření škálování nastavit pomocí prostředí Azure PowerShell nebo rozhraní příkazového řádku.

### <a name="can-we-move-a-scale-set-tooanother-resource-group"></a>Jsme, že škálování nastavit skupinu prostředků tooanother můžete přesunout?

Ano, můžete přesunout škálování sadu prostředků tooa nové předplatné nebo skupinu prostředků.

### <a name="how-tooi-update-my-virtual-machine-scale-set-tooa-new-image-how-do-i-manage-patching"></a>Jak aktualizovat tooI Moje škálovací sady virtuálních počítačů tooa novou bitovou kopii? Jak spravovat, opravy?

tooupdate vaše škálovací sady virtuálních počítačů tooa novou bitovou kopii a opravy chyb toomanage, najdete v části [upgradovat škálovací sadu virtuálních počítačů](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).

### <a name="can-i-use-hello-reimage-operation-tooreset-a-vm-without-changing-hello-image-that-is-i-want-reset-a-vm-toofactory-settings-rather-than-tooa-new-image"></a>Můžete použít tooreset operace obnovení z Image hello virtuálních počítačů beze změny hello image? (To znamená, chci resetování nastavení virtuálních počítačů toofactory místo tooa novou bitovou kopii.)

Ano, aniž byste museli měnit hello image můžete tooreset operace obnovení z Image hello virtuálního počítače. Ale pokud vaše škálovací sady virtuálních počítačů odkazuje image platformy s `version = latest`, virtuální počítač můžete aktualizovat tooa při volání bitové kopie operačního systému novější `reimage`.

Další informace najdete v tématu [spravovat všechny virtuální počítače ve škálovací sadu virtuálních počítačů](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).



## <a name="troubleshooting"></a>Řešení potíží

### <a name="how-do-i-turn-on-boot-diagnostics"></a>Jak zapnout na Diagnostika spouštění?

tooturn na Diagnostika spouštění, nejdřív vytvořte účet úložiště. Pak, přesuňte tento blok JSON sady škálování virtuálního počítače **virtualMachineProfile**a aktualizovat sadu škálování virtuálního počítače hello:

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

Při vytváření nového virtuálního počítače hello InstanceView vlastnost hello virtuální počítač zobrazuje podrobnosti hello hello – snímek obrazovky a tak dále. Tady je příklad:
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a>Vlastnosti virtuálního počítače

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-hello-fault-domain-for-each-of-hello-100-vms-in-my-virtual-machine-scale-set"></a>Jak získat informace o vlastnosti pro každý virtuální počítač bez volání více? Například jak by získat doména selhání hello každý hello 100 virtuálních počítačů v mé škálovací sadu virtuálních počítačů?

informace o vlastnosti tooget pro každý virtuální počítač bez volání více, můžete volat `ListVMInstanceViews` pomocí rozhraní REST API `GET` na hello následující identifikátor URI prostředku:

/subscriptions/ < ID_ODBĚRU > /resourceGroups/ < resource_group_name > /providers/Microsoft.Compute/virtualMachineScaleSets/ < scaleset_name > / virtualMachines? $expand = instanceView & $select = instanceView

### <a name="can-i-pass-different-extension-arguments-toodifferent-vms-in-a-virtual-machine-scale-set"></a>Může I předání jiné rozšíření argumentů toodifferent virtuální počítače ve škálovací sadě virtuálního počítače?

Ne, nelze předat jiné rozšíření argumenty toodifferent virtuální počítače ve škálovací sadě virtuálních počítačů. Rozšíření však může fungovat podle hello jedinečné vlastnosti hello virtuální počítač běží na, například jako na název počítače hello. Rozšíření také můžete dotazovat instanci metadata na http://169.254.169.254 tooget Další informace o hello virtuálních počítačů.

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a>Proč se mezery mezi Moje identifikátory virtuálních počítačů a názvy počítačů virtuálních počítačů sady škálování virtuálního počítače? Příklad: 0, 1, 3...

Protože vaše škálovací sady virtuálních počítačů jsou mezery mezi názvy počítačů virtuálních počítačů sady škálování virtuálního počítače a virtuálního počítače ID **overprovision** je nastavena výchozí hodnota toohello **true**. Pokud předimenzování je nastaven příliš**true**, než požadovaná vytvářejí další virtuální počítače. Navíc za virtuální počítače, budou odstraněna. V takovém případě získat nasazení vyšší spolehlivost ale na náklady hello souvislý pojmenování a souvislý překlad adres (NAT) pravidla. 

Tuto vlastnost lze nastavit příliš**false**. Pro malé virtuálního počítače sady škálování to nemá vliv výrazně spolehlivost nasazení.

### <a name="what-is-hello-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-hello-vm-when-should-i-choose-one-over-hello-other"></a>Co je hello rozdíl mezi odstranění virtuálního počítače ve škálovací sadu virtuálních počítačů a rušení přidělení hello virtuálního počítače? Pokud by měl vybrat některého hello další?

Hello hlavní rozdíl mezi odstranění virtuálního počítače ve škálovací sadu virtuálních počítačů a rušení přidělení hello virtuálních počítačů je, že `deallocate` nedojde k odstranění hello virtuální pevné disky (VHD). Jsou náklady na úložiště přidružené k spuštění `stop deallocate`. Může použít jednu nebo druhou pro jednu z následujících důvodů hello hello:

- Chcete toostop platícího výpočetní náklady, ale chcete tookeep hello disku stav hello virtuálních počítačů.
- Chcete toostart sadu virtuálních počítačů rychleji, než může škálovat škálovací sadu virtuálních počítačů.
  - Související toothis scénáři jste mohli vytvořit vlastní modul škálování a chcete rychlejší škálování začátku do konce.
- Máte škálovací sadu virtuálních počítačů, nerovnoměrně distribuované domén selhání a aktualizace domény. Může to být proto selektivně odstranit virtuální počítače, nebo proto, že virtuální počítače byly odstraněny po předimenzování. Spuštění `stop deallocate` následuje `start` na virtuálním počítači hello rozděluje rovnoměrně sad škálování virtuálních počítačů hello na domén selhání nebo doménách aktualizace.


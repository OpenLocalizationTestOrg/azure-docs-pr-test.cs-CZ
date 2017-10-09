---
title: "pomocí obnovení po havárii aaaImplement zálohování a obnovení v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toouse zálohování a obnovení tooperform zotavení po havárii v Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 058bfb579e3a3f51fb1dac8ea37eb4fdbc83a4ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimplement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Jak tooimplement po havárii obnovení pomocí služby zálohování a obnovení v Azure API Management
Pomocí výběr toopublish a spravovat vaše rozhraní API pomocí Azure API Management jsou využívat výhod mnoha odolnost proti chybám a infrastruktury možnosti, které byste jinak museli toodesign, implementovat a spravovat. Hello platformy Azure snižuje velké zlomek případy možného selhání za zlomek hello náklady.

toorecover z dostupnosti problémů, které mají vliv na hello oblast, kde služby API Management je hostovaná vám musí být připravené tooreconstitute služby v jiné oblasti kdykoli. V závislosti na vašich cílů dostupnosti a cíli času obnovení můžete má tooreserve zálohování služby v jedné nebo více oblastech a zkuste toomaintain jejich konfigurace a obsahu, které jsou synchronizované se službou active hello. Hello služby zálohování a obnovení funkce poskytuje hello nezbytné stavební blok pro implementaci strategie zotavení po havárii.

Tato příručka ukazuje, jak požaduje tooauthenticate Azure Resource Manager a jak toobackup a obnovit vaše instance služby API Management.

> [!NOTE]
> pro replikaci instance služby API Management pro scénáře, jako je pracovní se lze také Hello proces zálohování a obnovení instance služby API Management pro zotavení po havárii.
>
> Všimněte si, že každá záloha vyprší po 30 dnech. Když zkusíte toorestore zálohu po dobu platnosti 30denní hello vypršela platnost, se nezdaří obnovení hello s `Cannot restore: backup expired` zprávy.
>
>

## <a name="authenticating-azure-resource-manager-requests"></a>Požadavky na ověřování Azure Resource Manager
> [!IMPORTANT]
> Hello REST API pro zálohování a obnovení používá Azure Resource Manager a má jiný mechanismus ověřování než hello rozhraní REST API pro správu vaší entity API Management. Hello kroky v této části popisují, jak požaduje tooauthenticate Azure Resource Manager. Další informace najdete v tématu [požadavků ověřování Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx).
>
>

Všechny hello úlohy, které můžete provést na prostředků pomocí Azure Resource Manager hello musí být ověřeny v Azure Active Directory pomocí hello následující kroky.

* Přidáte klienta Azure Active Directory toohello aplikaci.
* Nastavte oprávnění pro hello aplikace, které jste přidali.
* Získáte hello token pro ověřování požadavků tooAzure Resource Manager.

prvním krokem Hello je toocreate aplikaci Azure Active Directory. Přihlaste se k hello [portálu Azure Classic](http://manage.windowsazure.com/) pomocí hello předplatného, který obsahuje služby API Management instance a přejděte toohello **aplikace** kartu pro výchozí Azure Active Directory.

> [!NOTE]
> Pokud hello Azure Active Directory výchozí adresář není viditelné tooyour účtu, správce kontaktní hello hello předplatného Azure toogrant hello vyžaduje oprávnění tooyour účtu.

![Vytvoření aplikace Azure Active Directory][api-management-add-aad-application]

Klikněte na tlačítko **přidat**, **přidat aplikaci, kterou vyvíjí Moje organizace**a zvolte **nativní klientskou aplikaci**. Zadejte popisný název a klikněte na šipku další hello. Zadejte adresu URL zástupného symbolu, jako `http://resources` pro hello **identifikátor URI pro přesměrování**, jako je povinné pole, ale hodnota hello nepoužívá později. Klikněte na tlačítko hello políčko toosave hello aplikace.

Jakmile aplikace hello je uložena, klikněte na možnost **konfigurace**, projděte dolů toohello **oprávnění tooother aplikace** a klikněte na **přidat aplikaci**.

![Přidání oprávnění][api-management-aad-permissions-add]

Vyberte **Windows** **Azure Service Management API** a klikněte na tlačítko hello políčko tooadd hello aplikace.

![Přidání oprávnění][api-management-aad-permissions]

Klikněte na tlačítko **delegovaná oprávnění** vedle hello nově přidaná **Windows** **Azure Service Management API** aplikace, políčko hello **přístup k Azure Správa služeb (preview)**a klikněte na tlačítko **Uložit**.

![Přidání oprávnění][api-management-aad-delegated-permissions]

Předchozí tooinvoking hello rozhraní API, která generovat hello zálohování a obnovte ji, je nutné tooget token. Hello následující příklad používá hello [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) token hello tooretrieve balíček nuget.

```c#
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed tooobtain hello JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Nahraďte `{tentand id}`, `{application id}`, a `{redirect uri}` pomocí pokynů hello.

Nahraďte `{tenant id}` s id klienta hello hello aplikaci Azure Active Directory, kterou jste právě vytvořili. Přistupujete hello id kliknutím **zobrazit koncové body**.

![Koncové body][api-management-aad-default-directory]

![Koncové body][api-management-endpoint]

Nahraďte `{application id}` a `{redirect uri}` pomocí hello **Id klienta** a hello adresu URL z hello **identifikátory URI přesměrování** části z vaší aplikace Azure Active Directory **konfigurace**  kartě.

![Zdroje][api-management-aad-resources]

Jakmile jsou zadané hodnoty hello, by měl vrátit příklad kódu hello tokenu podobné toohello následující ukázka.

![Token][api-management-arm-token]

Před voláním hello zálohování a obnovení popsaný v následující části hello, nastavte hello autorizační hlavičky požadavku pro volání REST.

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <a name="step1"></a>Zálohování služby API Management
tooback až hello problém služby API Management následující požadavek HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

Kde:

* `subscriptionId`– id předplatného hello obsahující hello služba API Management se pokoušíte toobackup
* `resourceGroupName`-řetězec v podobě hello 'API - výchozí – {služba region}, kde `service-region` identifikuje hello oblast Azure, kde je umístěn hello služba API Management se pokoušíte toobackup, například`North-Central-US`
* `serviceName`-Název hello hello služba API Management provádíte zálohování zadaný během hello jeho vytvoření
* `api-version`-nahradit`2014-02-14`

V textu hello hello požadavku zadejte název účtu úložiště Azure hello cíl, přístupový klíč, název kontejneru objektu blob a název zálohy:

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Nastavte hodnotu hello hello `Content-Type` hlavička požadavku příliš`application/json`.

Zálohování je dlouhotrvající operace, která může trvat několik minut toocomplete.  Pokud hello požadavek byl úspěšný a byl zahájen proces zálohování hello dostanete `202 Accepted` stavový kód odpovědi s `Location` záhlaví.  Zkontrolujte požadavky "GET" toohello adresu URL v hello `Location` záhlaví toofind out hello stav operace hello. Když probíhá hello zálohování bude pokračovat tooreceive 202 přijata stavový kód. Kód odpovědi `200 OK` označí úspěšném dokončení operace zálohování hello.

Všimněte si hello následující omezení při vytváření zálohy požadavku.

* **Kontejner** zadaná v textu žádosti hello **musí existovat**.
* Při zálohování se v průběhu **neměli žádné operace správy, služba** například SKU upgrade nebo přechod na starší verzi, změna názvu domény, atd.
* Obnovení systému **zálohování záruku, že se pouze po dobu 30 dnů** od okamžiku hello jeho vytvoření.
* **Data o využití** použitý k vytvoření sestavy analýzy **nezahrnuje** hello zálohování. Použití [REST API služby Azure API Management] [ Azure API Management REST API] tooperiodically načíst analytics sestavy z bezpečnostních důvodů udržován.
* Hello četnost, se kterým můžete provést zálohování služby ovlivní vaše plánovaného bodu obnovení. toominimize ji doporučujeme, provádění pravidelných záloh a také provádění zálohování na vyžádání po provedení důležité změny tooyour služba API Management.
* **Změny** učiněna toohello konfigurace služby (např. rozhraní API, zásady, vzhled portálu vývojáře) během operace zálohování je v procesu **nemusí být součástí hello zálohování a proto budou ztraceny**.

## <a name="step2"></a>Obnovení služby API Management
toorestore služby API Management z dříve vytvořeného zkontrolujte hello následující požadavek HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

Kde:

* `subscriptionId`– id předplatného hello obsahující hello služba API Management jsou obnovení ze zálohy do
* `resourceGroupName`-řetězec v podobě hello 'API - výchozí – {služba region}, kde `service-region` identifikuje hello oblast Azure, kde je umístěn hello obnovujete zálohu do služby API Management, například`North-Central-US`
* `serviceName`-Název hello hello API Management zadat služby obnovena do okamžiku hello jeho vytvoření
* `api-version`-nahradit`2014-02-14`

V textu hello hello požadavku zadejte umístění záložního souboru hello, tj. název účtu úložiště Azure, přístupový klíč, název kontejneru objektu blob a název zálohy:

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

Nastavte hodnotu hello hello `Content-Type` hlavička požadavku příliš`application/json`.

Obnovení je dlouhotrvající operace, která může trvat až too30 nebo další toocomplete minut.  Pokud hello požadavek byl úspěšný a byl zahájen proces obnovení hello dostanete `202 Accepted` stavový kód odpovědi s `Location` záhlaví.  Zkontrolujte požadavky "GET" toohello adresu URL v hello `Location` záhlaví toofind out hello stav operace hello. Když probíhá hello obnovení bude pokračovat tooreceive '202 platných' stavový kód. Kód odpovědi `200 OK` označí úspěšném dokončení operace obnovení hello.

> [!IMPORTANT]
> **Hello SKU** služby hello obnovena do **musí odpovídat** hello SKU hello zálohovat služba obnovena.
>
> **Změny** učiněna toohello konfigurace služby (např. rozhraní API, zásady, vzhled portálu vývojáře) během operace obnovení je v průběhu **může dojít k přepsání**.
>
>

## <a name="next-steps"></a>Další kroky
Podívejte se na následující blogy od Microsoftu pro dva různé postupy procesu zálohování a obnovení hello hello.

* [Replikovat účty Azure API Management](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * Děkujeme, že tooGisela pro svůj článek toothis příspěvku.
* [Azure API Management: Zálohování a obnovení konfigurace](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * přístup Hello podrobné podle Stanislav neodpovídá hello oficiální pokyny, ale je velmi zajímavé.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png

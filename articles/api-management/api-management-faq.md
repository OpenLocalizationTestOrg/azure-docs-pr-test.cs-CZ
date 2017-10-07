---
title: "aaaAzure nejčastější dotazy týkající se rozhraní API Management | Microsoft Docs"
description: "Zjistěte, zda text hello odpovídá na dotazy toocommon, vzorce a osvědčené postupy v Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 9e7cdf1b881a4dfed4bd2cfd7fbb4994f48b5f79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-faqs"></a>Nejčastější dotazy Azure API Management
Získáte hello odpovědi toocommon otázky, vzorce a osvědčené postupy pro Azure API Management.

## <a name="contact-us"></a>Kontaktujte nás
* [Jak mohu požádat tým Microsoft Azure API Management hello dotaz?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)


## <a name="frequently-asked-questions"></a>Nejčastější dotazy
* [Co znamená funkce je ve verzi preview?](#what-does-it-mean-when-a-feature-is-in-preview)
* [Jak můžete zabezpečit hello připojení mezi bránou rozhraní API správy hello a Moje back endové služby?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
* [Jak lze zkopírovat Moje instance služby API Management instance tooa nové?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
* [Můžete spravovat instance Moje API Management prostřednictvím kódu programu?](#can-i-manage-my-api-management-instance-programmatically)
* [Jak přidat skupinu uživatelů toohello správci?](#how-do-i-add-a-user-to-the-administrators-group)
* [Proč je hello zásady, že chcete tooadd není k dispozici v editoru zásad hello?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
* [Jak používat správu verzí rozhraní API ve službě API Management?](#how-do-i-use-api-versioning-in-api-management)
* [Jak nastavím prostředí s více v jediného rozhraní API?](#how-do-i-set-up-multiple-environments-in-a-single-api)
* [Můžete použít protokolu SOAP s API Management?](#can-i-use-soap-with-api-management)
* [Je hello API Management gateway IP adresu konstanta? Můžete je používat v pravidlech brány firewall?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
* [Můžete nakonfigurovat serveru ověřování OAuth 2.0 pomocí služby AD FS zabezpečení?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
* [V nasazení toomultiple geografických polohách jaké metody směrování používá rozhraní API Management?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
* [Můžete použít toocreate šablony Azure Resource Manager instanci služby API Management?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
* [Můžete použít certifikát podepsaný svým držitelem SSL pro back-end?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
* [Proč se při tooclone úložiště GIT zobrazí chybu ověřování?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
* [Funguje s Azure ExpressRoute API Management?](#does-api-management-work-with-azure-expressroute)
* [Proč jsme vyžaduje vyhrazenou podsíť ve stylu Resource Manager virtuální sítě, když API Management se nasadí do nich?](#why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them)
* [Jaká je velikost minimální podsíť hello potřebné při nasazování API Management do virtuální sítě?](#what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet)
* [Lze přesunout z jednoho tooanother předplatné služby API Management?](#can-i-move-an-api-management-service-from-one-subscription-to-another)
* [Existují známé problémy s importem Moje rozhraní API nebo omezení?](#are-there-restrictions-on-or-known-issues-with-importing-my-api)

### <a name="how-can-i-ask-hello-microsoft-azure-api-management-team-a-question"></a>Jak mohu požádat tým Microsoft Azure API Management hello dotaz?
Kontaktujte nás pomocí jedné z těchto možností:

* Zveřejněte svoje otázky v našem [fórum MSDN správy rozhraní API](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
* Odeslat e-mail příliš<mailto:apimgmt@microsoft.com>.
* Pošlete nám žádosti o funkci v hello [fóru pro zpětnou vazbu Azure](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Co znamená funkce je ve verzi preview?
Když je funkce ve verzi preview, znamená to, že jsme se aktivně vyhledávání zpětnou vazbu o tom, jak je funkce hello práci za vás. Funkce ve verzi preview je funkčně dokončení, ale je možné, že budete uděláme narušující, změňte v odpovědi toocustomer zpětné vazby. Doporučujeme vám, že nemáte závisí na funkce, která je ve verzi preview v provozním prostředí. Pokud máte jakékoli názor na funkce verze preview, dejte nám vědět prostřednictvím jednoho z způsobů kontaktování hello v [jak mohu požádat tým Microsoft Azure API Management hello otázku?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-hello-connection-between-hello-api-management-gateway-and-my-back-end-services"></a>Jak můžete zabezpečit hello připojení mezi bránou rozhraní API správy hello a Moje back endové služby?
Máte několik možností toosecure hello připojení mezi bránou rozhraní API správy hello a back endové služby. Můžete:

* Základní ověřování pomocí protokolu HTTP. Další informace najdete v tématu [nastavení konfigurace rozhraní API](api-management-howto-create-apis.md#configure-api-settings).
* Používá vzájemné ověřování SSL, jak je popsáno v [jak toosecure back endové služby pomocí ověření klientského certifikátu ve službě Azure API Management](api-management-howto-mutual-certificates.md).
* Vytvoření seznamu povolených IP použijte na back endové službě. Pokud máte Standard nebo Premium instance API Management vrstvě, hello IP adresu brány hello konstantní. Vaše tooallow seznamu povolených IP adres můžete nastavit tuto IP adresu. IP adresa hello instanci služby API Management můžete získat na hello řídicí panel v hello portálu Azure.
* Připojte vaše tooan instance API Management Azure Virtual Network.

### <a name="how-do-i-copy-my-api-management-service-instance-tooa-new-instance"></a>Jak lze zkopírovat Moje instance služby API Management instance tooa nové?
Pokud chcete, aby toocopy nové instance služby API Management instance tooa máte několik možností. Můžete:

* Použít hello zálohování a obnovení funkce ve službě API Management. Další informace najdete v tématu [jak tooimplement zotavení po havárii pomocí služby zálohování a obnovení v Azure API Management](api-management-howto-disaster-recovery-backup-restore.md).
* Vytvoření vlastního zálohování a obnovení funkce pomocí hello [rozhraní API REST API správy](https://msdn.microsoft.com/library/azure/dn776326.aspx). Použijte toosave hello REST API a obnovit hello entity ze hello instance služby, který chcete.
* Stažení konfigurace služby hello pomocí Git a nahrajte ho tooa novou instanci. Další informace najdete v tématu [jak toosave a konfigurovat konfigurace služby API Management pomocí Git](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Můžete spravovat instance Moje API Management prostřednictvím kódu programu?
Ano, rozhraní API správy programově spravovat pomocí:

* Hello [rozhraní API REST API správy](https://msdn.microsoft.com/library/azure/dn776326.aspx).
* Hello [knihovny SDK služby správy Microsoft Azure ApiManagement](http://aka.ms/apimsdk).
* Hello [nasazení služby](https://msdn.microsoft.com/library/mt619282.aspx) a [Správa služeb](https://msdn.microsoft.com/library/mt613507.aspx) rutiny prostředí PowerShell.

### <a name="how-do-i-add-a-user-toohello-administrators-group"></a>Jak přidat skupinu uživatelů toohello správci?
Zde je, jak můžete přidat skupinu uživatelů toohello správci:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Přejděte toohello skupinu prostředků, která má instance služby API Management hello chcete tooupdate.
3. Ve službě API Management, přiřaďte hello **rozhraní Api správy Přispěvatel** toohello uživatelské role.

Teď hello nově přidaná Přispěvatel můžete použít Azure PowerShell [rutiny](https://msdn.microsoft.com/library/mt613507.aspx). Tady je způsob toosign v jako správce:

1. Použití hello `Login-AzureRmAccount` toosign rutiny v.
2. Nastavit hello kontextu toohello předplatné, které má služba hello pomocí `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Získat jednu adresu URL přihlašování pomocí `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Použití hello URL tooaccess hello portál pro správu.

### <a name="why-is-hello-policy-that-i-want-tooadd-unavailable-in-hello-policy-editor"></a>Proč je hello zásady, že chcete tooadd není k dispozici v editoru zásad hello?
Pokud zásady hello, který chcete, aby existovala tooadd neaktivní nebo stínování editor zásad hello, ujistěte se, že jste ve správném oboru hello hello zásady. Každý prohlášení o zásadách je určená pro toouse v určité obory a zásad oddílů. části zásady hello tooreview a obory pro zásady, najdete v tématu Použití zásad hello kapitoly [zásady služby API Management](https://msdn.microsoft.com/library/azure/dn894080.aspx).

### <a name="how-do-i-use-api-versioning-in-api-management"></a>Jak používat správu verzí rozhraní API ve službě API Management?
Máte několik možností správy verzí toouse rozhraní API ve službě API Management:

* Ve službě API Management můžete nakonfigurovat různé verze rozhraní API toorepresent. Například můžete mít dvě různé rozhraní API, MyAPIv1 a MyAPIv2. Vývojář můžete zvolit, že hello verzi, která hello vývojáře chce toouse.
* Můžete také konfigurovat rozhraní API s adresou URL služby, která nezahrnuje segment verze, například https://my.api. Potom nakonfigurujte segment verze na každou operaci [přepisu adresy URL](https://msdn.microsoft.com/library/azure/dn894083.aspx#RewriteURL) šablony. Například můžete mít operace s [adresa URL šablony](api-management-howto-add-operations.md#url-template) názvem/Resource a [přepisu adresy URL](api-management-howto-add-operations.md#rewrite-url-template) šablona nazývá/v1/prostředků. Hodnota segmentu verze hello samostatně pro každou operaci, můžete změnit.
* Pokud chcete tookeep segment "Výchozí" verze v adrese URL služby hello rozhraní API, vybrané operací na nastavit zásady, které používá hello [nastavení back-end služby](https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBackendService) cestu požadavku back-end hello toochange zásad.

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Jak nastavím prostředí s více v jediného rozhraní API?
tooset více prostředí, například testovací prostředí a provozním prostředí, do jediného rozhraní API, máte dvě možnosti. Můžete:

* Různé rozhraní API hostitele na hello stejné klienta.
* Hostitele hello stejná rozhraní API na jiných klientů.

### <a name="can-i-use-soap-with-api-management"></a>Můžete použít protokolu SOAP s API Management?
[Průchozí SOAP](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) podpora je k dispozici. Správci mohou importovat hello WSDL své služby SOAP. Azure API Management se vytvoří front-end protokolu SOAP. Portál dokumentaci pro vývojáře, testovací konzoly, zásady a analýzy byly všechny dostupné pro služby SOAP.

### <a name="is-hello-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>Je hello API Management gateway IP adresu konstanta? Můžete je používat v pravidlech brány firewall?
Na úrovních Standard a Premium hello, hello veřejná IP adresa (VIP) hello API Management klienta je statický dobu jeho existence hello hello klienta na několik výjimek. změny IP adresy Hello za těchto okolností:

* Hello služby se odstraní a potom znovu vytvoří.
* předplatné služby Hello je [pozastaveno](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) nebo [upozorněn](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) (například nonpayment) a pak obnoveny.
* Přidat nebo odebrat virtuální sítě Azure (virtuální sítě můžete použít pouze na hello Developer a úroveň Premium).

Pro nasazení s více oblast hello změny místní adresy, pokud oblast hello vacated a pak obnoveny (můžete použít nasazení s více oblast jenom na úrovni Premium hello).

Premium vrstvy klientů, které jsou nakonfigurovány pro nasazení s více oblast přiřazené jednu veřejnou IP adresu podle oblastí.

IP adresa (nebo adresy, v nasazení s více oblast) můžete získat na stránce klienta hello hello portálu Azure.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Můžete nakonfigurovat serveru ověřování OAuth 2.0 pomocí služby AD FS zabezpečení?
jak tooconfigure serveru autorizace OAuth 2.0 zabezpečení služby Active Directory Federation Services (AD FS), najdete v části toolearn [pomocí služby AD FS ve službě API Management](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-toomultiple-geographic-locations"></a>V nasazení toomultiple geografických polohách jaké metody směrování používá rozhraní API Management?
API Management používá hello [metoda směrování provozu výkonu](../traffic-manager/traffic-manager-routing-methods.md#priority) v nasazení toomultiple geografických polohách. Příchozí provoz se směruje toohello nejbližší Brána rozhraní API. Pokud jedné oblasti přejde do režimu offline, příchozí provoz je automaticky směrované toohello další nejbližší brány. Další informace o metodách směrování v [metody směrování Traffic Manageru](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-toocreate-an-api-management-service-instance"></a>Můžete použít toocreate šablony Azure Resource Manager instanci služby API Management?
Ano. V tématu hello [Azure API Management Service](http://aka.ms/apimtemplate) šablony rychlý start.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Můžete použít certifikát podepsaný svým držitelem SSL pro back-end?
Ano. Zde je, jak toouse podepsaný svým držitelem vrstvy SSL (Secure Sockets) certifikátu pro back-end:

1. Vytvoření [back-end](https://msdn.microsoft.com/library/azure/dn935030.aspx) entity pomocí rozhraní API Management.
2. Sada hello **skipCertificateChainValidation** vlastnost příliš**true**.
3. Pokud již nechcete tooallow certifikáty podepsané svým držitelem, odstraňte hello back-end entity, nebo nastavte hello **skipCertificateChainValidation** vlastnost příliš**false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-tooclone-a-git-repository"></a>Proč se při tooclone úložiště Git zobrazí chybu ověřování?
Pokud použijete Správce přihlašovacích údajů Git, nebo pokud se pokoušíte tooclone úložiště Git pomocí sady Visual Studio, můžete narazit na známý problém s dialogové okno přihlašovací údaje Windows hello. Dialogové okno Hello omezuje znaků too127 délku hesla a se zkrátí hello Microsoft vytvořené heslo. Pracujeme na zkrátit hello heslo. Prozatím použijte Git Bash tooclone úložiště Git.

### <a name="does-api-management-work-with-azure-expressroute"></a>Funguje s Azure ExpressRoute API Management?
Ano. API Management pracuje s Azure ExpressRoute.

### <a name="why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them"></a>Proč jsme vyžaduje vyhrazenou podsíť ve stylu Resource Manager virtuální sítě, když API Management se nasadí do nich?
požadavek vyhrazené podsíť Hello rozhraní API správy pochází z hello fakt, který je založen na modelu nasazení Classic (PAAS V1 layer). Když jsme můžete nasadit do virtuální sítě Resource Manageru (V2 layer), nejsou toothat důsledky. Hello model nasazení Classic v Azure není pevně spojená s hello modelu Resource Manager a tak když vytvoříte prostředek v V2 vrstvy, vrstvy V1 hello neví o něm a může dojít problémům, například při toouse IP, která je již přidělena API Management tooa síťový adaptér (založený na V2).
Další informace o rozdílu Classic a Resource Manager modelů v Azure odkazovat příliš toolearn[rozdíl v modelech nasazení](../azure-resource-manager/resource-manager-deployment-model.md).

### <a name="what-is-hello-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet"></a>Jaká je velikost minimální podsíť hello potřebné při nasazování API Management do virtuální sítě?
velikost minimální podsítě Hello potřeby toodeploy API Management se [/29](../virtual-network/virtual-networks-faq.md#configuration), což je velikost hello minimální podsítě, který podporuje Azure.

### <a name="can-i-move-an-api-management-service-from-one-subscription-tooanother"></a>Lze přesunout z jednoho tooanother předplatné služby API Management?
Ano. Postup naleznete v tématu toolearn [přesunout prostředky tooa novou skupinu prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md).

### <a name="are-there-restrictions-on-or-known-issues-with-importing-my-api"></a>Existují známé problémy s importem Moje rozhraní API nebo omezení?
[Známé problémy a omezení](api-management-api-import-restrictions.md) otevřete API(Swagger) WSDL a WADL formáty.

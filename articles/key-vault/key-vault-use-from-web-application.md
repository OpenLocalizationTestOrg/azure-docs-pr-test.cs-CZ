---
title: "aaaUse Azure Key Vault z webové aplikace | Microsoft Docs"
description: "Použijte tento kurz toohelp zjistíte, jak toouse Azure Key Vault z webové aplikace."
services: key-vault
documentationcenter: 
author: adhurwit
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: adhurwit
ms.openlocfilehash: d5e2299e60b379c4e234d5cd6be03411c5a5c958
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-key-vault-from-a-web-application"></a>Použití Azure Key Vault z webové aplikace
## <a name="introduction"></a>Úvod
Použijte tento kurz toohelp zjistíte, jak toouse Azure Key Vault z webové aplikace v Azure. Provede vás procesem hello přístup k tajného klíče z Azure Key Vault, takže je možné ve webové aplikaci.

**Odhadovaný čas toocomplete:** 15 minut

Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).

## <a name="prerequisites"></a>Požadavky
toocomplete tento kurz, musíte mít hello následující:

* Identifikátor URI tajného klíče tooa v Azure Key Vault
* ID klienta a tajný klíč klienta pro webovou aplikaci, které jsou zaregistrované v Azure Active Directory, který má přístup tooyour Key Vault
* Webová aplikace. Jsme se, že se zobrazuje hello kroky pro aplikaci ASP.NET MVC nasazené v Azure jako webovou aplikaci.

> [!NOTE]
> Je nezbytné, že jste dokončili hello kroků uvedených v [Začínáme s Azure Key Vault](key-vault-get-started.md) pro tento kurz, který vám hello tooa identifikátor URI tajného klíče a hello ID klienta a tajný klíč klienta pro webovou aplikaci.
> 
> 

Hello webové aplikace, která bude mít přístup k hello Key Vault je hello jeden, který je zaregistrován ve službě Azure Active Directory a udělil přístup tooyour Key Vault. Pokud to není hello případ, přejděte zpět tooRegister aplikace v kurzu Začínáme hello a opakujte kroky hello zobrazeny.

Tento kurz je určen pro vývojářům webů, které pochopit základy hello vytváření webových aplikací v Azure. Další informace o službě Azure Web Apps, naleznete v části [přehled Web Apps](../app-service-web/app-service-web-overview.md).

## <a id="packages"></a>Přidání balíčků Nuget
Jsou dva balíčky, které webové aplikace potřebuje toohave nainstalována.

* Knihovna ověřování Active Directory - obsahuje metody pro interakci s Azure Active Directory a správa identity uživatele
* Azure Key Vault Library - obsahuje metody pro interakci s Azure Key Vault

Obě tyto balíčky můžete nainstalovat pomocí konzoly Správce balíčků pomocí příkazu hello Install-Package hello.

    // this is currently hello latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Upravit soubor Web.Config
Existují tři nastavení aplikace, které je třeba soubor web.config přidané toohello toobe následujícím způsobem.

    <!-- ClientId and ClientSecret refer toohello web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is hello URI for hello secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Pokud ale nebudete toohost aplikace jako webové aplikace Azure, měli byste přidat hello skutečné ClientId, sdílený tajný klíč klienta a identifikátor URI tajného klíče hodnoty toohello web.config. V opačném případě ponechte tyto fiktivní hodnoty, protože jsme přidáním hello skutečnými hodnotami v hello portál Azure pro další úroveň zabezpečení.

## <a id="gettoken"></a>Přidat metoda tooGet tokenu přístupu
V pořadí toouse hello API trezoru klíč musíte přístupový token. Hello klíč trezoru klienta zpracovává volání toohello trezoru klíč rozhraní API, ale můžete potřebovat toosupply její funkci, která se získá hello přístupový token.  

Následuje hello kód tooget přístupový token ze služby Azure Active Directory. Tento kód můžete přejít kdekoli v aplikaci. I jako tooadd Utils nebo EncryptionHelper třídy.  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property toohold hello secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //hello method that will be provided toohello KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed tooobtain hello JWT token");

        return result.AccessToken;
    }

> [!NOTE]
> Pomocí ID klienta a tajný klíč klienta je hello nejjednodušší způsob, jak tooauthenticate aplikaci Azure AD. A použití ve vaší webové aplikace je možné oddělení povinností a větší kontrolu nad vaší správy klíčů. Je však závislý na uvedení hello tajný klíč klienta v nastavení konfigurace, které pro některé můžou být jako rizikové jako uvedení hello tajný klíč, který má tooprotect v nastavení konfigurace. Níže najdete podrobné informace o tom, jak toouse ID klienta a certifikát místo ID klienta a tajný klíč klienta tooauthenticate hello aplikaci Azure AD.
> 
> 

## <a id="appstart"></a>Načtení hello tajný klíč na spuštění aplikace
Nyní jsme potřebovat code toocall hello trezoru klíč rozhraní API a načíst hello tajný klíč. Hello následující kód můžou být přepnuté odkudkoli, dokud se označuje jako předtím, než je nutné toouse ho. I tak, aby spustí jednou při spuštění a díky hello tajný klíč, které jsou k dispozici pro aplikace hello zavedla tento kód hello událost spustit aplikace v souboru Global.asax hello.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

    //I put a variable in a Utils class toohold hello secret for general  application use.
    Utils.EncryptSecret = sec.Value;



## <a id="portalsettings"></a>Přidání nastavení aplikace v hello portálu Azure (volitelné)
Pokud máte webové aplikace Azure nyní můžete přidat hello skutečnými hodnotami pro hello AppSettings v hello portálu Azure. Díky tomu skutečnými hodnotami hello nebude v souboru web.config hello ale chráněné prostřednictvím hello portál, kde můžete dělat samostatnou přístupovou ovládacího prvku. Tyto hodnoty se nahradí hello hodnoty, které jste zadali v souboru web.config. Ujistěte se, že hello názvy jsou hello stejné.

![Nastavení aplikace se zobrazí na portálu Azure][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Ověřování pomocí certifikátu místo tajný klíč klienta
Jiný způsob tooauthenticate aplikaci Azure AD je pomocí ID klienta a certifikát místo ID klienta a tajný klíč klienta. Následující jsou hello kroky toouse certifikát ve webové aplikaci Azure:

1. Získat nebo vytvořit certifikát
2. Hello certifikát přidružit aplikaci Azure AD
3. Přidat kód tooyour webové aplikace toouse hello certifikátu
4. Přidat certifikát tooyour webové aplikace

**Získejte nebo vytvořte certifikát** pro naše účely budeme testovacího certifikátu. Tady je několik příkazů, můžete použít v toocreate příkazový řádek vývojáře certifikát. Změňte adresář toowhere chcete hello cert soubory vytvořené.  Navíc pro hello počáteční a koncové datum hello certifikát, použijte hello aktuální datum plus 1 rok.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 03/07/2017 -e 03/07/2018 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Poznamenejte si hello koncové datum a hello hesla pro hello .pfx (v tomto příkladu: 07/31. prosinci 2016 a test123). Je nutné je níže.

Další informace o vytvoření testovacího certifikátu najdete v tématu [postup: vytvořit vaše vlastní testovací certifikát](https://msdn.microsoft.com/library/ff699202.aspx)

**Přidružení hello certifikát s aplikací Azure AD** nyní, když máte certifikát, je nutné tooassociate ji pomocí aplikace Azure AD. Hello portálu Azure v současné době nepodporuje tento pracovní postup; To lze provést pomocí prostředí PowerShell. Spusťte následující příkazy tooassoicate hello certifikát s aplikací Azure AD hello hello:

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $x509.Import("C:\data\KVWebApp.cer")
    $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    # If you used different dates for makecert then adjust these values
    $now = [System.DateTime]::Now
    $yearfromnow = $now.AddYears(1)

    $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $now -EndDate $yearfromnow

    $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get hello thumbprint toouse in your app settings
    $x509.Thumbprint

Po spuštění těchto příkazů, uvidíte hello aplikace ve službě Azure AD. Při hledání, ujistěte se, že vyberete "Moje společnost vlastní aplikace" místo "Aplikace společnost používá" v dialogovém okně hledání hello.

toolearn informace o objekty aplikací Azure AD a ServicePrincipal objekty, najdete v části [objekty aplikací a hlavní objekty služeb](../active-directory/active-directory-application-objects.md)

**Přidat kód tooyour webové aplikace toouse hello certifikát** nyní přidáme kód tooyour webové aplikace tooaccess hello certifikátu a použít jej pro ověřování.

Nejprve je kód tooaccess hello certifikátu.

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since hello test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


Všimněte si, že hello StoreLocation je CurrentUser místo LocalMachine. A že jsme se poskytuje "Nepravda" toohello nalezena metoda, protože se používá testovací certifikát.

Dále je kód, který používá hello CertificateHelper a vytvoří ClientAssertionCertificate, která je potřebná pro ověřování.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Zde je hello nový kód tooget hello přístupový token. Tím se nahradí hello gettoken – metoda výše. Uvedené ho jiný název pro usnadnění práce.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

I všechny tohoto kódu umístili do projektu webové aplikace Utils třídy pro snadné použití.

Poslední změna kódu Hello se hello metody Application_Start. Nejdřív potřebujeme toocall hello GetCert() metoda tooload hello ClientAssertionCertificate. A pak se nám změnit hello metoda zpětného volání, které jsme zadat při vytváření nové KeyVaultClient. Všimněte si, že tím se nahradí hello kód, který jsme měli výše.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Přidat certifikát tooyour webové aplikace prostřednictvím portálu Azure hello** přidání certifikátu tooyour webové aplikace je jednoduchý dvoustupňový proces. Nejprve přejděte toohello portálu Azure a přejděte tooyour webové aplikace. V okně Nastavení hello pro vaši webovou aplikaci klikněte na položku hello "vlastní domény a SSL". Na hello budou okno, které se otevře, budete moct tooupload hello certifikát, který jste vytvořili výše, KVWebApp.pfx, ujistěte se, že jste si heslo hello pro hello pfx.

![Přidání certifikátu tooa webové aplikace v hello portálu Azure][2]

Hello poslední věcí, je nutné, aby toodo je tooadd nastavení aplikace tooyour webovou aplikaci, která má název webu hello\_zatížení\_certifikáty a hodnotu *. Tím bude zajištěno, že se načtou všechny certifikáty. Pokud byste chtěli, že pouze hello tooload certifikáty, které jste odeslali a potom můžete zadat seznam jejich kryptografické otisky oddělených čárkami.

toolearn Další informace o přidání certifikátu tooa webové aplikace, najdete v části [pomocí certifikátů v aplikacích weby Azure](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)

**Přidat certifikát tooKey trezoru jako tajný klíč** místo odeslání vašeho certifikátu toohello webové aplikace služby přímo, můžete ukládat v Key Vault jako tajný klíč a nasadit ho z ní. Toto je popsané v následujícím příspěvku na blogu hello ve dvou krocích [nasazení Azure certifikát webové aplikace prostřednictvím Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)

## <a id="next"></a>Další kroky
Programátorské reference najdete v části [Azure Key Vault C# klienta referenční dokumentace rozhraní API](https://msdn.microsoft.com/library/azure/dn903628.aspx).

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png

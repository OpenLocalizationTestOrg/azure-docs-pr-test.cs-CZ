---
title: "Používat Windows virtuální počítač MSI pro přístup k Azure SQL"
description: "Kurz vás provede procesem použití Windows virtuálního počítače spravované služby Identity (MSI) pro přístup k Azure SQL."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: a3fa96b2c709b2b585519fa02a4e105010797947
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-sql"></a>Použití Windows virtuálního počítače spravované služby Identity (MSI) pro přístup k Azure SQL

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

V tomto kurzu se dozvíte, jak používat a spravovat službu Identity (MSI) pro virtuální počítač (VM) systému Windows pro přístup k serveru Azure SQL. Identita spravované služby je automaticky prováděna nástrojem Azure a umožňují ověření pro služby, které podporují ověřování Azure AD, aniž by museli přihlašovací údaje vložit do vašeho kódu. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Povolit MSI v systému Windows virtuálního počítače 
> * Udělit přístup virtuálních počítačů k serveru Azure SQL
> * Získání přístupového tokenu pomocí identity virtuálního počítače a použít ho k dotazování server Azure SQL

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k webu Azure Portal na adrese [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Vytvoření virtuálního počítače s Windows v nové skupině prostředků.

V tomto kurzu vytvoříme nový virtuální počítač s Windows.  Můžete také povolit MSI na existující virtuální počítač.

1.  Klikněte na tlačítko **Nový** v levém horním rohu portálu Azure Portal.
2.  Vyberte **Compute** a potom vyberte **Windows Server 2016 Datacenter**. 
3.  Zadejte informace o virtuálním počítači. **Uživatelské jméno** a **heslo** vytvořený, zde je přihlašovací údaje, které používáte k přihlášení k virtuálnímu počítači.
4.  Vyberte správnou **předplatné** pro virtuální počítač v rozevírací nabídce.
5.  Chcete-li vybrat nový **skupiny prostředků** , během které můžete vytvořit virtuální počítač vyberte **vytvořit nový**. Jakmile budete hotovi, klikněte na **OK**.
6.  Vyberte velikost virtuálního počítače. Pokud chcete zobrazit další velikosti, vyberte **Zobrazit všechny** nebo změňte filtr **Podporovaný typ disku**. Na stránce nastavení ponechejte výchozí hodnoty a klikněte na tlačítko **OK**.

    ![Obrázek alternativní text](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Povolit MSI na vašem virtuálním počítači 

Virtuální počítač MSI umožňuje získat přístupové tokeny z Azure AD, aniž by bylo třeba uvést přihlašovací údaje do vašeho kódu. Povolení MSI informuje Azure k vytvoření spravovaného identity pro virtuální počítač. V pozadí, povolení MSI provádí dvě věci: nainstaluje rozšíření virtuálního počítače MSI na vašem virtuálním počítači a umožňuje MSI ve službě Správce prostředků Azure.

1.  Vyberte **virtuálního počítače** , které chcete povolit MSI v.  
2.  V levém navigačním panelu klikněte na tlačítko **konfigurace**. 
3.  Zobrazí **identita spravované služby**. Registrovat a povolit soubor MSI, vyberte **Ano**, pokud chcete zakázat, vyberte Ne. 
4.  Ujistěte se, kliknete na tlačítko **Uložit** konfiguraci uložíte.  
    ![Obrázek alternativní text](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Pokud chcete zkontrolovat a ověřit, která rozšíření jsou na tomto virtuálním počítači, klikněte na tlačítko **rozšíření**. Pokud je povoleno MSI, pak **ManagedIdentityExtensionforWindows** se zobrazí v seznamu.

    ![Obrázek alternativní text](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Udělit váš virtuální počítač přístup k databázi v serveru Azure SQL

Nyní můžete udělit váš virtuální počítač přístup k databázi v serveru Azure SQL.  Pro tento krok můžete použít existující server SQL nebo vytvořte novou.  Pokud chcete vytvořit nový server a databáze pomocí portálu Azure, postupujte podle to [Azure SQL rychlý Start](~/articles/sql-database/sql-database-get-started-portal.md). Existují také – elementy QuickStart, použít rozhraní příkazového řádku Azure a prostředí Azure PowerShell v [dokumentaci k systému Azure SQL](~/articles/sql-database/index.yml).

Existují tři kroky pro udělení váš virtuální počítač přístup k databázi:
1.  Vytvoření skupiny ve službě Azure AD a nastavte MSI virtuálního počítače jako člena skupiny.
2.  Povolení ověřování Azure AD pro SQL server.
3.  Vytvoření **uživatele** v databázi, která reprezentuje skupinu Azure AD.

> [!NOTE]
> Obvykle vytvoříte obsahují uživatele, který mapuje přímo do Virtuálního počítače MSI.  Azure SQL v současné době nepovoluje objekt zabezpečení služby Azure AD, který představuje MSI virtuálních počítačů nejde mapovat na uživatele.  Jako podporovaných řešení můžete nastavte MSI virtuálního počítače jako člena skupiny služby Azure AD, a potom vytvořte uživatele v databázi, která reprezentuje skupinu.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-msi-a-member-of-the-group"></a>Vytvoření skupiny ve službě Azure AD a nastavte MSI virtuálního počítače jako člena skupiny

Můžete použít existující skupinu pro Azure AD, nebo vytvořte novou pomocí Azure AD PowerShell.  

Nejdřív nainstalujte [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modulu. Potom se přihlaste pomocí `Connect-AzureAD`a spusťte následující příkaz pro vytvoření skupiny a uložte ho do proměnné:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

Výstup vypadá následující text, který také ověří hodnotu proměnné:

```powershell
$Group = New-AzureADGroup -DisplayName "VM MSI access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM MSI access to SQL
```

V dalším kroku přidejte MSI Virtuálního počítače do skupiny.  Je třeba MSI **ObjectId**, které můžete získat pomocí Azure PowerShell.  Nejprve stáhnout [prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Potom se přihlaste pomocí `Login-AzureRmAccount`, a spusťte následující příkazy, které:
- Ujistěte se, že je váš kontext relace nastaven na požadované předplatné Azure, pokud máte několik snímků.
- Seznam dostupných prostředků ve vašem předplatném Azure, v ověřte správné prostředků skupiny a názvy virtuálních počítačů.
- Získat vlastnosti Virtuálního počítače MSI, pomocí příslušnými hodnotami pro `<RESOURCE-GROUP>` a `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

Výstup vypadá následující text, který také prozkoumá ID objektu zabezpečení objekt služby MSI Virtuálního počítače:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Nyní přidejte MSI virtuálních počítačů do skupiny.  Hlavní název služby můžete přidat pouze do skupiny pomocí Azure AD PowerShell.  Spusťte tento příkaz:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Pokud byste taky zkontrolovat členství ve skupině následně, výstup vypadá takto:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Povolení ověřování Azure AD pro SQL server

Teď, když jste vytvořili skupinu a přidat MSI virtuálních počítačů do členství, můžete [nakonfigurovat ověřování Azure AD pro SQL server](~/articles/sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-server) pomocí následujících kroků:

1.  Na portálu Azure vyberte **servery SQL** z levé navigaci.
2.  Klikněte na SQL server, aby byl povolen pro ověřování Azure AD.
3.  V **nastavení** části okna klikněte na tlačítko **správce Active Directory**.
4.  Na panelu příkazů klikněte na tlačítko **nastavit správce**.
5.  Vyberte uživatelský účet služby Azure AD budou správce serveru, a klikněte na **vyberte.**
6.  Na panelu příkazů klikněte na tlačítko **uložit.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Vytvořte uživatele v databázi, která reprezentuje skupinu Azure AD

Pro tento další krok, budete potřebovat [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Než začnete, může být také vhodné přečtení následujících článků pro informace o integraci služby Azure AD:

- [Univerzální ověřování se službou SQL Database a SQL Data Warehouse (SSMS podporu vícefaktorového ověřování)](~/articles/sql-database/sql-database-ssms-mfa-authentication.md)
- [Konfigurovat a spravovat ověřování Azure Active Directory s SQL Database nebo SQL Data Warehouse](~/articles/sql-database/sql-database-aad-authentication-configure.md)

1.  Spusťte aplikaci SQL Server Management Studio.
2.  V **připojit k serveru** dialogové okno, zadejte název serveru SQL server **název serveru** pole.
3.  V **ověřování** pole, vyberte **Universal s podpora vícefaktorového ověřování služby Active Directory -**.
4.  V **uživatelské jméno** pole, zadejte název účtu služby Azure AD, který nastavíte jako správce serveru, napříkladhelen@woodgroveonline.com
5.  Klikněte na tlačítko **Možnosti**.
6.  V **připojit k databázi** pole, zadejte název jiné než systémové databáze, kterou chcete konfigurovat.
7.  Klikněte na **Připojit**.  Dokončete proces přihlášení.
8.  V **Průzkumník objektů**, rozbalte **databáze** složky.
9.  Klikněte pravým tlačítkem na databázi uživatele a klikněte na tlačítko **nový dotaz**.
10.  V okně dotazu zadejte následující řádek a klikněte na **Execute** na panelu nástrojů:
    
     ```
     CREATE USER [VM MSI access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     Příkaz je úspěšně dokončena, vytvoření obsažené uživatele pro skupinu.
11.  Zrušte okna dotazu, zadejte následující řádek a klikněte na tlačítko **Execute** na panelu nástrojů:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM MSI access to SQL]
     ```

     Příkaz je úspěšně dokončena, udělení oprávnění pro čtení celou databázi obsažené uživatele.

Kód spuštěný ve virtuálním počítači teď můžete získat token z MSI a použití tokenu k ověření serveru SQL.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>Získání přístupového tokenu pomocí identity virtuálního počítače a použít jej k vyvolání Azure SQL 

Azure SQL nativně podporuje Azure AD ověřování, mohl přímo přijímat tokeny přístupu získat pomocí Instalační služby MSI.  Můžete použít **přístupový token** metodu vytváření připojení k SQL.  To je součástí Azure SQL integraci s Azure AD a se liší od zadávání přihlašovacích údajů v připojovacím řetězci.

Tady je příklad kódu .net otevření připojení k SQL pomocí přístupového tokenu.  Tento kód musí být spuštěn na virtuální počítač přístup k koncový bod virtuálního počítače MSI.  **Rozhraní .net framework 4.6** nebo vyšší je vyžadován v případě metody token přístupu.  Nahraďte hodnoty AZURE-SQL-SERVERNAME a databáze odpovídajícím způsobem.  Všimněte si, že ID prostředku pro Azure SQL je "https://database.windows.net/".

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call MSI endpoint.
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader and extract access token.
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

//
// Open a connection to the SQL server using the access token.
//
if (accessToken != null) {
    string connectionString = "Data Source=<AZURE-SQL-SERVERNAME>; Initial Catalog=<DATABASE>;";
    SqlConnection conn = new SqlConnection(connectionString);
    conn.AccessToken = accessToken;
    conn.Open();
}
```

Případně je rychlý způsob, jak otestovat koncová nastavení bez nutnosti psaní a nasazení aplikace na virtuální počítač pomocí prostředí PowerShell.

1.  Na portálu, přejděte na **virtuální počítače** a přejděte k virtuálnímu počítači Windows a v **přehled**, klikněte na tlačítko **Connect**. 
2.  Zadejte ve vaší **uživatelské jméno** a **heslo** pro které jste přidali při vytváření virtuálního počítače Windows. 
3.  Teď, když jste vytvořili **připojení ke vzdálené ploše** s virtuálním počítačem, otevřete **prostředí PowerShell** ve vzdálené relaci. 
4.  Pomocí prostředí PowerShell na `Invoke-WebRequest`, vytvořte žádost na místní koncový bod MSI k získání tokenu přístupu pro Azure SQL.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://database.windows.net/"} -Headers @{Metadata="true"}
    ```
    
    Převeďte odpověď z objektu JSON na objekt prostředí PowerShell. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Rozbalte tokenu přístupu z odpovědi.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  Otevře připojení k systému SQL server. Nezapomeňte nahradit hodnoty pro AZURE-SQL-název serveru a databáze.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    V dalším kroku vytvořit a odeslat dotaz na server.  Nezapomeňte nahradit hodnotu pro tabulku.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Zkontrolujte hodnotu `$DataSet.Tables[0]` pro zobrazení výsledků dotazu.  Blahopřejeme, jste dotaz databáze pomocí MSI virtuálního počítače a bez nutnosti zadat přihlašovací údaje!

## <a name="related-content"></a>Související obsah

- Přehled MSI najdete v tématu [identita spravované služby přehled](msi-overview.md).
- Další informace o [podporu Azure SQL pro ověřování Azure AD](~/articles/sql-database/sql-database-aad-authentication.md).
- Další informace o [konfigurace podpory Azure SQL pro ověřování Azure AD](~/articles/sql-database/sql-database-aad-authentication-configure.md).
- Další informace o [ověřování a přístup v systému SQL server](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions).

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.

## <a name="prepare-tooauthenticate-azure-resource-manager-requests"></a>Příprava tooauthenticate, které požadavky Azure Resource Manager
Je třeba ověřit všechny hello operace, které můžete provádět na prostředky pomocí hello [Azure Resource Manager] [ lnk-authenticate-arm] s Azure Active Directory (AD). Hello tooconfigure nejjednodušší způsob, jak je to toouse prostředí PowerShell nebo rozhraní příkazového řádku Azure.

Nainstalujte hello [rutin prostředí Azure PowerShell] [ lnk-powershell-install] než budete pokračovat.

Dobrý den, jak následující kroky zobrazit tooset až ověřování hesla pro aplikaci AD pomocí prostředí PowerShell. Tyto příkazy můžete spustit ve standardní relaci prostředí PowerShell.

1. Přihlaste se tooyour předplatného Azure, pomocí hello následující příkaz:

    ```powershell
    Login-AzureRmAccount
    ```

1. Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello předplatná Azure přidružená přihlašovacích údajů. Použijte následující příkaz toolist hello předplatná Azure k dispozici pro vás toouse hello:

    ```powershell
    Get-AzureRMSubscription
    ```

    Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toomanage služby IoT hub hello. Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. Poznamenejte si vaše **TenantId** a **SubscriptionId**. Budete potřebovat později.
3. Vytvořte novou aplikaci Azure Active Directory pomocí hello následující příkaz, nahraďte zástupného hello:
   
   * **{Zobrazovaný název}:** zobrazovaný název pro vaši aplikaci, jako **MySampleApp**
   * **{Adresu URL domovské stránky}:** hello URL hello domovské stránky aplikace jako **http://mysampleapp/home**. Tato adresa URL nemusí toopoint tooa reálné aplikaci.
   * **{Identifikátor aplikace}:** jedinečný identifikátor, jako **http://mysampleapp**. Tato adresa URL nemusí toopoint tooa reálné aplikaci.
   * **{Heslo}:** heslo používat tooauthenticate s vaší aplikací.
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. Poznamenejte si hello **ApplicationId** hello aplikace, které jste vytvořili. Budete potřebovat později.
5. Vytvořit nový objekt služby pomocí hello následující příkaz, nahraďte **{MyApplicationId}** s hello **ApplicationId** hello v předchozím kroku:
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Nastavení přiřazení role pomocí hello následující příkaz, nahraďte **{MyApplicationId}** s vaší **ApplicationId**.
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

Nyní dokončení vytváření hello aplikaci Azure AD, která vám umožní tooauthenticate z vaší vlastní aplikaci C#. Je třeba hello později v tomto kurzu následující hodnoty:

* TenantId
* SubscriptionId
* ApplicationId
* Heslo

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps

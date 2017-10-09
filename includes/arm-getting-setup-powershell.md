## <a name="setting-up-powershell-for-resource-manager-templates"></a>Nastavení prostředí PowerShell pro správce prostředků šablony
Než budete moct použít Azure PowerShell s Resource Managerem, budete potřebovat verze hello toohave správné prostředí Windows PowerShell a prostředí Azure PowerShell.

### <a name="verify-powershell-versions"></a>Ověření verze prostředí PowerShell
Ověřte, že máte prostředí Windows PowerShell verze 3.0 nebo 4.0. toofind hello verzi prostředí Windows PowerShell, zadejte tento příkaz na příkazovém řádku prostředí Windows PowerShell.

    $PSVersionTable

Zobrazí hello následující typy informací:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Ověřte, zda text hello hodnota z **PSVersion** je 3.0 nebo 4.0. Pokud ne, najdete v části [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) nebo [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Nastavení předplatného a účtu Azure
Pokud nemáte předplatné Azure, můžete si aktivovat vaší [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

Otevřete příkazový řádek prostředí Azure PowerShell a přihlaste se tooAzure pomocí tohoto příkazu.

    Login-AzureRmAccount

Pokud máte víc předplatných Azure, můžete seznam vašich předplatných Azure pomocí tohoto příkazu.

    Get-AzureRmSubscription

Zobrazí hello následující typy informací:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Hello aktuální předplatné můžete nastavit tak, že spustíte tyto příkazy v příkazovém řádku prostředí Azure PowerShell hello. Vše v rámci hello uvozovky, včetně hello < a > znaky, nahraďte hello správný název.

    $subscr="<SubscriptionName from hello display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Další informace o předplatných Azure a účty, najdete v části [postupy: připojení odběru tooyour](/powershell/azureps-cmdlets-docs#step-3-connect).


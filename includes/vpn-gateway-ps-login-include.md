Před zahájením této konfigurace, musíte být přihlášení tooyour účet Azure. Hello rutina vás vyzve k zadání hello přihlašovací údaje k účtu Azure. Po přihlášení stahování nastavení svého účtu tak, aby byly k dispozici tooAzure prostředí PowerShell. Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../articles/powershell-azure-resource-manager.md).

toolog, otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu. Použijte následující příklad toohelp, ke kterým se připojujete hello:

```powershell
Login-AzureRmAccount
```

Pokud máte víc předplatných Azure, zkontrolujte hello předplatná pro účet hello.

```powershell
Get-AzureRmSubscription
```

Zadejte hello předplatné, které chcete toouse.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```
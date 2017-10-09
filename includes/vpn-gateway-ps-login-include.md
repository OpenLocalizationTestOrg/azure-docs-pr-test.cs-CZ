<span data-ttu-id="088a3-101">Před zahájením této konfigurace, musíte být přihlášení tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="088a3-101">Before beginning this configuration, you must log in tooyour Azure account.</span></span> <span data-ttu-id="088a3-102">Hello rutina vás vyzve k zadání hello přihlašovací údaje k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="088a3-102">hello cmdlet prompts you for hello login credentials for your Azure account.</span></span> <span data-ttu-id="088a3-103">Po přihlášení stahování nastavení svého účtu tak, aby byly k dispozici tooAzure prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="088a3-103">After logging in, it downloads your account settings so they are available tooAzure PowerShell.</span></span> <span data-ttu-id="088a3-104">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../articles/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="088a3-104">For more information, see [Using Windows PowerShell with Resource Manager](../articles/powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="088a3-105">toolog, otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="088a3-105">toolog in, open your PowerShell console with elevated privileges, and connect tooyour account.</span></span> <span data-ttu-id="088a3-106">Použijte následující příklad toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="088a3-106">Use hello following example toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="088a3-107">Pokud máte víc předplatných Azure, zkontrolujte hello předplatná pro účet hello.</span><span class="sxs-lookup"><span data-stu-id="088a3-107">If you have multiple Azure subscriptions, check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="088a3-108">Zadejte hello předplatné, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="088a3-108">Specify hello subscription that you want toouse.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```
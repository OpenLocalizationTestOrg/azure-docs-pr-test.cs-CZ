### <a name="troubleshoot-azure-diagnostics"></a><span data-ttu-id="45bb8-101">Odstraňování potíží Diagnostiky Azure</span><span class="sxs-lookup"><span data-stu-id="45bb8-101">Troubleshoot Azure Diagnostics</span></span>

<span data-ttu-id="45bb8-102">Pokud se zobrazí následující chybová zpráva hello, poskytovatel prostředků Microsoft.insights hello není registrovaný:</span><span class="sxs-lookup"><span data-stu-id="45bb8-102">If you receive hello following error message, hello Microsoft.insights resource provider is not registered:</span></span>

`Failed tooupdate diagnostics for 'resource'. {"code":"Forbidden","message":"Please register hello subscription 'subscription id' with Microsoft.Insights."}`

<span data-ttu-id="45bb8-103">Poskytovatel prostředků hello tooregister, provádět hello proveďte kroky v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="45bb8-103">tooregister hello resource provider, perform hello following steps in hello Azure portal:</span></span>

1.  <span data-ttu-id="45bb8-104">V navigačním podokně hello na levé straně hello, klikněte na tlačítko *odběrů*</span><span class="sxs-lookup"><span data-stu-id="45bb8-104">In hello navigation pane on hello left, click *Subscriptions*</span></span>
2.  <span data-ttu-id="45bb8-105">Vyberte předplatné hello určené v hello chybová zpráva</span><span class="sxs-lookup"><span data-stu-id="45bb8-105">Select hello subscription identified in hello error message</span></span>
3.  <span data-ttu-id="45bb8-106">Klikněte na *Poskytovatelé prostředků*.</span><span class="sxs-lookup"><span data-stu-id="45bb8-106">Click *Resource Providers*</span></span>
4.  <span data-ttu-id="45bb8-107">Najde hello *Microsoft.insights* zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="45bb8-107">Find hello *Microsoft.insights* provider</span></span>
5.  <span data-ttu-id="45bb8-108">Klikněte na tlačítko hello *zaregistrovat* odkaz</span><span class="sxs-lookup"><span data-stu-id="45bb8-108">Click hello *Register* link</span></span>

![Zaregistrujte poskytovatele prostředků Microsoft.insights.](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

<span data-ttu-id="45bb8-110">Jednou hello *Microsoft.insights* je zaregistrován poskytovatele prostředků, opakujte konfiguraci diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="45bb8-110">Once hello *Microsoft.insights* resource provider is registered, retry configuring diagnostics.</span></span>


<span data-ttu-id="45bb8-111">V prostředí PowerShell Pokud se zobrazí následující chybová zpráva, hello potřebujete tooupdate vaší verzi prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="45bb8-111">In PowerShell, if you receive hello following error message, you need tooupdate your version of PowerShell:</span></span>

`Set-AzureRmDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

<span data-ttu-id="45bb8-112">Aktualizujte svou verzi prostředí PowerShell toohello listopadu 2016 (v2.3.0) nebo novější verze pomocí pokynů hello v hello [Začínáme pomocí rutin prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) článku.</span><span class="sxs-lookup"><span data-stu-id="45bb8-112">Update your version of PowerShell toohello November 2016 (v2.3.0), or later, release using hello instructions in hello [Get started with Azure PowerShell cmdlets](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) article.</span></span>

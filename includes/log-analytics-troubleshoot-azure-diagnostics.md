### <a name="troubleshoot-azure-diagnostics"></a>Odstraňování potíží Diagnostiky Azure

Pokud se zobrazí následující chybová zpráva hello, poskytovatel prostředků Microsoft.insights hello není registrovaný:

`Failed tooupdate diagnostics for 'resource'. {"code":"Forbidden","message":"Please register hello subscription 'subscription id' with Microsoft.Insights."}`

Poskytovatel prostředků hello tooregister, provádět hello proveďte kroky v hello portálu Azure:

1.  V navigačním podokně hello na levé straně hello, klikněte na tlačítko *odběrů*
2.  Vyberte předplatné hello určené v hello chybová zpráva
3.  Klikněte na *Poskytovatelé prostředků*.
4.  Najde hello *Microsoft.insights* zprostředkovatele
5.  Klikněte na tlačítko hello *zaregistrovat* odkaz

![Zaregistrujte poskytovatele prostředků Microsoft.insights.](./media/log-analytics-troubleshoot-azure-diagnostics/log-analytics-register-microsoft-diagnostics-resource-provider.png)

Jednou hello *Microsoft.insights* je zaregistrován poskytovatele prostředků, opakujte konfiguraci diagnostiky.


V prostředí PowerShell Pokud se zobrazí následující chybová zpráva, hello potřebujete tooupdate vaší verzi prostředí PowerShell:

`Set-AzureRmDiagnosticSetting : A parameter cannot be found that matches parameter name 'WorkspaceId'.`

Aktualizujte svou verzi prostředí PowerShell toohello listopadu 2016 (v2.3.0) nebo novější verze pomocí pokynů hello v hello [Začínáme pomocí rutin prostředí Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) článku.

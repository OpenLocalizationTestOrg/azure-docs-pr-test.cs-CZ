
### <a name="installation-failures"></a>Selhání instalace
| **Ukázková chybová zpráva** | **Doporučená akce** |
|--------------------------|------------------------|
|Chyba účty tooload se nezdařilo. Chyba: System.IO.IOException: nelze tooread data z hello připojení přenosu při instalaci a registraci hello CS serverem.| Ujistěte se, že TLS 1.0 je povolena v počítači hello. |

### <a name="registration-failures"></a>Selhání registrace
Chyby registrace můžete ladit kontrolou hello protokoly v hello **%ProgramData%\ASRLogs** složky.

| **Ukázková chybová zpráva** | **Doporučená akce** |
|--------------------------|------------------------|
|**09:20:06**: InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Zpráva: ACS50008: Token SAML je neplatný.<br>ID trasování: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>ID korelace: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97><br>Časové razítko: **2016-12-12 14:50:08Z<br>** | Zajistěte, aby byl čas hello systémových hodin není víc než 15 minut vypnout hello místní čas. Registrace hello toocomplete hello znovu spusťte instalační program.|
|**09:35:27** : DRRegistrationException při pokusu o tooget všechny trezoru zotavení po havárii pro vybraný certifikát hello:: vyvolala Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException, Exception.Message: ACS50008: SAML token je neplatný.<br>ID trasování: e5ad1af1-2d39-4970-8eef-096e325c9950<br>ID korelace: abe9deb8-3e64-464d-8375-36db9816427a<br>Časové razítko: **2016-05-19 01:35:39Z**<br> | Zajistěte, aby byl čas hello systémových hodin není víc než 15 minut vypnout hello místní čas. Registrace hello toocomplete hello znovu spusťte instalační program.|
|06:28:45: selhání toocreate certifikátu<br>06:28:45: Instalace nemůže pokračovat. Certifikát vyžaduje tooSite tooauthenticate, obnovení nelze vytvořit. Znovu spusťte instalaci. | Ujistěte se, že spouštíte instalaci jako místní správce. |

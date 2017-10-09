<!--author=SharS last changed: 03/17/2016-->

#### <a name="tooinstall-update-12-from-hello-azure-classic-portal"></a>tooinstall 1.2 aktualizace z hello portál Azure classic
1. V hello portál Azure classic, přejděte toohello **zařízení** a vyberte zařízení.
2. Přejděte příliš**zařízení** > **konfigurace**.
3. V části **síťových rozhraní**, nejdřív ověřte, zda máte alespoň jeden síťové rozhraní, které je povolen iSCSI. Vyhledejte hello síťové rozhraní (jiné než DATA 0), který má bránu přiřazen.
4. Zakažte hello síťové rozhraní, která má přiřazenou brány a uložit změny konfigurace hello. Všimněte si hello nastavení síťového rozhraní jsou uchovány a pokud znovu povolíte toto síťové rozhraní později, tak bude portál hello obnovit původní nastavení toohello.
5. Teď můžete [použít hello Azure classic portálu tooinstall aktualizace 1.2](#install-update-12-via-the-azure-classic-portal). Postupujte podle pokynů hello od kroku 3 tohoto postupu. Po nainstalování všech aktualizací hello můžete znovu povolit hello síťové rozhraní, které jste zakázali.


## <a name="create-a-ruby-application"></a>Vytvoření Ruby aplikace
Pokyny najdete v tématu [vytvořit Ruby aplikace na platformě Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-toouse-service-bus"></a>Konfigurace vaší aplikace tooUse Service Bus
toouse Service Bus, stáhnout a použít balíček hello Azure Ruby, která obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště hello.

### <a name="use-rubygems-tooobtain-hello-package"></a>Použijte RubyGems tooobtain hello balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).
2. Zadejte "gem instalace azure" hello příkazového okna tooinstall hello gem a závislosti.

### <a name="import-hello-package"></a>Importovat balíček hello
Pomocí svém oblíbeném textovém editoru, přidejte následující toohello horní části hello Ruby hello souboru, ve kterém chcete toouse úložiště:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Nastavení připojení služby Service Bus
Použití hello následující kód tooset hello hodnoty oboru názvů, název hello klíče, klíče, podepisující osoba a hostitele:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Nastavte hello obor názvů hodnotu toohello hodnotu, kterou jste vytvořili místo hello celou adresu URL. Například použít **"yourexamplenamespace"**, není "yourexamplenamespace.servicebus.windows.net".

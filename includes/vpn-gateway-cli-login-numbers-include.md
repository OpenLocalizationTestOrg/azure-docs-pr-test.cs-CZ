1. Přihlaste se tooyour předplatné s hello [az přihlášení](/cli/azure/#login) příkazů a postupujte podle hello na obrazovce pokynů. Další informace o přihlašování najdete v tématu [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).

  ```azurecli
  az login
  ```
2. Pokud máte více než jedno předplatné, zobrazí seznam hello předplatná pro účet hello.

  ```azurecli
  az account list --all
  ```
3. Zadejte hello předplatné, které chcete toouse.

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```
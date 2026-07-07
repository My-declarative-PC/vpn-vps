# VPS Ansible Setup

Настройка Ubuntu Server VPS:

- полное обновление системы;
- установка Docker;
- установка AmneziaWG;
- установка Linuxbrew;
- создание пользователя `timofey`;
- отключение SSH-доступа по паролю и для `root`;
- клонирование compose-репозитория в `/srv/homelab`.

Перед запуском укажите адрес сервера в `inventory.yml`.

Создайте локальный файл переменных:

```bash
cp vars.example.yml vars.yml
```

Заполните в `vars.yml` пользователя и SSH-ключи:

```yaml
main_user: timofey

main_user_ssh_public_keys:
  - "ssh-ed25519 AAAA... user@example"
```

`vars.yml` обязателен и не коммитится. Если VPS автоматически прокидывает
ключи в `/root/.ssh/authorized_keys`, `main_user_ssh_public_keys` можно оставить
пустым списком:

```yaml
main_user_ssh_public_keys: []
```

Playbook остановится до отключения SSH-доступа по паролю и для `root`, если не
найдёт ни root-ключей, ни ключей из `main_user_ssh_public_keys`.

```bash
ansible-playbook playbook.yml
```

При необходимости ключ можно переопределить через `-e`:

```bash
ansible-playbook playbook.yml \
  -e 'main_user_ssh_public_keys=["ssh-ed25519 AAAA... user@example"]'
```

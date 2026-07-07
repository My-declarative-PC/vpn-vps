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

Создайте локальный файл пользовательских переменных:

```bash
cp vars/user.example.yml vars/user.yml
```

Заполните пользователя в `vars/user.yml`:

```yaml
main_user: timofey
```

Заполните SSH-ключи там же:

```yaml
main_user_ssh_public_keys:
  - "ssh-ed25519 AAAA... user@example"
```

Локальный `vars/user.yml` обязателен и не коммитится. `vars/linuxbrew.yml` и
`vars/homelab.yml` версионируются, потому что содержат квазистатическую
конфигурацию. Если VPS автоматически прокидывает ключи в
`/root/.ssh/authorized_keys`, `main_user_ssh_public_keys` можно оставить пустым
списком:

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

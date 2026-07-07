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

```bash
ansible-playbook playbook.yml
```

Если VPS не прокидывает SSH-ключи в `/root/.ssh/authorized_keys`, передайте ключ явно:

```bash
ansible-playbook playbook.yml \
  -e 'main_user_ssh_public_keys=["ssh-ed25519 AAAA... user@example"]'
```

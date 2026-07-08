# VPS Ansible Setup

Настройка Ubuntu Server VPS:

- полное обновление системы;
- установка Docker;
- установка AmneziaWG;
- установка Linuxbrew;
- инициализация dotfiles через chezmoi;
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

Локальный `vars/user.yml` обязателен и не коммитится. `vars/linuxbrew.yml` и
`vars/homelab.yml` версионируются, потому что содержат квазистатическую
конфигурацию. SSH-ключи `main_user` управляются dotfiles через chezmoi.
Playbook отключает SSH-доступ по паролю и для `root` после применения dotfiles.

```bash
ansible-playbook playbook.yml
```

При необходимости пользователя можно переопределить через `-e`:

```bash
ansible-playbook playbook.yml \
  -e 'main_user=timofey'
```

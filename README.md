# ansible

Colección de **playbooks de Ansible** de referencia para automatizar tareas
habituales de administración de sistemas: actualización de equipos Linux y
Windows e instalación del cliente [ZeroTier](https://www.zerotier.com/).

> ℹ️ Repositorio de **referencia y aprendizaje**. Los playbooks están pensados
> para adaptarlos a tu propio inventario, no para ejecutarlos tal cual.

## Contenido

| Playbook | Descripción |
|---|---|
| `playbooks/update_rpi.yml` | Actualiza un equipo Debian/Ubuntu/Raspberry Pi (`apt`: update, upgrade, autoclean, autoremove). |
| `playbooks/update_windows.yml` | Actualiza Windows vía WinRM: busca, descarga e instala actualizaciones (roles `search` → `download` → `install`). |
| `playbooks/install_zerotier.yml` | Instala el cliente ZeroTier en Ubuntu (rol `install_zerotier`). |

### Roles

- **`install_zerotier`** — instala y configura el cliente ZeroTier (rol completo con `defaults`, `handlers`, `tasks`, `meta` y `tests`).
- **`search` / `download` / `install`** — fases de actualización de Windows con el módulo `ansible.windows.win_updates` (categorías *Security*, *Critical* y *Rollups*; `install` reinicia si es necesario).
- **`check` / `update` / `update-ubuntu`** — variantes auxiliares de comprobación y actualización.

## Inventarios

Edita los inventarios con tus hosts y credenciales antes de ejecutar:

- `inventario_linux.yml` — grupos `ubuntu` y `rpi`.
- `inventario_windows.yml` — grupo `windows` (conexión WinRM por el puerto 5986).

Los valores entre `<...>` (usuario, contraseña) y las IPs de ejemplo
(`192.168.1.x`) son **marcadores de posición**: sustitúyelos por los tuyos.
No guardes contraseñas en claro — usa [Ansible Vault](https://docs.ansible.com/ansible/latest/vault_guide/index.html).

## Uso

```bash
# Linux / Raspberry Pi
ansible-playbook -i inventario_linux.yml playbooks/update_rpi.yml --limit host_rpi

# Windows (requiere la colección ansible.windows)
ansible-galaxy collection install ansible.windows
ansible-playbook -i inventario_windows.yml playbooks/update_windows.yml --limit w1000-01

# Comprobar sin aplicar cambios (modo dry-run)
ansible-playbook -i inventario_windows.yml playbooks/update_windows.yml -C
```

## Requisitos

- Ansible (núcleo moderno).
- Para Windows: colección `ansible.windows` y WinRM habilitado en los hosts destino.

## Licencia

[MIT](LICENSE) © JaimeAlberto

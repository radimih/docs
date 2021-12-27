#### o1. Порядок определения хостов в директиве `hosts`

Если требуется указать несколько хостов или групп хостов в директиве `hosts`, то они определяются
в **алфавитном** порядке.

* play:
  ```yaml
  - hosts:
      - activemq_server
      - signing_server
      - work_calendar_server
    roles:
      ...
  ```

#### o2. Порядок определения тэгов в директиве `tags`

Тэги в директиве `tags` определяются в **алфавитном** порядке.

* play:
  ```yaml
  TODO: пример
  ```

#### o3. Порядок определения хостов и групп хостов в inventory

Хосты внутри групп и сами группы хостов определяются в **алфавитном** порядке.

* inventory:
  ```yaml
  TODO: пример
  ```

#### o4. Порядок определения переменных

Переменные в inventory, у роли в `defaults/main.yml` и `vars/main.yml` и глобально в плейбуке через директиву `vars`
определяются в **алфавитном** порядке.
При этом Ansible позволяет в определении переменных ссылаться на переменные, определённые далее по тексту.

Это правило не касается определения [входных переменных роли](), которые определяются в смысловом/произвольном порядке.

Примеры:

* inventory:
  ```yaml
  TODO: пример
  ```

* `vars/main.yml`:
  ```yaml
  TODO: пример
  ```

* плейбук:
  ```yaml
  TODO: пример
  ```

#### o5. Порядок определения директив

В зависимости от типа секции, [директивы](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html)
определяется в следующем порядке:

<table>
<thead>
<th>

Play

</th>
<th>

Role

</th>
<th>

Task

</th>
<tr>
</thead>
<tbody>
<td valign="top">

1. `hosts`
1. остальные [play-директивы](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html#play)
1. `pre_tasks`
1. `roles` / `tasks`
1. `post_tasks`
1. `handlers`
1. `tags`

```yaml
TODO: пример
```

</td>
<td valign="top">

1. `role`
1. `vars`
   * входные переменные роли в **произвольном** порядке
1. остальные [role-директивы](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html#role)
1. `tags`


```yaml
TODO: пример



```

</td>
<td valign="top">

1. `name`
1. модуль
   * параметры модуля в **произвольном** порядке
1. остальные [task-директивы](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_keywords.html#task)
1. `tags`

```yaml
TODO: пример



```

</td>
</tr>
<tr>
<td colspan="3">

1. Секция начинается с _ключевой директивы_: `hosts`, `role` или `name`.
1. Далее для секций **role** и **task** указывается _роль_ с входными переменными или _модуль_ с соответствующими параметрами.
1. Далее следуют _остальные директивы_ секции в произвольном порядке.
1. Далее для секции **play** следуют директивы в порядке их выполнения: `pre_tasks`, `roles`, `tasks`, `post_tasks`, `handlers`
1. Последней директивой в секции указывается `tags`, если необходимо задать тэги секции.

Рекомендации:

* Одновременное использование директив `roles` и `tasks` нежелательно из-за неочевидного порядка выполнения
  (сначала выполняется `roles`, затем `tasks`).  Совместно с `roles` лучше использовать директивы `pre_tasks`
  и `post_tasks`, если необходимо выполнить какие-либо задачи _до_ и _после_ выполнения ролей.
* Одновременное использование директив `pre_tasks`, `tasks` и `post_tasks` почти всегда лишено смысла.
  Лучше использовать одну директиву `tasks`. Единственное исключение — выполнение handler'ов. Они
  выполняются отдельно в конце каждой директивы `pre_tasks`, `tasks` и `post_tasks`.

</td>
</tr>
</tbody>
</table>

Допустимо использовать короткую версию записи role, если эта запись не будет слишком длинной:

```yaml
- { role: server_defaults, tags: [initial] }
- { role: manage_repository, thirdparty_repos_enable: true, tags: [initial, never] }
- { role: manage_docker, insecure_registries: ["172.17.21.6:4444"], registry_mirrors: ["http://172.17.21.6:4444"] }
```

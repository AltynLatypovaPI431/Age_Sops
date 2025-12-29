# Практическая работа по изучению и применению современных инструментов управления секретами в DevOps:
**Age** и **SOPS**.

---

## Цель работы

Целью данной практической работы является:
- изучение принципов шифрования конфигурационных файлов;
- освоение инструмента **Age** для асимметричного шифрования;
- освоение **SOPS** для безопасного хранения секретов в Git;
- шифрование и расшифровка YAML-файла с секретами;
- организация безопасного workflow без хранения приватных ключей в репозитории.

---

## Настройка SOPS
Файл .sops.yaml определяет правила шифрования:
creation_rules:
  - path_regex: 'secrets/database/.*\.enc\.yaml$'
    age: 'age1XXXXXXXXXXXXXX'

sops_version: 3.8.1

## Процесс шифрования
Генерация ключей Age:
```
age-keygen -o keys.txt
```

Приватный ключ хранится локально:
```
~/.config/sops/age/keys.txt
```

Шифрование YAML-файла:
```
sops --encrypt secrets/database/postgres.yaml > secrets/database/postgres.enc.yaml
```

## Процесс расшифровки
```
sops --decrypt secrets/database/postgres.enc.yaml
```

## Безопасность
-  Приватные ключи НЕ хранятся в Git
-  В репозитории только зашифрованные файлы
-  Используется .gitignore для защиты ключей

## Структура проекта

practice5-sops-demo/
├── .sops.yaml # Конфигурация SOPS
├── .gitignore # Игнорирование приватных ключей
├── public_keys/ # Публичные ключи команды
│ └── developer1.pub
└── secrets/ # Зашифрованные секреты
└── database/
└── postgres.enc.yaml

### Проверка установки Age и SOPS

<img width="366" height="82" alt="image" src="https://github.com/user-attachments/assets/f7988ca6-5da6-45b5-aee6-c8ec5d5032f2" />

### Шифрование файла с секретами

<img width="942" height="40" alt="image" src="https://github.com/user-attachments/assets/189a60c3-1181-458b-997b-5d238ab53bd0" />

### Просмотр зашифрованного файла

<img width="941" height="243" alt="image" src="https://github.com/user-attachments/assets/2f2ae776-244a-4983-88ce-175a131662f0" />

### Расшифровка секрета

<img width="913" height="306" alt="image" src="https://github.com/user-attachments/assets/54b44b61-c6f1-4b00-bc85-cdc0044c1193" />

## Вывод:
В ходе работы:
* установлен и настроен Age;
* установлен и настроен SOPS;
* создана конфигурация .sops.yaml;
* зашифрован YAML-файл с секретами;
* выполнена успешная расшифровка;
* обеспечено безопасное хранение секретов в Git.

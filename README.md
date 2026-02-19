graph TB
    subgraph Users ["6. Клиентский уровень (UI и доступ)"]
        Admin[Администраторы ОС/БД]
        Dev[DevOps/Разработчики]
        User[Рядовые пользователи]
        Browser[Браузерные расширения]
    end

    subgraph Auth ["2. Доступ и Аутентификация"]
        LDAP[AD / FreeIPA / SSO]
        MFA[2ФА / MFA]
        RBAC[Ролевая модель доступа]
    end

    subgraph Core ["1 & 5. Ядро Хранилища"]
        direction TB
        Enc[Шифрование AES-256 / Хеширование]
        Master[Мастер-ключ / Unseal]
        Secrets[(База секретов и паролей)]
        Dynamic[Динамические секреты / JIT]
        API[API / CLI / Скрипты]
    end

    subgraph Infrastructure ["1 & 5. Потребители секретов"]
        K8s[Kubernetes: Секреты k8s / Sidecars]
        CICD[CI/CD: GitLab / Jenkins]
        OS[Администраторы ОС / Серверы]
    end

    subgraph Security ["3 & 4. Безопасность и Регуляторика"]
        FSTEK[Сертификат ФСТЭК / Реестр ПО]
        SIEM[События аудита -> SIEM]
        Mon[Инфраструктурный мониторинг]
        Bastion[Интеграция с СКДПУ НТ]
    end

    %% Взаимодействия
    User & Admin & Dev --> Browser
    Browser --> Auth
    Auth --> RBAC
    RBAC --> Core
    
    Core <---- API
    API ----> K8s
    API ----> CICD
    API ----> OS
    
    Core ----> SIEM
    Core ----> Mon
    Core -.-> FSTEK
    Core <----> Bastion

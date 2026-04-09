ЗАДАНИЕ.
Игра "Герои" (текстовый hack-and-slash).
Необходимо реализовать несколько разных героев, которые могут атаковать друг друга.
Необходимо реализовать возможность сохранения и восстановления состояния героя.
Необходимо реализовать текстовый интерфейс для игры.
Над балансом игры можно не думать.

Также необходимо применять паттерны и принципы SOLID, желательно сделать слоёную архитектуру приложения, желательно заложить возможность дальнейшей доработки приложения.

Для начала нужно сделать диаграммы для понимания что и как будет происходить в игре

Диаграмма действий Игроков:

```mermaid
flowchart TD
    A["Запуск игры"]
    B["Первый игрок выбирает героя"]
    C["Второй игрок выбирает героя"]
    D["Начало битвы"]
    E["Ход первого игрока"]
    F{"Атака первого игока"}
    G["Ход второго игрока"]
    H{"Атака второго игрока"}
    I[Конец игры, победа первого игрока]
    J["Конец игры, победа второго игрока"]
   

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F -->|Второй игрок жив| G
    G --> H
    F -->|Второй игрок умер| I
    H -->|Первый игрок умер| J
    H -->|Первый игрок жив| F

```
Диаграмма действий Героев:

```mermaid
flowchart TD
    Start["Начало хода игрока"]

    ChooseAction{"Выбрать действие?"}
    Attack["Атаковать соперника"]

    CalcDamage["Рассчитать урон"]
    ApplyDamage["Нанести урон сопернику"]

    CheckEnemyHealth{"Health соперника > 0?"}
    EnemyTurn["Ход сопернику"]

    CheckPlayerAlive{"Health героя <= 0?"}
    DeathScreen["Герой погиб"]
    ContinueBattle["Продолжить битву"]

    EndBattle["Конец битвы"]

    Start --> ChooseAction
    ChooseAction -->|"Атака"| Attack
  

    Attack --> CalcDamage
    CalcDamage --> ApplyDamage
    ApplyDamage --> CheckEnemyHealth

    CheckEnemyHealth -->|"Да"| EnemyTurn
    CheckEnemyHealth -->|"Нет"| EndBattle

    EnemyTurn -->|"Соперник атакует"| CalcPlayerDamage["Рассчитать урон по герою"]
    CalcPlayerDamage --> ApplyPlayerDamage["Нанести урон герою"]
    ApplyPlayerDamage --> CheckPlayerAlive

    CheckPlayerAlive -->|"Да"| ContinueBattle
    CheckPlayerAlive -->|"Нет"| DeathScreen

    ContinueBattle --> Start
    DeathScreen --> EndBattle
```

Диаграмма состояния Героя:

```mermaid
stateDiagram-v2
    [*] --> Живой
    
    state Живой {
        [*] --> Значение
        Значение --> Attacking : Attack()
        Attacking --> Значение
        
        Значение --> TakingDamage : ReceiveDamage(dmg)
        TakingDamage --> Значение : HP > 0
        TakingDamage --> Мертвый : HP <= 0
    }
    
    state Мертвый {
        [*] --> Finished  direction LR
        Finished --> [*]
    }
    
    Живой --> Мертвый
```


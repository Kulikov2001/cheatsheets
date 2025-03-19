Для выполнения миграций в базе данных (например, добавление новой колонки `attachment` в таблицу `chat_history`) можно использовать инструменты для управления миграциями. Один из самых популярных инструментов — это **Alembic**, который работает с SQLAlchemy и позволяет автоматизировать процесс создания и применения миграций.

Вот пошаговая инструкция, как настроить Alembic и выполнить миграцию:

---

### **1. Установка Alembic**

Установите Alembic с помощью `pip`:

```bash
pip install alembic
```

---

### **2. Инициализация Alembic**

Создайте директорию для миграций, выполнив команду:

```bash
alembic init alembic
```

Это создаст папку `alembic` в вашем проекте, содержащую следующие файлы:
- `alembic.ini`: Конфигурационный файл.
- `alembic/env.py`: Файл, где Alembic подключается к вашей базе данных.
- `alembic/versions/`: Папка для хранения скриптов миграций.

---

### **3. Настройка `alembic.ini`**

Откройте файл `alembic.ini` и измените строку `sqlalchemy.url`, указав URL вашей базы данных. Например:

```ini
sqlalchemy.url = postgresql://username:password@localhost:5432/dbname
```

Если вы используете SQLite, URL может выглядеть так:

```ini
sqlalchemy.url = sqlite:///./test.db
```

---

### **4. Настройка `env.py`**

Откройте файл `alembic/env.py` и убедитесь, что Alembic знает о ваших моделях и метаданных SQLAlchemy. Найдите строку:

```python
target_metadata = None
```

Замените её на:

```python
from app.database import metadata  # Импортируйте ваш объект metadata
target_metadata = metadata
```

Теперь Alembic будет использовать ваши модели и метаданные для сравнения текущего состояния базы данных с желаемым.

---

### **5. Создание Миграции**

Чтобы создать новую миграцию, выполните команду:

```bash
alembic revision --autogenerate -m "add_attachment_column"
```

- `--autogenerate`: Автоматически генерирует изменения на основе ваших моделей.
- `-m`: Описание миграции (например, "add_attachment_column").

После выполнения этой команды в папке `alembic/versions/` появится новый файл миграции. Откройте его, чтобы проверить, что изменения корректны. Например:

```python
def upgrade():
    op.add_column('chat_history', sa.Column('attachment', sa.String(), nullable=True))

def downgrade():
    op.drop_column('chat_history', 'attachment')
```

---

### **6. Применение Миграции**

Чтобы применить миграцию к базе данных, выполните команду:

```bash
alembic upgrade head
```

Эта команда применит все неприменённые миграции, включая ту, которую вы только что создали.

---

### **7. Проверка Результата**

После выполнения миграции убедитесь, что база данных обновлена. Например:

- Для PostgreSQL:
  ```sql
  \d chat_history
  ```

- Для SQLite:
  ```bash
  sqlite3 test.db
  .schema chat_history
  ```

Вы должны увидеть новую колонку `attachment` в таблице `chat_history`.

---

### **8. Откат Миграции (Опционально)**

Если вам нужно откатить миграцию, выполните команду:

```bash
alembic downgrade -1
```

Это отменит последнюю миграцию.

---

### **9. Автоматизация Миграций**

Чтобы упростить процесс разработки, вы можете добавить команды для работы с миграциями в `Makefile` или `package.json`. Например:

#### **Makefile**
```makefile
migrate-generate:
	alembic revision --autogenerate -m "$(msg)"

migrate-upgrade:
	alembic upgrade head

migrate-downgrade:
	alembic downgrade -1
```

#### **Использование**
```bash
make migrate-generate msg="add_attachment_column"
make migrate-upgrade
```

---

### **10. Заключение**

Теперь у вас настроен Alembic для управления миграциями базы данных. Вы можете легко добавлять новые изменения в структуру базы данных, создавать миграции и применять их. Если возникнут вопросы или проблемы, дайте знать!

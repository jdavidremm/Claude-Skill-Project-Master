# Database

Last updated: 2025-11-05

---

## 📋 Template (Ne pas modifier cette section)

**Format strict pour chaque model :**

```
### ModelName
File: `path/to/file`
Table: `table_name`
Relations: → OtherModel (foreign_key)
Key fields: field1, field2, field3
```

**Exemple :**

```
### User
File: `models/User.py`
Table: `users`
Relations: (aucune)
Key fields: id, email, password_hash, created_at

### Todo
File: `models/Todo.py`
Table: `todos`
Relations: → User (user_id)
Key fields: id, user_id, title, completed, created_at
```

---

## Models

(Liste des models - À remplir)

# Задание 1
<img width="1007" alt="Снимок экрана 2024-11-28 в 23 54 02" src="https://github.com/user-attachments/assets/c3d25a45-a447-487b-a0b9-11a9e62ec3c6">

# Задание 2
<img width="871" alt="Снимок экрана 2024-11-28 в 23 58 02" src="https://github.com/user-attachments/assets/a11ca7e9-ec65-46e4-988f-760e5684c992">

# Задание 3
<img width="871" alt="Снимок экрана 2024-11-29 в 00 05 54" src="https://github.com/user-attachments/assets/3a79a626-fde7-4606-bf5a-04f8f16af491">
<img width="823" alt="Снимок экрана 2024-11-29 в 00 26 05" src="https://github.com/user-attachments/assets/467422c0-1200-4cc2-a0d8-cb0f17fedfcb">

# Задание 4

```python
import os
import subprocess


def get_all_git_objects():
    git_objects_dir = os.path.join(".git", "objects")
    if not os.path.exists(git_objects_dir):
        print("Каталог .git/objects не найден. Убедитесь, что это git-репозиторий.")
        return []

    objects = []
    for root, dirs, files in os.walk(git_objects_dir):
        dirs[:] = [d for d in dirs if d not in ("info", "pack")]
        for file in files:
            obj_hash = os.path.basename(root) + file
            objects.append(obj_hash)

    return objects


def cat_git_object(obj_hash):
    result = subprocess.run(
        ["git", "cat-file", "-p", obj_hash],
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE,
        text=True,
    )
    if result.returncode == 0:
        return result.stdout.strip()
    else:
        return f"Ошибка при чтении объекта {obj_hash}: {result.stderr.strip()}"


def main():
    objects = get_all_git_objects()
    if not objects:
        print("Нет объектов в репозитории.")
        return

    print(f"Найдено {len(objects)} объектов.")
    for obj_hash in objects:
        print(f"\n--- Содержимое объекта {obj_hash} ---")
        print(cat_git_object(obj_hash))


if __name__ == "__main__":
    main()
```

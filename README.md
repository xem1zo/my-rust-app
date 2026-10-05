# my-rust-app

Учебный проект с CI на GitHub Actions для Rust-приложения.

## 📌 Цель работы

Настроить CI для Rust-проекта: линтинг, форматирование, сборка (debug + release), тесты и сборка Docker-образа с сохранением в артефакт.

## 🎯 Что делает CI

Три параллельных/последовательных job'а:

1. **Lint & Format** — `cargo fmt --check` + `cargo clippy -D warnings`
2. **Build & Test** — `cargo check`, `cargo build`, `cargo build --release`, `cargo test`
3. **Build Docker Image** — сборка образа + сохранение `.tar.gz` в артефакт + тестовый запуск

## 📂 Структура проекта

```
my-rust-app/
├── .github/
│   └── workflows/
│       └── rust-ci.yml
├── src/
│   └── main.rs
├── Cargo.toml
├── Cargo.lock
├── Dockerfile
├── .dockerignore
├── .gitignore
└── README.md
```

## 🦀 Основной код

```rust
use std::io::{self, Write};

fn main() {
    println!("Hello from Rust in Docker! 🦀");
    io::stdout().flush().unwrap();
    std::thread::sleep(std::time::Duration::from_millis(200));
}
```

## 🚀 Запуск локально

### Через Docker

```bash
docker build -t my-rust-app:latest .
docker images | grep my-rust-app
docker run --rm my-rust-app:latest
```

Ожидаемый вывод:
```
Hello from Rust in Docker! 🦀
```

### Войти в контейнер

```bash
docker run -it --rm --entrypoint /bin/bash my-rust-app:latest
exit
```

## 📸 Результат запуска

![Вывод приложения](/img/terminal.png)

## ✅ Результат

При каждом push в `main` запускается CI.
На вкладке **Actions** отображаются 🟢 зелёные галочки.

**Ссылка на Actions:**  
https://github.com/xem1zo/my-rust-app/actions

Дополнительно сохраняется артефакт `docker-image` (`.tar.gz`) на 7 дней — его можно скачать и загрузить локально:

```bash
docker load -i docker-image.tar.gz
```

## 📝 Вывод

В ходе работы я освоил:

- Настройку CI для Rust-проектов в GitHub Actions
- Проверку форматирования (`rustfmt`) и линтинг (`clippy`)
- Сборку в debug и release режимах
- Многоэтапную сборку Docker-образа (builder + runtime)
- Кэширование сборки через `type=gha`
- Сохранение Docker-образа как артефакта
- Работу с непривилегированным пользователем в контейнере

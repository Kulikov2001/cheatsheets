`ss -atnp | grep <PORT>` найти какой порт занят каким процессом

`kill -9 <PID>` завершить процесс

`/var/www/<project>/venv/bin/gunicorn -c /var/www/<project>/gunicorn_config.py main:app` запуск fastapi uvicorn проекта

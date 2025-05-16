`ss -atnp | grep <PORT>` найти какой порт занят каким процессом

`kill -9 <PID>` завершить процесс

Windows аналог:

`netstat -ano | findstr :<PORT>`

`taskkill /F /PID <PID> /T` завершить процесс и его дочки

`/var/www/<project>/venv/bin/gunicorn -c /var/www/<project>/gunicorn_config.py main:app` запуск fastapi uvicorn проекта

windows аналог:

`netstat -ano | findstr :<PORT>`

`taskkill /F /PID <PID> /T` завершить процесс и его дочки

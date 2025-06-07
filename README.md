# 🚀 Tutorial de Instalación de Ticketz en EasyPanel 🌐🐳

¡Hola Dev! 🙋 Este tutorial te guiará paso a paso para instalar **Ticketz** (Frontend + Backend) en tu servidor gestionado por **EasyPanel**. 💠🧠

---

## 📦 Requisitos Previos

Antes de empezar, asegúrate de tener:

✅ Un servidor con **EasyPanel** instalado.

✅ Una red externa en EasyPanel (en este ejemplo: `easypanel-testos`).

✅ Acceso a los logs y consola del servidor.

---



Copia y Pega el siguiente `docker-compose.yml` 

```yaml
services:
  ticketz_backend:
    image: ghcr.io/ticketz-oss/ticketz-backend:latest
    container_name: ticketz-backend
    volumes:
      - ticketz_backend_public:/usr/src/app/public
      - ticketz_backend_private:/usr/src/app/private
    environment:
      - FRONTEND_HOST=$(EASYPANEL_DOMAIN)
      - BACKEND_PATH=/backend
      - EMAIL_ADDRESS=admin@example.com
      - TZ=America/Sao_Paulo
      - BACKEND_URL=https://$(EASYPANEL_DOMAIN)/backend
      - FRONTEND_URL=https://$(EASYPANEL_DOMAIN)
      - DB_DIALECT=postgres
      - DB_HOST=testos_postgres
      - DB_PORT=5432
      - DB_USER=psotgres
      - DB_NAME=postgres
      - DB_PASS=z3r0c00l
      - REDIS_URI=redis://default:5b46fe3ab5e22e0df88b@testos_redis:6379/1
      - REDIS_OPT_LIMITER_MAX=1
      - REDIS_OPT_LIMITER_DURATION=3000
      - USER_LIMIT=10000
      - CONNECTIONS_LIMIT=100000
      - CLOSED_SEND_BY_ME=true
      - VERIFY_TOKEN=ticketz
    networks:
      easypanel-testos:
        aliases:
          - backend

  ticketz_frontend:
    image: ghcr.io/ticketz-oss/ticketz-frontend:latest
    container_name: ticketz-frontend
    volumes:
      - ticketz_backend_public:/var/www/backend-public
    environment:
      - FRONTEND_HOST=https://$(EASYPANEL_DOMAIN)
      - BACKEND_PATH=/backend
      - BACKEND_HOST=$(EASYPANEL_DOMAIN)
      - BACKEND_PROTOCOL=https
      - BACKEND_PORT=443
      - EMAIL_ADDRESS=admin@example.com
    networks:
      - easypanel-testos

volumes:
  ticketz_backend_public:
  ticketz_backend_private:

networks:
  easypanel-testos:
    external: true
    name: easypanel-testos
```

---

## 🔐 Importante: Variables a Personalizar 🔧

👉 Cambia las siguientes variables por tus datos reales:

* `FRONTEND_HOST`: tu dominio en EasyPanel.
* `BACKEND_URL`, `FRONTEND_URL`: deben coincidir con tu dominio.
* `DB_HOST`, `DB_USER`, `DB_PASS`: deben coincidir con tu base de datos configurada en EasyPanel.
* `REDIS_URI`: revisa si Redis está corriendo en tu stack.

---

## 🏗️ Levantar los servicios


## 🦺 Verifica que todo funcione

* Accede a tu dominio en el navegador:
  👉 `https://tudominio.com` (reemplaza con el real)

* Usa EasyPanel para revisar logs o errores:
  💠 Dashboard > Containers > Logs

---

## 🎉 ¡Listo!

Ya tienes tu instancia de **Ticketz** corriendo en EasyPanel 💪✨
Ahora puedes gestionar tickets, usuarios y más desde tu propia plataforma.

---

## 📚 Recursos útiles

* 🐙 Repositorio GitHub: [ticketz-oss](https://github.com/ticketz-oss)
* 🧠 Docs oficiales (si existen): ¡busca en el repo!
* 💠 EasyPanel: [https://easypanel.io](https://easypanel.io)

---

¡Feliz deploy! 🚀🙌

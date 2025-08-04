# PM2 Command Cheat Sheet

PM2 is a popular Node.js process manager for production environments, enabling easy application management, monitoring, and automatic restarts.

---

## Installation
npm install pm2@latest -g

---

## Basic Process Management

| Command                         | Description                              | Example                              |
|--------------------------------|------------------------------------------|------------------------------------|
| `pm2 start <app>`               | Start an app                            | `pm2 start app.js`                  |
| `pm2 start <app> --name <name>`| Start app and assign a name             | `pm2 start app.js --name myapp`    |
| `pm2 stop <app|id>`             | Stop an app by name or ID                | `pm2 stop myapp` or `pm2 stop 0`  |
| `pm2 restart <app|id>`          | Restart an app                          | `pm2 restart myapp`                 |
| `pm2 reload <app|id>`           | Reload app (0 downtime)                  | `pm2 reload myapp`                  |
| `pm2 delete <app|id>`           | Delete an app from PM2                    | `pm2 delete myapp`                  |
| `pm2 list`                     | List all running processes               | `pm2 list`                         |
| `pm2 describe <app|id>`         | Show detailed info about a process      | `pm2 describe myapp`               |

---

## Logs & Monitoring

| Command                         | Description                              | Example                               |
|--------------------------------|------------------------------------------|-------------------------------------|
| `pm2 logs`                     | Show logs of all processes               | `pm2 logs`                          |
| `pm2 logs <app|id>`            | Show logs for specific app                | `pm2 logs myapp`                    |
| `pm2 flush`                   | Clear all log files                       | `pm2 flush`                        |
| `pm2 monit`                   | Monitor CPU and memory usage              | `pm2 monit`                        |

---

## Configuration & Setup

| Command                         | Description                              | Example                               |
|--------------------------------|------------------------------------------|-------------------------------------|
| `pm2 save`                    | Save current process list for resurrect  | `pm2 save`                         |
| `pm2 startup`                 | Generate startup script (run on server reboot) | `pm2 startup`                     |
| `pm2 resurrect`               | Restore previously saved processes      | `pm2 resurrect`                    |
| `pm2 update`                  | Reload PM2 with zero downtime            | `pm2 update`                      |

---

## Advanced

| Command                              | Description                              | Example                               |
|-------------------------------------|------------------------------------------|-------------------------------------|
| `pm2 scale <app> <instances>`       | Scale app to multiple instances          | `pm2 scale myapp 4`                  |
| `pm2 reload all`                   | Reload all running processes             | `pm2 reload all`                    |
| `pm2 stop all`                     | Stop all running processes               | `pm2 stop all`                      |
| `pm2 delete all`                   | Delete all processes from PM2 list       | `pm2 delete all`                    |

---

## Ecosystem File (process.json / ecosystem.config.js)

Use PM2 ecosystem file to define and manage multiple apps:

{
"apps": [
{
"name": "app1",
"script": "./app1.js",
"instances": "max",
"exec_mode": "cluster",
"watch": true,
"env": {
"NODE_ENV": "development"
},
"env_production": {
"NODE_ENV": "production"
}
}
]
}


Start all defined apps with:

pm2 start ecosystem.config.js


---

## Tips

- Use `pm2 logs` and `pm2 monit` to keep track of your apps in production.
- Use `pm2 save` after starting or modifying processes to ensure they restart on reboot.
- Generate startup scripts with `pm2 startup` for auto-launch after server restarts.

---

PM2 greatly simplifies Node.js production deployments with process management, load balancing, and monitoring features.

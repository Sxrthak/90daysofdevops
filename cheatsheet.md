# Docker cheat sheet

Every Docker command I used in Days 1–6 of #90DaysOfDevOps, grouped by topic, with the day I first used it.

Replace `<placeholders>` completely, including the angle brackets. In bash, `<` and `>` are redirection, not text.

## Images

| Command | What it does | Day |
|---|---|---|
| `docker build -t <name>:<tag> .` | Build an image from the `Dockerfile` in the current folder | [1](day-01-flask-docker-ec2/) |
| `docker build -f <file> -t <name>:<tag> .` | Build using a specific Dockerfile | [6](day-06-multi-stage-builds/) |
| `docker images` / `docker image ls` | List images | 1 |
| `docker image tag <src> <user>/<repo>:<tag>` | Add another name to an image (no copy, same ID) | [5](day-05-docker-hub/) |
| `docker push <user>/<repo>:<tag>` | Upload an image to Docker Hub | 5 |
| `docker rmi <image>` | Delete (or untag) an image | 5 |
| `docker history <image>` | Show the layers inside an image | 6 |

## Containers

| Command | What it does | Day |
|---|---|---|
| `docker run -d -p 80:80 <image>` | Run in the background, mapping host port → container port | 1 |
| `docker run -d --name <n> --network <net> -e KEY=val -v <vol>:<path> <image>` | Run with a name, network, environment variables and a volume | [2](day-02-docker-networking/)–[3](day-03-docker-volumes/) |
| `docker run --rm <image>` | Run and delete the container when it exits | 4 |
| `docker run --rm -v <vol>:/data alpine ls /data` | Look inside a volume | 3 |
| `docker ps` | List **running** containers | 1 |
| `docker ps -a` | List **all** containers, including stopped ones | 1 |
| `docker logs <container>` | Show a container's output (why it crashed) | 1 |
| `docker start <container>` | Start an existing stopped container | 1 |
| `docker stop <container>` | Stop a container | 3 |
| `docker restart <container>` | Restart a container | 3 |
| `docker rm <container>` | Delete a stopped container | 2 |
| `docker rm -f <container>` | Stop and delete in one step | 2 |
| `docker stop <c> && docker rm <c>` | Stop, then delete only if the stop succeeded (`&&`, not `&`) | 3 |
| `docker update --restart unless-stopped <container>` | Add a restart policy to an existing container | 1 |
| `docker exec <container> <command>` | Run a command inside a running container | 2 |
| `docker inspect <container\|volume>` | Show full details as JSON | 3 |

## Networks

| Command | What it does | Day |
|---|---|---|
| `docker network create <name> -d bridge` | Create a custom network (containers can reach each other by name) | 2 |
| `docker network ls` | List networks | 2 |
| `docker network inspect <name>` | Subnet, gateway and connected containers with their IPs | 2 |
| `docker network rm <name>` | Delete a network | 2 |

## Volumes

| Command | What it does | Day |
|---|---|---|
| `docker volume create <name>` | Create a named volume | 3 |
| `docker volume ls` | List volumes | 3 |
| `docker volume inspect <name>` | Show where the volume lives (Mountpoint) | 3 |

## Docker Compose

| Command | What it does | Day |
|---|---|---|
| `docker compose up` | Create and start everything (stays attached, showing logs) | [4](day-04-docker-compose/) |
| `docker compose up -d` | The same, in the background | 4 |
| `docker compose up -d --build` | Rebuild your image first | 4 |
| `docker compose ps` | Status of this project's containers (healthy?) | 4 |
| `docker compose logs -f <service>` | Follow a service's logs | 4 |
| `docker compose config` | Check the YAML and print the final config | 4 |
| `docker compose down` | Stop and remove containers and network (keeps volumes) | 4 |
| `docker compose down -v` | Also delete volumes, **which wipes the data** | 4 |
| `docker compose down --remove-orphans` | Also remove containers no longer in the file | 4 |
| `docker compose pull` | Download the newest versions of the images | 5 |
| `docker compose ls` | List Compose projects | 4 |

## Registry and login

| Command | What it does | Day |
|---|---|---|
| `docker login` | Browser login (creates an access token automatically) | 5 |
| `docker login -u <user>` | Log in with a username; paste an access token at the Password prompt | 5 |
| `docker logout` | Remove saved credentials | 5 |
| `until docker push <image>; do sleep 5; done` | Retry a push until it succeeds | 5 |

Credentials are saved base64-encoded (not encrypted) in `~/.docker/config.json`. Never share, screenshot or commit that file.

## Engine, setup and cleanup

| Command | What it does | Day |
|---|---|---|
| `docker version` / `docker info` | Client, server and engine details | 4 |
| `docker info --format '{{.Name}}'` | Which engine you're talking to | 4 |
| `docker context ls` / `docker context rm <name>` | List or remove engine contexts | 4 |
| `systemctl status docker` | Is the Docker engine running? (press `q` to exit) | 4 |
| `sudo systemctl restart docker` | Restart the engine | 5 |
| `sudo systemctl enable --now docker` | Start the engine now and on every boot | 4 |
| `docker system df` | Disk usage by images, containers, volumes and build cache | 6 |
| `docker builder prune` | Delete the build cache (including base images BuildKit pulled) | 6 |
| `docker system prune` | Delete unused containers, networks and dangling images. Careful with `-a` (all unused images) and `--volumes` (data) | 6 |

## Linux commands I used alongside Docker

| Command | Used for |
|---|---|
| `ssh -i <key>.pem ubuntu@<ip>` | Connecting to an EC2 server |
| `chmod 400 <key>.pem` | Making an SSH key private (on WSL this only works under `~`, not `/mnt/c`) |
| `sudo ss -ltnp \| grep <port>` | Finding what's using a port |
| `cat`, `vim`, `ls -a`, `pwd`, `cd -`, `history \| grep <word>` | Files and navigation |
| `cat > file <<'EOF'` … `EOF` | Creating a file from the terminal (heredoc) |
| `sed 's/old/new/' a > b` and `diff a b` | Making a modified copy of a file, then checking what changed |
| `jobs` / `fg` | Finding and resuming a program paused with Ctrl+Z |
| `wsl.exe --shutdown` | Restarting WSL (from Ubuntu with `.exe`, or from PowerShell without) |

## vim survival kit

| Keys | What it does |
|---|---|
| `i` | Start typing (INSERT mode) |
| `Esc` | Stop typing (back to normal mode) |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |
| `dd` | Delete the current line |
| `u` | Undo |
| `/text` + Enter | Search for `text` |
| `vim +11 file` | Open a file at line 11 |

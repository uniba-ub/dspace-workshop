# DSpace & Docker.
### An Introduction to containers for development & production.

The slides are available at the `slides/` subdirectory.
Just run `make slides` there and open your browser at `http://localhost:4100`.

The example tasks are at `tasks/`.

### Requirements
- git [git4windows](https://gitforwindows.org/) or [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

### Get Started
##### Local machine
```
git clone https://github.com/uniba-ub/dspace-workshop
cd dspace-workshop
ssh -i id_rsa_workshop root@${ip}
```

##### Continue on a server
```
git clone https://github.com/uniba-ub/dspace-workshop
cd dspace-workshop
cd tasks/task{1..} && cat README.md

```

### Docker docs
- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [docker-compose.yml Reference](https://docs.docker.com/compose/compose-file/)

You can get help about commands with `docker help` or `docker <Management Command> help`.
Docker-compose also provides help (`docker-compose help`)

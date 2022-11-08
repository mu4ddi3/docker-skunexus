# Skunexus-core Docker environment

## build (+with arg)
```bash
  docker-compose -f /home/user/code/docker-skunexus/docker-compose.yml -f /home/user/code/docker-skunexus/docker-compose.override.yml build
  docker-compose -f /home/user/code/docker-skunexus/docker-compose.yml -f /home/user/code/docker-skunexus/docker-compose.override.yml build --build-arg WITH_XDEBUG="false"
```

## up
```bash
  docker-compose -f /home/user/code/docker-skunexus/docker-compose.yml -f /home/user/code/docker-skunexus/docker-compose.override.yml up
```

## rebuild & up
```bash
  docker-compose -f /home/user/code/docker-skunexus/docker-compose.yml -f /home/user/code/docker-skunexus/docker-compose.override.yml up --build
```

## Using this setup for different projects

***IMPORTANT!*** I am assuming that all your projects exists in the same directory for example:
- /home/user/code/project-core
- /home/user/code/project-module-1
- /home/user/code/project-module-2
etc.

Then, without passing `PROJECT` environment variable,
path will be set for default profect (which you can set in override file -> `project-core` in below example):
```bash
...
volumes:
     - "/home/user/code/${PROJECT:-project-core}:/application"
...
```

If you want to use this setup for different project just pass its name as PROJECT variable before docker-compose command:

```bash
PROJECT=project-module-1 docker-compose -f /home/user/code/docker-skunexus/docker-compose.yml -f /home/user/code/docker-skunexus/docker-compose.override.yml up
```

or if you configured `dcupsku` alias for docker-compose, you can use:

```bash
PROJECT=project-module-1 dcupsku
```


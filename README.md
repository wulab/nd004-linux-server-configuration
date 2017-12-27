# Project: Linux Server Configuration

> A baseline installation of a Linux distribution on a virtual machine

## Table of Contents

- [Install](#install)
- [Features](#features)
- [Usage](#usage)
- [Contribute](#contribute)
- [License](#license)

## Features

- Apache HTTP Server with mod_wsgi
- OpenSSH
- PostgreSQL

## Install

Docker Compose is required to run the script. Please refer to [Install Docker
Compose](https://docs.docker.com/compose/install/) if you don't have one on
your system.

Then, clone this repository and generate keys for SSH:

    $ git clone https://github.com/wulab/nd004-linux-server-configuration.git
    $ cd nd004-linux-server-configuration
    $ ssh-keygen -f app/grader

## Usage

To run the app, issue the following command:

    $ docker-compose up

The command will create an `app` service, start SSH server on port 2200 and
HTTP server on port 80 and 443.

Using a browser, go to `http://localhost` and you'll see an example page. To
log into SSH server under `grader` account, run:

    $ ssh -i app/grader -p 2200 grader@localhost

Press `Ctrl+C` to stop the service. If you want to clean up everything, use:

    $ docker-compose down --rmi local --volumes

## Contribute

PRs not accepted. However, you can fork this repository and modify it under
your account.

## License

[MIT © Weera Wu.](LICENSE)

# Goal
Using your PC or laptop, or a school supplied one, install and configure
- a docker container for your future projects.
- PHP
- Visual Studio Code


# Requirements

For this task, you will require a PC/laptop (not a chromebook).

Once the docker container is deployed to the LTC Server, the remainder of the content can be developed and written on a Chromebook (through the use of GitHub's Codespaces)

# Instructions

## Docker Desktop

If Docker isn't installed, install it here - https://www.docker.com/products/docker-desktop/

![[docker.png]]

After downloading the installer, install Docker Desktop and open it to confirm it installed correctly. Accept the service agreement (after reading the terms).

Use the recommended settings

![[dockerSettings.png]]


Authenticate with Docker using your schools Google credentials.

![[dockerAuthentication.png]]



## Visual Studio Code

Install from here
https://code.visualstudio.com/

## PHP

> [!info]- Installing PHP on Windows
> 1. **Download PHP**:
>
> - Go to the PHP for Windows website.
> - Download the latest thread-safe ZIP file for your system (e.g., `php-8.x.x-Win32-vs16-x64.zip`).
> 
> ![[phpInstallWindows.png]]
> 1. **Extract the ZIP File**:
> - Extract the contents of the ZIP file to a directory (e.g., `C:\php`).
> 
> 1. **Configure PHP**:
> 
> - Rename `php.ini-development` to `php.ini`.
> - Open `php.ini` in a text editor and configure as needed (e.g., set `extension_dir` to `ext`).
> 
> 1. **Add PHP to System Path**:
> 
> - Open System Properties > Advanced > Environment Variables.
> - Add the PHP directory (e.g., `C:\php`) to the `Path` variable.
> 
> 1. **Verify Installation**:
> 
> - Open Command Prompt and type `php -v` to check the PHP version.


> [!info]- Installing PHP on macOS
> 
> 1. **Using Homebrew**:
>     
>     - Open Terminal.
>     - Install Homebrew if not already installed: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`.
>     - Install PHP: `brew install php`.
> 2. **Verify Installation**:
>     
>     - Type `php -v` in Terminal to check the PHP version.
> 


> [!info]- Installing PHP on Linux
> 
> #### Ubuntu/Debian
> 
> 1. **Update Package List**:
>     
>     - Open Terminal.
>     - Run `sudo apt update`.
> 2. **Install PHP**:
>     
>     - Run `sudo apt install php`.
> 3. **Verify Installation**:
>     
>     - Type `php -v` in Terminal to check the PHP version.
> 
> #### CentOS/RHEL
> 
> 1. **Enable EPEL Repository**:
>     
>     - Open Terminal.
>     - Run `sudo yum install epel-release`.
> 2. **Install PHP**:
>     
>     - Run `sudo yum install php`.
> 3. **Verify Installation**:
>     
>     - Type `php -v` in Terminal to check the PHP version.
> 
> #### Arch Linux
> 
> 1. **Update Package List**:
>     
>     - Open Terminal.
>     - Run `sudo pacman -Syu`.
> 2. **Install PHP**:
>     
>     - Run `sudo pacman -S php`.
> 3. **Verify Installation**:
>     
>     - Type `php -v` in Terminal to check the PHP version.
> 
These steps should help you get PHP up and running on your system. If you encounter any issues or need further assistance, feel free to ask!


# Docker Configuration



## `.devcontainer` file

This file configures the container so when it starts up, it automatically configures software to work for our project and environment.


In the repository folder, create a folder called `.devcontainer`. The folder starts with a `.` which may mean it is usually hidden in Windows Explorer/Finder etc.

Inside this folder, create a file called `devcontainer.json` and replace the contents with the following:

```json
// For format details, see https://aka.ms/devcontainer.json. For config options, see the
// README at: https://github.com/devcontainers/templates/tree/main/src/php
{
	"name": "PHP",
	// Or use a Dockerfile or Docker Compose file. More info: https://containers.dev/guide/dockerfile
	"image": "mcr.microsoft.com/devcontainers/php:1-8.2-bullseye",

	// Features to add to the dev container. More info: https://containers.dev/features.
	// "features": {},

	// Configure tool-specific properties.
	// "customizations": {},

	// Use 'forwardPorts' to make a list of ports inside the container available locally.
	// "forwardPorts": [
	// 	8080
	// ],
	"features": {
		"ghcr.io/warrenbuckley/codespace-features/sqlite:1": {},
		"ghcr.io/devcontainers/features/php:1": {}
	},
	// Use 'portsAttributes' to set default properties for specific forwarded ports. More info: https://code.visualstudio.com/docs/remote/devcontainerjson-reference.
	"portsAttributes": {
		"8000": {
			"label": "PHP Server",
			"onAutoForward": "notify"
		}
	},

	// Use 'postCreateCommand' to run commands after the container is created.
	"postCreateCommand": "bash .devcontainer/install-dependencies.sh",

	"postStartCommand": "php -S 0.0.0.0:8000"
	// Uncomment to connect as root instead. More info: https://aka.ms/dev-containers-non-root.
	// "remoteUser": "root"
}
```

## Install dependences file

In the same folder as the `devcontainder.json` file, create another file named `install-dependencies.sh`. Replace the contents with :

```bash
sudo apt update
sudo apt install sqlite3
```

This file is referenced in `devcontainer.json` , excuting the script after the Docker container has been initialised for the first time.

After creating the two files, your project should appear like this:

![[dockerDevContainer.png]]
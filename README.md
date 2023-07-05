<div>
<img align="left" style="margin: 0px 15px 0px 0px;" src="DoomRunner.64.png" alt="Doom Runner Icon" />

# Doom Runner on Flatpak

</div>

This project contains files to build [Doom Runner](https://github.com/Youda008/DoomRunner) as a Flatpak app.

The app include [GZDoom](https://zdoom.org/) engine to run WADs and files.

## How to build the app

### 1 - Prepare the environment
Ensure you have the following commands installed on your system:
- `git`
- `flatpak`
- `flatpak-builder`

Ensure you have the `flathub` repo enabled:
```shell
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

Clone this project on your computer:
```shell
$ git clone https://github.com/mbugni/DoomRunnerFlatpak.git
```

### 2 - Build and install the app
From the project directory run the command:
```shell
$ sudo flatpak-builder --verbose --install --install-deps-from=flathub --force-clean build \
  io.github.Youda008.DoomRunner.yaml
```

See [flatpak documentation](https://docs.flatpak.org/) for more info.

The first build can take a while (15 minutes or more), it depends on your machine performances. It compile and install the app, making it available for all users in your system.

### 3 - Run the app
You can run the Doom Runner launching it from your favorite desktop, or manually by using the `flatpak` command:
```shell
$ flatpak run io.github.Youda008.DoomRunner
```

## Basic usage
- Put data files in `~/.var/app/io.github.Youda008.DoomRunner/data`.
- Choose your favorite combination from main window
- Press the "Launch!" button
- Have fun!

#### GZDoom options
 GZDoom engine defaults are in file `~/.var/app/io.github.Youda008.DoomRunner/.config/gzdoom/gzdoom.ini`

## Advanced usage

### Add a Flatpak engine
Let's assume you want to use an engine with app identifier `my.app.Engine`. Remember that Flatpak apps are sandboxed: the engine cannot access to Doom Runner files and vice versa.

#### 1 - Grant permissions
Ensure the engine app can access to a shared folder (eg `~/doom`):
```shell
$ flatpak override my.app.Engine --filesystem=$HOME/doom
```

#### 2 - Create a launcher script
Create a launcher script (eg `~/doom/engine/myapp.sh`) in the shared folder like the following:
```shell
#!/usr/bin/bash
exec /usr/bin/flatpak run my.app.Engine "$@"
```

#### 3 - Add the engine
Add the engine to the Doom Runner list:
- select the file `~/doom/engine/myapp.sh` as "Executable path"
- provide an existing "Config directory" (eg `~/doom/config`)

#### 4 - Provide game files
Put data files in the shared folder (eg `~/doom/data`), so both Doom Runner and the engine can access them.

Your shared folder should look like this
```
~/doom/
├── config
│   └── myapp
│       └── doomsav0.dsg
├── data
│   ├── freedoom1.wad
│   └── freedoom2.wad
└── engine
    └── myapp.sh
```

Repeat the above steps for each Flatpak engine you want to add.
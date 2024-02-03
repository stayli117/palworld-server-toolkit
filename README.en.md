# palworld-server-toolkit
<p align="center">
   <a href="/README.md">简体中文</a> | <strong>English</strong>
</p>

<p align="center">
Tools for Palworld servers
</p>

<p align='center'>
<img alt="GitHub Repo stars" src="https://img.shields.io/github/stars/magicbear/palworld-server-toolkit?style=for-the-badge">&nbsp;&nbsp;
<img alt="Python" src="https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue">&nbsp;&nbsp;
</p>


### Prerequisites

1. Python 3.9 or newer.
    - Windows users: You can install [Python 3.12 from the Microsoft Store](https://apps.microsoft.com/detail/9NCVDN91XZQP) or from [python.org](https://www.python.org/)

2. Use `git submodule update --init --recursive` or Download [https://github.com/cheahjs/palworld-save-tools](https://github.com/cheahjs/palworld-save-tools) and put `palworld-save-tools` contents to `save_tools` directory

---

## list.py
Tool for list player's nickname, steam id on server

- List player - `python3 list.py` in working directory `/PalSaved/SaveGames/0/<server id>/Players`
- Check player detail - `python3 list.py <PLAYER HEX UID>`


---

## palworld-cleanup-tools.py

This tools is for cleanup the unreference item, rename the player name, migrate player and delete the player.

> ### :warning: This tool is experimental. Be careful of data loss and *always* make a backup.
> Without -o params, default save file is `Level_fixed.sav`

- For cleaning the capture log in guild, use the follow command `python palworld-cleanup-tools.py --fix-missing --fix-capture Level.sav`

- For modifiy the `Level.sav` file, use the follow command
`python -i palworld-cleanup-tools.py -o Level.sav Level.sav`

	- `ShowPlayers()` - List the Players
	- `FixMissing()` - Remove missing player instance
	- `FixCaptureLog()` - Remove invalid caputre log in guild
	- `FixDuplicateUser()` - Remove duplicated user data
	- `ShowGuild()` - List the Guild and members
	- `BindGuildInstanceId(uid,instance_id)` - Update Guild binding instance for user
	- `RenamePlayer(uid,new_name)` - Rename player to new_name
	- `DeletePlayer(uid,InstanceId=None, dry_run=False)` - Wipe player data from save InstanceId: delete specified InstanceId
	- `EditPlayer(uid)` - Allocate player base meta data to variable `player`
	- `OpenBackup(filename)` - Open Backup Level.sav file and assign to `backup_wsd`
	- `MigratePlayer(old_uid,new_uid)` - Migrate the player from old PlayerUId to new PlayerUId
	- `CopyPlayer(old_uid,new_uid, backup_wsd)` - Copy the player from old PlayerUId to new PlayerUId `backup_wsd` is the OpenBackup file, `wsd` is current file
	- `Save()` - Save the file and exit


### Function screenshot

![](./docs/img/ShowPlayer.png)
![](./docs/img/ShowGuild.png)

### Operate Sample

- Migrate difference server to single server sample:

	1. stop the server
	1. Copy old server `Level.sav` to `SaveGames/0/<Server ID>/Old-Level.sav`
	1. Copy old server `Players/xxxxxxxx000000000000000000000000.sav` to `SaveGames/0/<Server ID>/Players/xxxxxxxx000000000000000000000000.sav`
	1. Use interactive mode `python -i palworld-cleanup-tools.py Level.sav`
	1. Use following command `OpenBackup("Old-Level.sav")`
	1. Next step `CopyPlayer("xxxxxxxx-0000-0000-0000-000000000000", "xxxxxxxx-0000-0000-0000-000000000000", backup_wsd)` for every require to migrate player
		> :warning: UUID can be the same, the user data will be copy from `backup_wsd`
	1. Next step `Save()`
	1. Finally, remove all the old `xxxxxxxx000000000000000000000000.sav` and `Old-Level.sav`, rename `Level_fixed.sav` to `Level.sav` and start the Palworld Server.

- Migrate User

	1. stop the server
	1. Use interactive mode `python -i palworld-cleanup-tools.py Level.sav`	1. Use following command `MigratePlayer("xxxxxxxx-0000-0000-0000-000000000000","yyyyyyyy-0000-0000-0000-000000000000")`
	1. Next step `Save()`
	1. Finally, rename `Level_fixed.sav` to `Level.sav` and start the Palworld Server.


---

## taskset.py
Tools for set cpu affinity to CPU performance core

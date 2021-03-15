Running your own mstdn-ebooks bot
========================
*Last updated 2020-03-14*

## Preface
The easiest way to get an ebooks bot is to use the freely available [FediBooks](https://fedibooks.com) service. You don't have to install anything on your computer, and everything is handled through an intuitive web UI. If you'd like to host an ebooks bot on your own computer, however, you can use mstdn-ebooks.

I provide a paid mstdn-ebooks hosting service, although I strongly recommend using FediBooks instead. If, however, you do want to use this paid service, you can do so through [my Patreon](https://patreon.com/Lynnesbian).

## Introduction
This is a guide on how to run your own ebooks bot on Mastodon, with little to no programming required. Other instance types, such as Pleroma, are somewhat supported. Check [this table](https://github.com/Lynnesbian/mstdn-ebooks/blob/master/README.md#compatibility) to see if your instance type will work. 

Your bot will not have advanced shitposting capabilities like [lynnesbian_ebooks](https://botsin.space/@lynnesbian_ebooks) does, such as generating memes. It will simply learn from your posts and generate toots using a markov chain (kind of like predictive text on your phone), and optionally reply when mentioned.

## Regarding Windows
This guide is primarily written from a Linux/macOS perspective, although care has been taken to ensure that it will work for Windows too. I can't make any guarantees, however, since I don't use Windows.

## Requirements
### Mandatory
- Python **3** (not Python 2). If you're on Windows of macOS, you can get this from [python.org](https://python.org)
	- **Important information for Windows and Linux users:** 
		- If you are using **Linux**, do **not** install Python 3 from python.org. Use your package manager. If Python 3 isn't in your package manager, you should use python.org, but I doubt your distribution won't have it. Here are some example commands to install Linux using the package manager (`sudo` may be required where appropriate):
			- `pacman -S python`
			- `apt install python3`
			- `dnf install python3`
			- `zypper in python3`
		- If you are using **Windows**, the first screen of the installer will have a checkbox labeled "Add Python 3.X to PATH". You **must** check this box.

![A screenshot showing the checkbox checked.](https://lynnesbian.space/res/ceres/sshot_2018-12-03_at_08-33-43-1543790023.png)

If your installation of Python does not come with `pip` (this is highly unlikely), you'll need that too. Try installing it from your package manager if you're on Linux. Typical package names are `python-pip` or `python3-pip` If not, [follow these instructions.](https://packaging.python.org/tutorials/installing-packages/#ensure-you-can-run-pip-from-the-command-line)
### Optional
- git, to make updating easier
- docker, if you want to run the bot with Docker. If you don't know what this means, you can skip this. The Docker guide is written from a Unix (macOS/Linux) perspective, so if you're using Windows, it will not work without modification.

## Setup
If you'd like to run the bot with Docker, scroll down to the Docker section of this guide.
### Linux/macOS/Windows
- Open the terminal/command prompt:
	- On Linux, try pressing Ctrl-Alt-T
	- On macOS, open "Terminal.app" from the Utilities folder in the Applications folder
	- On Windows, press Win-R, type `cmd`, and press enter
- Use `cd` to change to the directory you'd like to put the bot in. You can use `ls` (or `dir` on Windows) to see a list of directories, and you can press the "tab" key to autocomplete a name (may be case sensitive). For example, to move to `C:\Users\lynne`, you would type `cd C:\Users\lynne`.
- If you've installed git, run`git clone https://github.com/Lynnesbian/mstdn-ebooks`
- If not: 
	- Download and extract [this zip file](https://github.com/Lynnesbian/mstdn-ebooks/archive/master.zip)
	- Move the folder, called `mstdn-ebooks`, to the directory you'd like to put the bot in. This is the same folder you used `cd` to change to earlier.
- Try running `pip3 --version`.
	- If it works, and you get something like `pip 18.0 from /usr/lib/python3.7/site-packages/pip (python 3.7)`, continue as normal
	- If it doesn't work, and you get something like `bash: pip3: command not found` or `'pip3' is not recognised as an internal or external command, operable program or batch file.`, replace `pip3` with `pip` in the next few commands.
	- If it *still* doesn't work, you may have forgotten to add Python to your PATH. The Windows installer provides a checkbox for this, and you *must* check it during installation. Reinstall Python.
- `cd mstdn-ebooks`
- `pip3 install --user -r requirements.txt` (or `pip`)
	- If this fails, try `pip3 install -r requirements.txt`
- If you haven't already created an account on botsin.space (or the instance you want to use), [do so now](https://botsin.space/auth), using an email account that you can access
- Accept the verification email
- Sign in
- **Important:** Unfollow every account your bot is following. Many instances set new accounts to follow people by default. For example, new accounts on botsin.space automatically follow the botsin.space admin. If you don't unfollow these users, you'll end up downloading and learning from other people's toots. If you accidentally run the bot without unfollowing others first, unfollow the account, and then delete `toots.db`.
- Follow all of your accounts (or at least, all the ones you want it to get toots from). If any of your accounts are locked, you need to approve the follow request.
	- If they don't show up when searching, enter the full name (e.g. `@me@instance.com` instead of just `@me`)
- If you want to use another instance instead of botsin.space, you **must** do the following:
	- Create an account there instead
	- Edit `config.json` in the `mstdn-ebooks` folder and replace `https://botsin.space` with your instance
- `python3 ./main.py`
	- If `python3` cannot be found or `is not recognised as blah blah blah`, replace `python3` with `python` in all subsequent steps
	- If `python` doesn't work either, close the terminal window and open it again.
	- If you closed and reopened the window and it *still* doesn't work:
		- If you're on Windows, make sure that you checked the "Add to PATH" box when installing Python
- The command will output a URL. Open this URL using the same browser that's logged into your bot account.
- Authorise the request.
- Wait for the bot to download your toots. This can take quite a while.
- If the command succeeds, you will see something like the following screen:

![The command ends with "Done!"](https://lynnesbian.space/res/ceres/sshot_2018-12-03_at_08-06-11-1543788371.png)
- Run `python3 ./gen.py --simulate`
- This command should only take a few seconds to complete, and it should output some gibberish that sounds sort of like you.

![Gibberish](https://lynnesbian.space/res/ceres/sshot_2018-12-03_at_08-08-48-1543788528.png)
- If this succeeds, try running `python3 ./gen.py`, without the `--simulate` option. It will take a little longer than last time.
Your bot should now post its first toot! You can run `python3 ./gen.py` at any time to make it post. It will download your latest toots whenever you run `main.py`.
I'd appreciate it if you linked to the GitHub page in the bot's bio so that people know where to find the code, but it's not necessary!

If you'd like to tweak your bot's configuration, a table explaining the options in `config.json` is available [here](https://github.com/Lynnesbian/mstdn-ebooks/blob/master/README.md#configuration)

### Using Docker
Thanks to [@ben@lubar.me](https://mastodon.lubar.me/@ben), it is possible to run the ebooks bot using Docker! This guide is Linux/macOS only.

- Create an account for your bot wherever you'd like, such as at botsin.space.
- **Important:** unfollow all accounts your bot is following
- Follow your own account(s)
- Run the following command:
	```
	docker run -ti --name my_ebooks_bot \
	-e ebooks_site=https://botsin.space \
	-v "$HOME/my_ebooks_bot_data_folder:/ebooks/data" \
	--restart unless-stopped lynnesbian/mstdn-ebooks
	```
  - The `-e` option can be skipped if your bot is on botsin.space.
  - The `-v` option can be skipped to leave the data volume unnamed.
- Follow the prompts. The container will automatically die and restart when it is ready to run without input.
- Congratulations, you now have a fully-working ebooks bot.

## Updating the ebooks software
### Git
This is easy! just run `git pull` in the bot folder. Then, restart the `reply.py` service (if you set it up.)
### Docker
    docker pull lynnesbian/mstdn-ebooks
    docker stop my_ebooks_bot
    docker rename my_ebooks_bot my_ebooks_bot_temp
    docker run -d --name my_ebooks_bot -e ebooks_site=https://botsin.space --volumes-from my_ebooks_bot_temp --restart unless-stopped lynnesbian/mstdn-ebooks
    docker rm my_ebooks_bot_temp
### Downloading the zip
Simply re-download the zip file linked above, open the zip, and copy the new files over the old files, replacing all files *except config.json.* If you delete config.json, you will have to log in again (by running `python3 main.py`). Then, restart the `reply.py` service (if you set it up.)

## Scheduling automatic posting
### Linux/macOS
- `EDITOR=nano crontab -e`
	- You can, of course, replace this with vim/emacs/whatever if you prefer those, or just omit the `EDITOR` bit entirely to use the default.
- To make sure this works, we're going to include the path to `python3`. To get this, type `which python3`, and copy the path to the clipboard.
- Add the following lines to the bottom of the file. Make sure you replace `/path/to/mstdn-ebooks/` with the path you downloaded mstdn-ebooks to, along with the full path to `python3` we got earlier (by running `which python3`).
```
*/30 * * * * cd /path/to/mstdn-ebooks/ && /path/to/python3 gen.py
5 */2 * * * cd /path/to/mstdn-ebooks/ && /path/to/python3 main.py

```
- Make sure you leave a blank line at the end of the file!
- Save the file (if you're using nano, press control-O, enter, and then control-X to exit)

Your bot should now automatically post once every half hour, and download new toots every two hours. The Magic of Computing!
### Windows
- Open notepad.exe
- Paste the following text into the new Notepad document, making sure to replace the placeholder and the path to your `python.exe`:
```
@echo off
cd C:\path\to\mstdn-ebooks
C:\Python37\python.exe main.py
C:\Python37\python.exe gen.py
```
- Save the file:
	- In the bottom-right, click the dropdown labeled "Text files (*.txt)" and change it to "All files". **This step is very important!**
	- Save the file as `run-bot.bat`
- Open the command prompt (Win-R, type "cmd", enter)
- `cd` to your mstdn-ebooks folder
- Enter the following command, replacing the placeholder:
	- `schtasks /create /tn "mstdn-ebooks autoposter" /tr "C:\path\to\mstdn-ebooks\run-bot.bat" /sc MINUTE /mo 30`

Your bot should now automatically download toots and post once every half hour. The Magic of Computing!

## Enabling replies
### Linux
#### systemd
- `sudo nano /etc/systemd/system/reply.service` (you don't have to call it `reply.service` but this guide will assume you did)
- Paste the following into the file:
```
[Unit]
Description=mstdn-ebooks bot replier
After=network.target
[Service]
Type=simple
User=your-username
WorkingDirectory=/path/to/mstdn-ebooks
ExecStart=/usr/bin/python3 /path/to/mstdn-ebooks/reply.py
TimeoutSec=3600
Restart=always
RestartSec=5s
[Install]
WantedBy=multi-user.target  
```
- Fill out the placeholder info (`User`, `WorkingDirectory`, `ExecStart`). `User` should be the user you log in to your computer as (e.g. `lynne`).
- Save the file (control-O, control-X)
- `sudo systemctl enable reply.service` (if you want it to automatically run at startup. if not, you can skip this step)
- `sudo systemctl start reply.service`
Now try mentioning your bot and it should hopefully reply! Magic!
#### Other init systems
no idea, sorry

### macOS/Windows
good luck my friend uwu

## End
Bot code and guide by [@lynnesbian@fedi.lynnesbian.space](https://fedi.lynnesbian.space/@lynnesbian). Feel free to contact me if you need assistance. Donations are appreciated to help with server costs! [Patreon](https://patreon.com/Lynnesbian), [Ko-fi](https://ko-fi.com/lynnesbian), [PayPal](https://paypal.me/lynnesbian)
# MHF IELess Launcher

MHF default launcher requires IE to login. IE sucks.

This project reverse engineered the MHF launcher in order to make it possible to boot the game directly, without going through `mhf.exe` and `mhl.dll`.

If you're wondering 'Why use this instead of the original launcher?', here are some of the issues that are are solved by using a custom launcher:

- Not being locked to IE.
  - Should open a sea of possibilities on how to design the launcher.
  - Won't take 10 seconds for each request.
  - Game might boot under Linux/Steam deck when using Proton/Wine, since IE was the main reason those weren't even options.
- Not being locked to the weird way MHF connects to the server.
  - Allows launcher operations to be implemented using with HTTP(S), JSON, custom ports, etc.
- Not being locked to the operations and data model the original launcher uses.
  - Allows implementing new operations, such as adding separate buttons for 'Sign Up' and 'Login'.
  - Allows storing and displaying extra information. For example, it would be *possible* to get character portraits on the launcher window.
  - Removes the need to modify the launcher (since we're replacing it) and `mhfo-hd.dll` to remove GameGuard, since `mhfo-hd.dll` calls a function provided by the launcher to run GameGuard checks.

Obs.: It should be noted that the Python GUI on the root is for testing the APIs. A decent GUI will be developed in the future, but some decisions need to be made first.


## Usage

If you want to test this, make sure your server is running under the [`newsign`](https://github.com/rockisch/Erupe-1/tree/newsign) branch.

After that, copy `gui.py` and `mhf-iel.exe` (either from the releases page or by compiling it yourself) to the folder MHF is installed. Run `gui.py`, and be happy.

### How to run no GUI on Erupe

1. Download `mhf-iel.exe` from [releases](https://github.com/rockisch/mhf-iel/releases) and move to MHFrontier folder.
2. Create an sign_session on your PosgreSQL `INSERT INTO sign_sessions ("user_id", "token") VALUES (<user_id>, <token>)`

Example: `INSERT INTO sign_sessions ("user_id", "token") VALUES (1, "tqVPK2TASRVTjEDQ")`

3. Run .exe on MHFrontier folder. `./mhf-iel.exe <char_id> <is_new> <token>`

Example: `./mhf-iel.exe 1 1 tqVPK2TASRVTjEDQ`




## Compile

In order to compile the project, a x86/32bit version of `cl` should be used, since we need to interface with `mhfo-hd.dll`, which was built for x86.

The easiest way to get `cl` is by installing Visual Studio's 'Desktop C++ Dev' package.
If your Visual Studio is from 2019 or before, it should target x86 by default.
Otherwise, you can run the 'x86 Native Tools CMD' shortcut, which will set everything for you.

After having a x86 version of `cl` available on your shell, just run `buildCMD.bat` or `buildDLL.bat`.

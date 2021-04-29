# Installing Unreal Engine 4 on openSUSE

This guide can be used as a reference by anyone who wants to play with the Unreal Engine on Linux. I came up with this guide by reading through several references on the internet (including the official documentation, as well as community posts from users that experienced the same issues like me).

## Tested With...

1. Operating System: `openSUSE Leap 15.2`
2. Unreal Engine Version: `4.26.2`

### Getting things working

#### 1. Prerequisites

Make sure you have plenty of disk space available on your hard drive. I installed my Unreal Engine version on a second partition on a 2 TB hard drive. The basic installation without any projects being added takes at least 96 GB. So make sure, you got way more space available than that. I assume you do not just want to install the Unreal Engine, but also add own or third party projects to it ;-)

#### 2. Fetch the Unreal Engine source code

The first step is to fetch the Unreal Engine source code. There's no ready built binary like on Windows. You have to create an account at EpicGames and connect that account with your GitHub account. This is pretty well documented [here](https://github.com/EpicGames/Signup).

As soon as you have access to the Unreal Engine's private repository on GitHub, you may pull the source code via SSH to your prefered location. I used second partition which had 2 TB of disk space available.

Clone the git repository:

```
git clone git@github.com:EpicGames/UnrealEngine.git
```

If that command fails or it says that repository does not exist, you may want to check if you can even access this link: https://github.com/EpicGames/UnrealEngine

If you cannot even access that link, your GitHub account might not be authorized to access the Unreal Engine source code. So go through Epic Games's guide (referenced above) again and make sure you gain access. If you're still facing issues with that, reach out to the Epic Games support.

#### 2. Choosing the right version

Assuming you cloned the source code and entered its git directory, you checkout one of the stable branches to build the engine from. Every Unreal Engine version has its own branch and every version was tagged. I wanted to use the latest version (as of 2021-04-28) so I chosed `4.26.2-release`.

Checkout that tag:
```
git checkout 4.26.2-release
```

Now you entered that branch and we can continue with building the sources.

If you want to use a different version, you might want to see what version fits your needs:

```
git tag --list
```

That shows you all available tagged versions.

#### 3. Building the sources

Building the engine from its source code was easier than expected and requires you to run 3 simple commands:

```
./Setup.sh
```

Make sure you see a success message at the end.

Now generate the project files of the engine:

```
./GenerateProjectFiles.sh
```

As soon as this has succeeded as well, you can build the sources now. Unreal Engine ships their own bundled compiler and its dependencies, so you actually do not need to install any further packages.

Now run `make` to compile the source code. They recommended using just `make`, but I decided to use `make -j6` to speed up the compilation process. The `-j` option allows make to do several make jobs in parallel, which sped the compilation process significantly. The `6` after the `-j` says that `make` shall execute 6 jobs in parallel (I don't even think all of them were in use when I did my tests).
```
make -j6
```

(If make is not available, install it via `zypper install make`)

This can take some hours or minutes, depending on your CPU.

#### 4. Running the Editor

The engine should have been built by now. You can try running editor, but it likely will fail. Anyway, let's try it. Go to your engine git checkout directory and call the UE4Editor binary which is located at `Engine/Binaries/Linux/UE4Editor` relative to your unreal engine git checkout directory.

However, it will probably fail at launch saying that it misses Vulkan support. Run the following command to install the missing dependencies:

For an intel GPU:

```
zypper install libvulkan_intel libvulkan_intel-32bit
```

or if you have an AMD GPU, run:

```
zypper install libvulkan_radeon libvulkan_radeon-32bit
```

Try to open the UE4Editor binary again. In my case it showed me that a driver is missing to run Vulkan. This happened because I didn't install any GPU driver on my Linux system so far. As I'm having an NVIDIA GPU, I opened the official openSUSE documentation on how to install an NVIDIA graphics card driver: https://en.opensuse.org/SDB:NVIDIA_drivers

I installed the graphics card driver that matched my GPU and after a reboot (including enabling the kernel driver module as described in the documentation), the UE4Editor could be opened just fine.

#### 5. Creating your first project

##### 5.1 Blueprint Projects

You are able to create a blueprint project via the UE4Editor. It should just work fine and out of the box.

##### 5.2 C++ Projects

Creating C++ projects does not work out of the box. You will notice that after creation of the project, the UE4Editor closes and a text editor opens. The reason for that is that the compilation process needs to be triggered manually after the C++ project creation. Please navigate to your Unreal Engine project location (which you just have created). A `Makefile` should be there. My project directory looks like this:

```
manuel@localhost:~/Documents/Unreal Projects/MyProject3> l
total 4984
drwxr-xr-x 11 manuel users    4096 Apr 29 17:02 ./
drwxr-xr-x  5 manuel users    4096 Apr 28 20:31 ../
drwxr-xr-x  3 manuel users    4096 Apr 28 20:34 Binaries/
-rw-r--r--  1 manuel users   87489 Apr 28 20:33 CMakeLists.txt
drwxr-xr-x  2 manuel users    4096 Apr 28 20:31 Config/
drwxr-xr-x  9 manuel users    4096 Apr 29 17:04 Content/
drwxr-xr-x  3 manuel users    4096 Apr 29 17:04 DerivedDataCache/
-rw-r--r--  1 manuel users      29 Apr 28 20:32 .ignore
drwxr-xr-x  8 manuel users    4096 Apr 29 17:05 Intermediate/
drwxr-xr-x  2 manuel users    4096 Apr 28 20:32 .kdev4/
-rw-r--r--  1 manuel users   38842 Apr 28 20:31 Makefile
-rw-r--r--  1 manuel users  419137 Apr 28 20:33 MyProject3CodeCompletionFolders.txt
-rw-r--r--  1 manuel users       0 Apr 28 20:33 MyProject3CodeLitePreProcessor.txt
-rw-r--r--  1 manuel users     354 Apr 28 20:32 MyProject3.code-workspace
-rw-r--r--  1 manuel users  150697 Apr 28 20:33 MyProject3Config.pri
-rw-r--r--  1 manuel users      13 Apr 28 20:33 MyProject3Defines.pri
-rw-r--r--  1 manuel users 1906847 Apr 28 20:33 MyProject3Header.pri
-rw-r--r--  1 manuel users  370864 Apr 28 20:33 MyProject3Includes.pri
-rw-r--r--  1 manuel users      59 Apr 28 20:32 MyProject3.kdev4
-rw-r--r--  1 manuel users   40444 Apr 28 20:33 MyProject3.pro
-rw-r--r--  1 manuel users 1468726 Apr 28 20:33 MyProject3Source.pri
-rw-r--r--  1 manuel users     224 Apr 28 20:31 MyProject3.uproject
-rw-r--r--  1 manuel users  539289 Apr 28 20:33 MyProject3.workspace
drwxr-xr-x  6 manuel users    4096 Apr 29 17:05 Saved/
drwxr-xr-x  3 manuel users    4096 Apr 28 20:31 Source/
drwxr-xr-x  4 manuel users    4096 Apr 28 20:32 .vscode/
````

In order to open the project inside of the Unreal Editor, I built the 'MyProject3Editor' target of the Makefile. Run the following command inside your UE4 project directory:

```
make MyProject3Editor
```

You need to replace `MyProject3` editor with the name of your project and append `Editor` to it. If your project is named `BlaBlaBla`, then the command would be `make BlaBlaBlaEditor`.

As soon as this has built, you can open the `UE4Editor` binary again and open the project. It may not be visible in your recent projects, but by clicking `More` you can search for projects on your hard drive.

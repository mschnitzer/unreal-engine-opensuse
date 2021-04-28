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

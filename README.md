# think-cell guide on how to submit boost patches

## Clone and setup the boost repository 

1. On the command line, do

        git clone --recursive https://github.com/think-cell/boost.git boost_thinkcell

    In case this fails with an error "Fetched in submodule path ...", type 

        cd boost_thinkcell
	    git submodule update --recursive --remote

    TODO: Checkout develop branch

2. Once the clone is complete, go to `boost_thinkcell/tools/build/` and execute `booststrap[.bat|.sh]`. This will build `b2`, the Boost.Build executable.

    For convenience, add the build folder to your path variable. On Windows, right-click the start menu button, choose Control Panel and then search for "environment variable" and choose "Edit environment variables for your account." On the Mac, enter 

        echo export PATH=\$PATH:~/path_to/boost_thinkcell/tools/build >> ~/.bashrc
        source ~/.bashrc    

3. On the Mac, please setup the boost documentation toolchain with the following commands: 

        brew install doxygen libxslt
        cd tools/boostbook
        ./setup_boostbook.sh 

You can now compile boost, run its tests, and build its documentation. On the Mac, run `b2` in the root folder to build all of boost or inside a `doc` / `test` subfolder (e.g. `libs/interprocess/test`) to build the documentation or run the tests. On Windows, run `b2 link=static runtime-link=static` instead.

## How to make and contribute a patch

1. Find out if we have forked the library already that you want to patch? 

	Open `boost_thinkcell/.git/config` and look for the library. If its URL points to https://github.com/think-cell/library then you can proceed with the next step. Otherwise, we have to make a thinkcell fork of the library itself first. You need a github.com account for this and you have to be a member of the think-cell organization with fork privileges. 

	Go to https://github.com/boostorg/library and click the fork button. Github asks you, if you want to fork with your personal github account or as think-cell, choose as think-cell. 

	Edit `boost_thinkcell/.git/config` and change the remote URL to https://github.com/think-cell/library, then run 

    git submodule sync

    Commit the changed `.gitmodules` file.

2. Go to `libs/library_you_want_to_patch`

    `git remote -v` should return the github.com/think-cell/library remote. 

    (TODO: Make sure the develop branch is synced with github.com/boostorg/library
        git submodule foreach git fetch
        git submodule update
    )

    Create a branch of the `develop` branch for your bugfix, prefix the branch name with "thinkcell_". 

        git checkout develop
        git checkout -B thinkcell_mybranch

    Make your changes, commit them and push them to github.com. Consider adding tests/updating the documentation when your change is non-trivial. This may increase the chance of the fix getting merged. Otherwise, we have to maintain the patch indefinitely. 
    




    


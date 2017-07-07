# think-cell guide on how to submit boost patches (Work in progress)

## Clone and setup the boost repository 

1. Consider cloning boost on a Mac, it takes a long time on Windows. On the command line, do

        git clone --recursive -b develop https://github.com/think-cell/boost.git boost_thinkcell

2. Once the clone is complete, go to `boost_thinkcell/tools/build/` and execute `booststrap[.bat|.sh]`. This will build `b2`, the Boost.Build executable.

    For convenience, add the build folder to your path variable. On Windows, right-click the start menu button, choose Control Panel and then search for "environment variable" and choose "Edit environment variables for your account." On the Mac, enter 

        echo export PATH=\$PATH:/path_to/boost_thinkcell/tools/build >> ~/.bashrc
        source ~/.bashrc    

3. On the Mac, please setup the boost documentation toolchain with the following commands: 

        brew install doxygen libxslt
        cd tools/boostbook
        ./setup_boostbook.sh 

You can now compile boost, run its tests, and build its documentation. On the Mac, run `b2` in the root folder to build all of boost or inside a `doc` / `test` subfolder (e.g. `libs/interprocess/test`) to build the documentation or run the tests. On Windows, run `b2 link=static runtime-link=static` instead.

## How to make and contribute a patch

1. Find out if we have forked the library already that you want to patch? 

	Open `boost_thinkcell/.gitmodules` and look for the library. If its URL points to https://github.com/think-cell/library then you can proceed with the next step. Otherwise, we have to make a thinkcell fork of the library itself first. You need a github.com account for this and you have to be a member of the think-cell organization with fork privileges. 

	Go to https://github.com/boostorg/library and click the fork button. Github asks you, if you want to fork with your personal github account or as think-cell, choose as think-cell. 

	Edit `boost_thinkcell/.gitmodules` and change the remote URL to https://github.com/think-cell/library, then run 

    	git submodule sync

    Commit the changed `.gitmodules` file.

2. Make your changes
	
	Go to `libs/library_you_want_to_patch`. 
	
	`git remote -v` should return the github.com/think-cell/library remote. 

    Create a branch of the `develop` branch for your bugfix, prefix the branch name with "thinkcell_". 

        git checkout develop
        git checkout -B thinkcell_mybranch

    Make your changes, commit them and push them to github.com. Consider adding tests/updating the documentation when your change is non-trivial. This may increase the chance of the fix getting merged. Otherwise, we have to maintain the patch indefinitely. 
    
3. Open Pull Request

	

## Update develop branch from git

Update all think-cell forked submodules

	cd libs/container; git checkout develop; git pull https://github.com/boostorg/container; git push;
	cd libs/range; git checkout develop; git pull https://github.com/boostorg/range; git push;
	cd libs/interprocess; git checkout develop; git pull https://github.com/boostorg/interprocess; git push;

	git pull https://github.com/boostorg/boost develop
	git push
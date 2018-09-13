******************
Upgrade your users
******************

Test it live: https://github.com/nxxm/example-upgrd-app

Installing upgrd
================

nxxm keeps your deployments up-to-date for you, had it to `.nxxm/deps`:: 

  { 
      "nxxm/upgrd" : { "@" : "v0.0.2" } 
  } 

Add to your app main function the following::

  #include <upgrd/upgrd.hxx>

  int main(int argc, char** argv) {

    // Download Releases out of GitHub Release Page Assets
    upgrd::manager up{
      "github-account",
      "your-github-repo",
      "v0.0.1",
      argv[0],
      std::cout
    };
    up.propose_upgrade_when_needed(); 

    return 0;
  }

Relies on GitHub Releases to distribute always the newest version to your users. 

Publishing Releases
===================

Look at an example: https://github.com/nxxm/example-upgrd-app/releases

- Add a zip file with your binary to a GitHub Release. Simply add in the zip name respective to each platform :
  * `windows`
  * `linux`
  * `macOS`

- Add a SHA1 sum in the body of the release for each archive :
  `archive-name.zip:SHA1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX`

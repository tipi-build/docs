---
title: 🚀 Getting started C++
aliases: []
---

Setup your machine:

1. Create your account on [tipi.build](https://tipi.build/)
2. Install `tipi` :
    - Tipi Visual Studio Code extension: &nbsp; [<img src="~/assets/vscode.png" style="height: 1em; vertical-align: middle;">&nbsp; Add to Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=tipi.tipi-build)
    - Command line utility:

```bash
# Linux & MacOS:
/bin/bash -c \
 "$(curl -fsSL https://raw.githubusercontent.com/tipi-build/cli/master/install/install_for_macos_linux.sh)"
```

```powershell
# Windows 10 / 11 in Powershell
[Net.ServicePointManager]::SecurityProtocol = "Tls, Tls11, Tls12, Ssl3"
. { `
  iwr -useb https://raw.githubusercontent.com/tipi-build/cli/master/install/install_for_windows.ps1 `
} | iex

# P.S.: we highly recommend you give a the new Windows Terminal app a try. It truly augments your 
# console experience on Windows!
```

3. Run `tipi connect` and link your installation to your [tipi.build account](https://tipi.build/dashboard) so you can use your tipi subscription
4. Create an empty folder for the example project on your disk and `cd` into it
5. Create an `example.cpp` inside that folder and write a simple hello world:

```cpp
#include <iostream>

int main(int argc, char** argv) {
  std::cout << "tipi is cool!" << std::endl;
  return 0;
}
```

> ##### Note on platforms and environments:
>
> You can replace occurrences of `linux-cxx17` in the instructions below with `windows` or `macos-cxx17` when using your subscription
> or the environment matching your machine's platform
>
> On Linux: `linux-cxx17` or `linux-cxx20`
>
> On MacOS: `macos-cxx17` or `macos-cxx20`
>
> On Windows: `windows` or `windows-cxx17` or `windows-cxx20` or `vs-16-2019-cxx17` (if you have Build Tools for Visual Studio 2019 installed)
>
>
> When using your tipi subscription to build or run, a cloud node of the corresponding platform is deployed in the tipi cloud.

6. build the example using either:
    - your [tipi subscription](https://tipi.build/dashboard/subscription): `tipi -t linux-cxx20 build . `
    - your local machine[^1]: `tipi -t linux-cxx20 . `

7. run the resulting binary using:
    - your [tipi subscription](https://tipi.build/dashboard/subscription): `tipi -t linux-cxx20 .run build/linux-cxx20/bin/example`
    - your local machine[^1]: `tipi run build/linux-cxx20/bin/example`

8. Add a dependency from GitHub: we're going to add a JSON manipulation library from: [github.com/nlohmann/json](https://github.com/nlohmann/json)
    - create a file `.tipi/deps` with content

```json
{
	"nlohmann/json" : {
		"@" : "v3.11.2",
		"u": false,
		"x": [
			"benchmarks",
			"/tests",
			"/docs",
			"/tools"
		]
	}
}

```
  
> Note: we are pinning the version of the dependency to the tagger release `v3.11.2` (list can be found under 
> [nlohmann/json/releases](https://github.com/nlohmann/json/releases) ). At time of building tipi will
> pull the release from the GitHub repository and build it. If you want to live on the edge, you can remove
> the `@` pin or write a branch name like `master` in there.

9. Edit your `example.cpp`:

```cpp
#include <iostream>
#include <nlohmann/json.hpp> // tipi.build will find it online

int main(int argc, char** argv) {

  std::cout << "Wonderful JSON formatter with tipi is cool!" << std::endl;

  auto json = nlohmann::json::parse(argv[1]);
  std::cout << json.dump(2) << std::endl;
  return 0;
}
```

10. Compile and run (see #6 and #7 above):

```bash
$> tipi -t linux-cxx20 build .
$> tipi -t linux-cxx20 .run ./build/linux-cxx20/bin/example '[4,52,25]'
Wonderful JSON formatter with tipi is cool!
[
  4,
  52,
  25
]
```



*******************************
Passing -D defines
*******************************
Pass them simply via ``tipi . -DSOME_OPTION=1 -DOTHER_OPTION=OK``.

.. hint:: The defines will transitively be passed to all your dependencies build as well.


***************
Compile options
***************
We rely on the CMake project. tipi abstracts it away, but you might need to tweak the compilation flags.

We encourage you to put all your compile options within your target toolchain files. We deliver many target toolchain files pre-configured. You can also add your own in ``.nxxxm/<distro>/polly/``.

Nevertheless if you add compile options, they will be used for all projects in the build tree as we add them in the context of the current project to the sysroot toolchain file.

.tipi/opts[.target-platform]
============================

If the file is named ``.tipi/opts`` it is always used, to that if their are ``.tipi.target-platform`` file the options are used only in the case the platfom is selected via ``tipi . -t target-platform`` it is cumulative to the the main opts.

The opts file have to contain valid CMake Syntax. For example to pass #defines or compile options this way simply add:: 

  add_compile_options ( -fmath-errno -Wextra )
  add_compile_definitions( DEFINE_TO_PASS_WITHOUT_D_BEFORE=1 )
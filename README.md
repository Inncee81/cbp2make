# cbp2make

This is imported/copied from https://sourceforge.net/p/cbp2make 

Licensed by the original author as GNU General Public License version 3.0 (GPLv3)

About
'''cbp2make''' is a stand-alone build tool that allows you to generate makefile(s) for GNU Make out of Code::Blocks IDE project or workspace file. (See also [http://forums.codeblocks.org/index.php/topic,13675.0.html])
Usage
Create makefile for a single project or workspace
Let's assume you have a project "my_project.cbp" and you need a makefile for this project. In this simplest case all you have to do is:
cbp2make -in my_project.cbp
Same thing applies to workspaces.
cbp2make -in my_projects.workspace
Create makefile with another filename
By default, '''cbp2make''' will append ".mak" extension to the project name to compose a filename for makefile.
If you want to change that, use following command:
cbp2make -in my_project.cbp -out Makefile
Create makefile for another platform
If you are working in GNU/Linux and you want to generate a makefile for Windows or the other way around, you can specify one or more platforms for which you need makefiles.

cbp2make -in my_project.cbp -windows

cbp2make -in my_project.cbp -unix

cbp2make -in my_project.cbp -unix -windows -mac

cbp2make -in my_project.cbp --all-os
'''cbp2make''' will append ".unix" or ".windows" or ".mac" suffix to makefile name for each platform respectively.
Create makefile for multiple projects or workspaces
If you have more than one independent project or workspace, you can process them at once, but you will need a text file containing the list of projects, e.g., ''projects.lst'', with one project filename per line.
my_project.cbp
my_other_project.cbp
And then you can process them using command:
cbp2make -list -in projects.lst
Configuration
Some installation-specific or project-specific options, primarily toolchain settings, can be saved to a configuration file. By default (since rev.110), cbp2make does not save any settings to a configuration file unless the user explicitly specifies the "--config" option. A configuration file can be either global (stored in user profile / home directory) or local (stored in current directory).
Please, keep in mind that since cbp2make is in early stage of development, an old configuration file may become incompatible with new tool version and it may be necessary to update it manually or initialize a new one.
Initialization
cbp2make --config options --global

cbp2make --config options --local
Later use
When cbp2make is invoked, first it tries to load a local configuration file. If a local configuration is missing, next attempt will be to load a global one. If this attempt is not successful either, the default built-in configuration is used. Configuration lookup order can be overridden with "--local" or "--global" command line options. If one of options is supplied to cbp2make, non-specified configuration is not picked up even if the specified one is missing and non-specified do exists.
Default lookup order
cbp2make -in project.cbp -out Makefile
Explicitly specified configuration
cbp2make --local -in project.cbp -out Makefile

cbp2make --global -in project.cbp -out Makefile
Command line syntax

Generate makefile:
cbp2make -in <project_file> [-cfg <configuration>] [-out <makefile>]
[-unix] [-windows] [-mac] [--all-os] [-targets "<target1>[,<target2>[, ...]]"]
[--flat-objects] [--flat-objpath] [--wrap-objects] [--wrap-options]
[--with-deps] [--keep-objdir] [--keep-outdir] [--target-case keep|lower|upper]
cbp2make -list -in <project_file_list> [-cfg <configuration>]
[-unix] [-windows] [-mac] [--all-os] [-targets "<target1>[,<target2>[, ...]]"]
[--flat-objects] [--flat-objpath] [--wrap-objects] [--wrap-options]
[--with-deps] [--keep-objdir] [--keep-outdir] [--target-case keep|lower|upper]
     Manage toolchains:
            cbp2make --config toolchain --add \[-unix|-windows|-mac\] -chain <toolchain>
            cbp2make --config toolchain --remove \[-unix|-windows|-mac\] -chain <toolchain>

    Manage build tools:
            cbp2make --config tool --add \[-unix|-windows|-mac\] -chain <toolchain>
                     -tool <tool> -type <type> <tool options>
            cbp2make --config tool --remove \[-unix|-windows|-mac\] -chain <toolchain>
                     -tool <tool>

    Tool types:      pp=preprocessor as=assembler cc=compiler rc=resource compiler
                     sl=static linker dl=dynamic linker el=executable linker
                     nl=native linker
    Tool options (common):
                     -desc <description> -program <executable> -command <command_template>
                     -mkv <make_variable> -srcext <source_extensions> -outext <output_extension>
                     -quotepath <yes|no> -fullpath <yes|no> -unixpath <yes|no>
    Tool options (compiler):
                     -incsw <include_switch> -defsw <define_switch> -deps <yes|no>
    Tool options (linker):
                     -ldsw <library_dir_switch> -llsw <link_library_switch> -lpfx <library_prefix>
                     -lext <library_extension> -objext <object_extension> -lflat <yes|no>

    Manage platforms:
            cbp2make --config platform \[-unix|-windows|-mac\] \[-pwd <print_dir_command>\]
                     \[-cd <change_dir_command>\] \[-rm <remove_file_command>\]
                     \[-rmf <remove_file_forced>\] \[-rmd <remove_dir_command>\]
                     \[-cp <copy_file_command>\] \[-mv <move_file_command>\]
                     \[-md <make_dir_command>\] \[-mdf <make_dir_forced>\]
                     \[-make <default_make_tool>\]

    Manage global compiler variables:
            cbp2make --config variable --add \[-set <set_name>\] -name <var_name>
                     \[-desc <description>\] \[-field <field_name>\] -value <var_value>
            cbp2make --config variable --remove \[-set <set_name>\] \[-name <var_name>\]
                     \[-field <field_name>\]

    Manage options:
            cbp2make --config options --default-options "<options>"
            cbp2make --config show

    Common options:
            cbp2make --local        // use configuration from current directory
            cbp2make --global       // use configuration from home directory
            cbp2make --verbose      // show project information
            cbp2make --quiet        // hide all messages
            cbp2make --help         // display this message
            cbp2make --version      // display version information
Options
''Makefile generation''
-in <project_file> // specifies an input file or a list of files;

-cfg <configuration> // specifies a configuration file, see also "--local" and "--global" options;

-out <makefile> // specifies the name of a makefile or a list of makefiles;

-unix // enables Unix / Linux compatible makefile generation;

-windows // enables Windows compatible makefile generation;

-mac // enables Macintosh compatible makefile generation;

--all-os // enables all target platforms at once;

-targets "<target1>[,<target2>[, ...]]" // specifies the only build targets that a makefile will be made for;

--flat-objects // forces "flat" names for object files with limited character set;

--flat-objpath // forces "flat" paths for object files with no subdirectories;

--wrap-objects // allows to use multiline lists of objects which makes a makefile easier to read;

--wrap-options // allows to use multiline macros;

--with-deps // allows a built-in dependency scanner for C/C++ projects;

--keep-objdir // disables command that erase directories for object files in 'clean' target;

--keep-outdir // disables command that erase directory for an output binary file in 'clean' target;

--target-case [keep|lower|upper] // specifies style for makefile targets;
Examples

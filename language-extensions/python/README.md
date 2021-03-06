# Python Language Extension 

## Getting Started
Language Extensions is a feature of SQL Server used for executing external code. The relational data can be used in the external code using the extensibility framework.

For more information about SQL Server Language Extensions, refer to this [documentation](https://docs.microsoft.com/en-us/sql/language-extensions/language-extensions-overview?view=sql-server-ver15).

The Python extension version in this repository is compatible with SQL Server 2019 CU3 onwards. 

Note that the Python Extension released in the current repository works with Python 3.7.x - to use the released package, follow (this tutorial)[https://docs.microsoft.com/en-us/sql/machine-learning/install/custom-runtime-py?view=sql-server-ver15]. For any other version of Python (3.6, 3.8, etc) you must modify and rebuild the Python Extension binaries using the following instructions.

## Building

### Windows
1. Install [CMake for Windows](https://cmake.org/download/) and [Python](https://www.python.org/downloads/release/python-379/).

1. Install the package numpy into your python. This is needed to build boost_numpy in the next step.

1. Download and build [Boost Python](https://www.boost.org/doc/libs/1_74_0/libs/python/doc/html/building/no_install_quickstart.html) with your version of Python. Make sure to build the static libraries.

1. Install C++ Tools for CMake from the [Build Tools for Visual Studio 2017](https://my.visualstudio.com/Downloads?q=visual%20studio%202017&wt.mc_id=o~msft~vscom~older-downloads). 
		Download the Visual Studio 2017 Build Tools installer and check the **Visual C++ build tools** option under Workloads. In the sidebar on the right, make sure **Visual C++ tools for CMake** is checked, then install.
		
1. Set CMAKE_ROOT, PYTHONHOME, and BOOST_ROOT pointing to their respective installation and build folders.

1. Modify [**language-extensions\python\src\CMakeLists.txt**](.\src\CMakeLists.txt). Change each `find_library` call to point to your custom python and boost libraries. 

1. Run [**build-python-extension.cmd**](.\build\windows\build-python-extension.cmd) which will generate: \
		- PATH\TO\ENLISTMENT\build-output\pythonextension\windows\release\release\pythonextension.dll 
		
1. Run [**create-python-extension-zip.cmd**](.\build\windows\create-python-extension-zip.cmd) which will generate: \
		- PATH\TO\ENLISTMENT\build-output\pythonextension\windows\release\release\packages\python-lang-extension.zip \
		This zip can be used in CREATE EXTERNAL LANGUAGE, as detailed in the tutorial in the Usage section below.

### Linux
1. Install [CMake for Linux](https://cmake.org/download/) and [Python](https://www.python.org/downloads/source/)

1. Install the package numpy into your python. This is needed to build boost_numpy in the next step.

1. Download and build [Boost Python](https://www.boost.org/doc/libs/1_74_0/libs/python/doc/html/building/no_install_quickstart.html) with your version of Python. Make sure to build the static libraries.

1. Set PYTHONHOME and BOOST_ROOT to point to your python installation and boost build folder respectively.

1. Modify [**language-extensions\python\src\CMakeLists.txt**](.\src\CMakeLists.txt). Change each `find_library` call to point to your custom python and boost libraries. 

1. Run [**build-python-extension.sh**](.\build\linux\build-python-extension.sh) which will generate: \
		- PATH/TO/ENLISTMENT/build-output/pythonextension/linux/release/libPythonExtension.so.1.0 

1. Run [**create-python-extension-zip.sh**](.\build\linux\create-python-extension-zip.sh) which will generate: \
		- PATH/TO/ENLISTMENT/build-output/pythonextension/linux/release/packages/python-lang-extension.zip \
		This zip can be used in CREATE EXTERNAL LANGUAGE, as detailed in the tutorial in the Usage section below.

## Usage
After downloading or building the Python extension zip, use CREATE EXTERNAL LANGUAGE to create the language on the SQL Server. 

This [tutorial](https://docs.microsoft.com/en-us/sql/machine-learning/install/custom-runtime-py?view=sql-server-ver15) will walk you through an end to end sample using the Python language extension. 

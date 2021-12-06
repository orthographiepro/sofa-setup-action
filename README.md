# sofa-setup-action

## Usage example

In a matrix workflow with a `sofa_branch` axis like `sofa_branch: [master, v21.06]`
```yml
# Setup SOFA, dependencies, environment variables, workspace tree, ...
- name: Setup SOFA and environment
  id: sofa
  uses: sofa-framework/sofa-setup-action@v3.0
  with:
    sofa_root: ${{ github.workspace }}/sofa
    sofa_version: ${{ matrix.sofa_branch }}
    sofa_scope: 'standard'

# Checkout my plugin sources into workspace_src_path (prepared by sofa-setup-action)
- name: Checkout source code
  uses: actions/checkout@v2
  with:
    path: ${{ steps.sofa.outputs.workspace_src_path }}

# Build, install, test, deploy my plugin
# ...
```

A more detailed example is available in this repository: `examples/workflow.yml`

## Parameters

### Inputs

```yml
  sofa_root:
    description: 'SOFA install directory'
    required: true
    default: '/opt/sofa'
  sofa_version:
    description: 'Major version of SOFA to install'
    required: true
    default: 'v21.06'
  sofa_scope:
    description: 'Choose between: minimal, standard, full'
    required: false
    default: 'minimal'
  python_version:
    description: 'Python version to install'
    required: true
    default: '3.9'
  workspace_auto_setup:
    description: 'Should this action setup your workspace tree?'
    required: false
    default: 'true'
```

### Outputs as objects

```yml
  sofa_version:
    description: "SOFA version"
  sofa_root:
    description: "SOFA root directory"
  exe:
    description: "Extension for executable files"
  run_branch:
    description: "Pretty git branch of this workflow run"
  python_root:
    description: "Root directory of the Python installed for SOFA"
  python_version:
    description: "Version of the Python installed for SOFA"
  vs_install_dir:
    description: "VS install directory"
  vs_vsdevcmd:
    description: "Command to init VS environment"
  workspace_src_path:
    description: "Directory to checkout your sources after calling this action"
  workspace_build_path:
    description: "Directory to build your sources after calling this action"
  workspace_install_path:
    description: "Directory to install your binaries after calling this action"
  workspace_artifact_path:
    description: "Directory to package your binaries after calling this action"
```

### Outputs as environment variables

```txt
  EXE: see outputs as objects
  RUN_BRANCH: see outputs as objects
  
  SUDO: set to 'sudo' on Unix systems, empty on others
  EIGEN_VERSION: Fixed for now, set to '3.3.7'. Used only on Windows.
  EIGEN_INSTALL_DIR: [Internal] Directory where Eigen was installed. Used only on Windows.
  BOOST_VERSION: Fixed for now, set to '1.69.0'. Used only on Windows.
  BOOST_INSTALL_DIR: [Internal] Directory where Boost was installed. Used only on Windows.
  VS_INSTALL_DIR: see outputs as objects
  VS_VSDEVCMD: see outputs as objects
  
  WORKSPACE_SRC_PATH: see outputs as objects
  WORKSPACE_BUILD_PATH: see outputs as objects
  WORKSPACE_INSTALL_PATH: see outputs as objects
  WORKSPACE_ARTIFACT_PATH: see outputs as objects
  
  CCACHE_COMPRESS: [Unix] Default value for ccache. Set to 'true'
  CCACHE_COMPRESSLEVEL: [Unix] Default value for ccache. Set to '6'
  CCACHE_MAXSIZE: [Unix] Default value for ccache. Set to '1G'
  CCACHE_BASEDIR: [Unix] Default value for ccache. Set to WORKSPACE_BUILD_PATH
  CCACHE_DIR: [Unix] Default value for ccache. Set to GITHUB_WORKSPACE/.ccache
  
  PYTHONUSERBASE: set to 'C:\pythonuserbase' on Windows, set to '/tmp/pythonuserbase' on others
  PYTHONIOENCODING: always set to 'UTF-8' 
  PYTHON_ROOT: see outputs as objects
  Python_ROOT: see outputs as objects
  
  SOFA_VERSION_FOR_DEPS: [Internal] Variable used to install different deps upon SOFA version
  
  BOOST_ROOT: [Windows] Directory where Boost was installed, needed for CMake detection
  Boost_ROOT: [Windows] Directory where Boost was installed, needed for CMake detection
  
  EIGEN3_ROOT: [Windows] Directory where Eigen was installed, needed for CMake detection
  Eigen3_ROOT: [Windows] Directory where Eigen was installed, needed for CMake detection
  
  SOFA_ROOT: Directory where SOFA was installed
```


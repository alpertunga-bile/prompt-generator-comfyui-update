# ComfyUI Manager


## NOTICE
* Versions prior to V0.22.2 will no longer detect missing nodes unless using a local database. Please update ComfyUI-Manager to the latest version.

## Installation

### Installation[method1] (General installation method: ComfyUI-Manager only)

To install ComfyUI-Manager in addition to an existing installation of ComfyUI, you can follow the following steps:

1. cd custom_nodes
2. git clone https://github.com/ltdrdata/ComfyUI-Manager.git
3. Restart ComfyUI


### Installation[method2] (Installation for portable ComfyUI version: ComfyUI-Manager only)
1. install git 
- https://git-scm.com/download/win
- standalone version  
- select option: use windows default console window
2. Download [scripts/install-manager-for-portable-version.bat](https://github.com/ltdrdata/ComfyUI-Manager/raw/main/scripts/install-manager-for-portable-version.bat) into installed `"ComfyUI_windows_portable"` directory
3. double click `install-manager-for-portable-version.bat` batch file

![portable-install](misc/portable-install.png)


### Installation[method3] (Installation for linux+venv: ComfyUI + ComfyUI-Manager)

To install ComfyUI with ComfyUI-Manager on Linux using a venv environment, you can follow these steps:

1. Download [scripts/install-comfyui-venv-linux.sh](https://github.com/ltdrdata/ComfyUI-Manager/raw/main/scripts/install-comfyui-venv-linux.sh) into empty install directory
- ComfyUI will be installed in the subdirectory of the specified directory, and the directory will contain the generated executable script.
2. `chmod +x install-comfyui-venv-linux.sh`
3. `./install-comfyui-venv-linux.sh`


You can execute ComfyUI by running either `./run_gpu.sh` or `./run_cpu.sh` depending on your system configuration.

## Colab Notebook
This repository provides Colab notebooks that allow you to install and use ComfyUI, including ComfyUI-Manager. To use ComfyUI, [click on this link](https://colab.research.google.com/github/ltdrdata/ComfyUI-Manager/blob/main/notebooks/comfyui_colab_with_manager.ipynb).
* Support for installing ComfyUI
* Support for basic installation of ComfyUI-Manager
* Support for automatically installing dependencies of custom nodes upon restarting Colab notebooks.

## Changes
* **0.29** Add `Update all` feature
* **0.25** support db channel
  * You can directly modify the db channel settings in the `config.ini` file.
  * If you want to maintain a new DB channel, please modify the `channels.list` and submit a PR.
* **0.23** support multiple selection
* **0.18.1** `skip update check` feature added.
  * A feature that allows quickly opening windows in environments where update checks take a long time.
* **0.17.1** Bug fix for the issue where enable/disable of the web extension was not working. Compatibility patch for StableSwarmUI.
  * Requires latest version of ComfyUI (Revision: 1240)
* **0.17** Support preview method setting feature.
* **0.14** Support robust update.
* **0.13** Support additional 'pip' section for install spec.
* **0.12** Better installation support for Windows.
* **0.9** Support keyword search in installer menu.
* **V0.7.1** Bug fix for the issue where updates were not being applied on Windows.
  * **For those who have been using versions 0.6, please perform a manual git pull in the custom_nodes/ComfyUI-Manager directory.**
* **V0.7** To address the issue of a slow list refresh, separate the fetch update and update check processes.
* **V0.6** Support extension installation for missing nodes.
* **V0.5** Removed external git program dependencies.


## How To Use

1. Click "Manager" button on main menu

![mainmenu](misc/main.png)


2. If you click on 'Install Custom Nodes' or 'Install Models', an installer dialog will open.

![menu](misc/menu.jpg)

* When the 'Use local DB' feature is enabled, the application will utilize the data stored locally on your device, rather than retrieving node/model information over the internet

* The ```Fetch Updates``` menu retrieves update data for custom nodes locally. Actual updates are applied by clicking the ```Update``` button in the ```Install Custom Nodes``` menu.

3. Click 'Install' or 'Try Install' button.

![node-install-dialog](misc/custom-nodes.png)

![model-install-dialog](misc/models.png)

* Installed: This item is already installed.
* Install: Clicking this button will install the item.
* Try Install: This is a custom node of which installation information cannot be confirmed. Click the button to try installing it.

4. If you set the `Badge:` item in the menu as `Badge: Nickname` or `Badge: #ID Nickname`, the information badge will be displayed on the node.
* `Badge: Nickname` displays the nickname of custom nodes, while `Badge: ID Nickname` also includes the internal ID of the node.

![model-install-dialog](misc/nickname.jpg)

## How to register your custom node into ComfyUI-Manager

* Add an entry to `custom-node-list.json` located in the root of ComfyUI-Manager and submit a Pull Request.
* NOTE: Before submitting the PR after making changes, please check `Use local DB` and ensure that the extension list loads without any issues in the `Install custom nodes` dialog. Occasionally, missing or extra commas can lead to JSON syntax errors.
* The remaining JSON will be updated through scripts in the future, so you don't need to worry about it.

## Custom node support guide

* Currently, the system operates by cloning the git repository and sequentially installing the dependencies listed in requirements.txt using pip, followed by invoking the install.py script. In the future, we plan to discuss and determine the specifications for supporting custom nodes.

* Please submit a pull request to update either the custom-node-list.json or model-list.json file.

* The scanner currently provides a detection function for missing nodes, which is capable of detecting nodes described by the following two patterns.
  * Or you can provide manually `node_list.json` file. 

```
NODE_CLASS_MAPPINGS = {
    "ExecutionSwitch": ExecutionSwitch,
    "ExecutionBlocker": ExecutionBlocker,
    ...
}

NODE_CLASS_MAPPINGS.update({
    "UniFormer-SemSegPreprocessor": Uniformer_SemSegPreprocessor,
    "SemSegPreprocessor": Uniformer_SemSegPreprocessor,
})
```

* When you write a docstring in the header of the .py file for the Node as follows, it will be used for managing the database in the Manager.
  * Currently, only the `nickname` is being used, but other parts will also be utilized in the future.
  * The `nickname` will be the name displayed on the badge of the node.
  * If there is no `nickname`, it will be truncated to 20 characters from the arbitrarily written title and used.
```
"""
@author: Dr.Lt.Data
@title: Impact Pack
@nickname: Impact Pack
@description: This extension offers various detector nodes and detailer nodes that allow you to configure a workflow that automatically enhances facial details. And provide iterative upscaler.
"""
```


* **Special purpose files** (optional)
  * `node_list.json` - When your custom nodes pattern of NODE_CLASS_MAPPINGS is not conventional, it is used to manually provide a list of nodes for reference. ([example](https://github.com/melMass/comfy_mtb/raw/main/node_list.json))
  * `requirements.txt` - When installing, this pip requirements will be installed automatically 
  * `install.py` - When installing, it is automatically called
  * `uninstall.py` - When uninstalling, it is automatically called
  * `disable.py` - When disabled, it is automatically called
    * When installing a custom node setup `.js` file, it is recommended to write this script for disabling.
  * `enable.py` - When enabled, it is automatically called
  * **All scripts are executed from the root path of the corresponding custom node.**

## Support of missing nodes installation

![missing-menu](misc/missing-menu.png)

* When you click on the ```Install Missing Custom Nodes``` button in the menu, it displays a list of extension nodes that contain nodes not currently present in the workflow.

![missing-list](misc/missing-list.png)

* Currently, support is not available for custom nodes that can only be downloaded through civitai.

* [ComfyUI_Custom_Nodes_AlekPet](https://github.com/AlekPet/ComfyUI_Custom_Nodes_AlekPet)


## Troubleshooting
* If your `git.exe` is installed in a specific location other than system git, please install ComfyUI-Manager and run ComfyUI. Then, specify the path including the file name in `git_exe = ` in the ComfyUI-Manager/config.ini file that is generated.
* If updating ComfyUI-Manager itself fails, please go to the **ComfyUI-Manager** directory and execute the command `git update-ref refs/remotes/origin/main a361cc1 && git fetch --all && git pull`.
 * Alternatively, download the update-fix.py script from [update-fix.py](https://github.com/ltdrdata/ComfyUI-Manager/raw/main/scripts/update-fix.py) and place it in the ComfyUI-Manager directory. Then, run it using your Python command.
   For the portable version, use `..\..\..\python_embeded\python.exe update-fix.py`.
* For cases where nodes like `PreviewTextNode` from `ComfyUI_Custom_Nodes_AlekPet` are only supported as front-end nodes, we currently do not provide missing nodes for them.
* Currently, `vid2vid` is not being updated, causing compatibility issues.


## TODO: Unconventional form of custom node list

* https://github.com/diontimmer/Sample-Diffusion-ComfyUI-Extension
* https://github.com/senshilabs/NINJA-plugin


## Roadmap

- [x] category/keyword filter
- [x] Automatic recognition of missing custom nodes
- [x] Automatic installation suggestion of missing custom nodes
- [x] 3rd party repository
- [ ] installation from git url 
- [ ] Specification of custom nodes
- [ ] Specification scanner
- [ ] Search extension by node name
- [ ] workflow downloader


# Disclaimer

* This extension simply provides the convenience of installing custom nodes and does not guarantee their proper functioning.

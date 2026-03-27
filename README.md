# onshape_description

This repository contains a template for a ROS2 robot description package. By convention, ROS2 packages
that exist solely to house a robot's URDF files and CAD meshes are usually titled "[robot name]_description".
These packages can be as simple as a single urdf file, but often include additional features, such as a launch file to preview the robot URDF (as this template provides). Overall, this repository is meant to exist
as a convention for our robot description packages.

**Rice Robotics Members!!!** In order for the attached workflow to work, you must create a *public* repository, as the OnShape API key is stored in our organization's secrets, which can only be accessed by a GitHub actions workflow in a public repository.

## Features
This template includes the following features:

1. Copying the URDF file and meshes to `install/` on a `colcon build`.
2. Previewing the urdf file with rviz2.
3. Loading in the urdf file from an Onshape assembly via [onshape-to-robot](https://onshape-to-robot.readthedocs.io/en/latest/index.html).
4. Specifying a joint name array as a python package.

### rviz2

The URDF model can be previewed in rviz2 by running this launch file:
```bash
ros2 launch onshape_description visualize.launch.py
```

### onshape-to-robot

This package provides the github action workflow `.github/workflows/update-urdf.yaml`,
which updates the contents of the package from the Onshape assembly specified in `config.json`. 
This can be run by going to the Actions tab, going to the Update URDF action, clicking on "run workflow", 
and specifying the main branch. The corresponding Onshape assembly document should be in the Rice Robotics Club
organization, as the API key used for fetching the files from Onshape are stored in the GitHub organization's secrets.
This allows anyone to perform this operation as members of the GitHub organization, without needing to obtain an API key.

In order for the import to work properly, the Onshape assembly needs to comply with the onshape-to-robot tool's joint name scheme.
This is specified in the documentation [here](https://onshape-to-robot.readthedocs.io/en/latest/design.html). The main requirement 
is that all mates corresponding to a URDF joint need to be named according to `dof_[joint name]`. There are additional features
that may be used, such as for handling kinematic loops.

### Python Joint Names

A joint name array constant is included asa python package, which can be imported via:
```Python
from onshape_description import JOINT_NAMES
```

## Usage
In order to extend from this template repository, the package name, robot name, and Onshape information need to be specified in the following files.

1. In `config.json`, specify the Onshape document, workspace, and element ids, along with the assembly name:
```json
{
  "robot_name": "robot",
  "package_name": "onshape_description",
  "assets_directory": "meshes",
  "output_filename": "urdf/robot",
  "output_format": "urdf",
  "document_id": "", # TODO: insert document_id (after d/ in url)
  "workspace_id": "", # TODO: insert workspace_id (after w/ in url)
  "element_id": "", # TODO: insert element_id (after e/ in url)
  "assembly_name": "" # TODO: insert OnShape assembly name
}
```

2. In `onshape_description/__init__.py`, specify the URDF's joint names:

```python
JOINT_NAMES = [
	"" # TODO: add all URDF joint names
]
```

All done!

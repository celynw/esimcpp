# esimcpp

Python bindings for simple access to the [ESIM event simulator](https://github.com/uzh-rpg/rpg_esim).
This package itself doesn't need ROS at runtime, but it does for building because it seems quite involved to link to ESIM otherwise.

## Usage

```python
from esimcpp import SimulatorBridge

bridge = SimulatorBridge(
	0.5, # Contrast threshold (positive): double contrast_threshold_pos
	0.5, # Contrast threshold (negative): double contrast_threshold_neg
	0.0, # Standard deviation of contrast threshold (positive): double contrast_threshold_sigma_pos = 0.021
	0.0, # Standard deviation of contrast threshold (negative): double contrast_threshold_sigma_neg = 0.021
	0, # Refractory period (time during which a pixel cannot fire events just after it fired one), in nanoseconds: int64_t refractory_period_ns
	True, # Whether to convert images to log images in the preprocessing step: const bool use_log_image
	0.001, # Epsilon value used to convert images to log: L = log(eps + I / 255.0): const double log_eps
	False, # Whether to simulate color events or not (default: false): const bool simulate_color_events
	# const double exposure_time_ms = 10.0, # Exposure time in milliseconds, used to simulate motion blur
	# const bool anonymous = false, # Whether to set a random number after the /ros_publisher node name (default: false)
	# const int32_t random_seed = 0 # Random seed used to generate the trajectories. If set to 0 the current time(0) is taken as seed.
)
fps = 30
time_in_ns = 0
for _ in range(20):
	# Get `rgb` (numpy image array) from some source
	events = bridge.img2events(rgb, int(time_in_ns))
	time_in_ns += (1 / fps)
	# Do something with `events`
```

## Installation

Requirements:

```bash
sudo apt-get install ros-noetic-pybind11-catkin
pip install empy
```

Build rpg_esim in the current workspace.
[My fork](https://github.com/celynw/rpg_esim/wiki/Installation) might be easier to install, especially for ROS noetic.

```bash
catkin build esimcpp
```

You *CAN* build this for Python 3 (think conda environment) with Ubuntu 18 or lower, for which ROS is based on Python 2!
Replace `path_to_python_binary` with your chosen python 3 binary, e.g. `venv/bin/python`

```bash
catkin build esimcpp --cmake-args -DPYTHON_VERSION=3.10 -DPYTHON_EXECUTABLE=path_to_python_binary
```

You might have to first clean and build `rpg_esim` this way too...

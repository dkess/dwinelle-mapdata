This repository contains data for generating the map of Dwinelle Hall found at [dkess.me/dwinelle](https://dkess.me/dwinelle/). Data was collected by taking videos of a walk through Dwinelle Hall, guided by the [Indoor Mapper Android app](https://github.com/dkess/IndoorMapperAndroid). The data in this repository is licensed under the [Open Database License (ODbL)](http://opendatacommons.org/licenses/odbl/1.0/).

The meanings of each file:

## `29.mp4`, `30.mp4`, `31.mp4`, `32.mp4`
These video files contain the actual GoPro footage of walking through Dwinelle. They are not included in this repo because they total 16GB. If you're interested in looking at these, send me an email.

## `nodes.json`
`nodes.json` is outputted directly from the Indoor Mapper Android app.
### nodes
The nodes array is indexed by node number-- the root node (0) is index 0 of the array. The "branches" object holds the nodes connected to this one, indexed by absolute direction.

### log
The log array is a listing of each edge walked in real time. Each log entry contains the node walked, which way the user turned after visiting that node (absolute, not relative direction), and the times that this node was entered and exited at. Times are measured in milliseconds since the epoch.

## `times`
A space-delimited text file. Each line represents a video file.

* Column 1: the name of the video file, with extension stripped.
* Column 2: time that the video started at, measured in milliseconds since the epoch.
* Column 3: how long the video is, measured in milliseconds.

## `walk*.txt`
These files are generated from running the [walking.lua MPV script](https://github.com/dkess/dwinelle-tools/blob/master/luatest/walking.lua) on each video file. It contains information about when walking was actually taking place (as opposed to standing/waiting). The letter i indicates that walking has stopped at that point in the video, and the letter y indicates that walking has started. Times are measured in seconds since the start of the video. If an edge has multiple entries for either i or y, the last one is used. If an edge doesn't have any entries for either i or y, it is assumed that walking started or ended at the start and end of the log entry (based on `time_enter` and `time_exit` keys in `nodes.json`).

## `rooms*.txt`
These files are generated from running the [roomentry.lua MPV script](https://github.com/dkess/dwinelle-tools/blob/master/luatest/roomentry.lua) on each video file. It contains information about when the camera passed room entrances. Times are measured in seconds since the start of the video. Some room names have special meaning:
* `w` - women's restroom
* `m` - men's restroom
* `mw` - gender-neutral restroom
* `u` - going up stairs (timestamp is bottom of a staircase)
* `d` - going down stairs (timestamp is top of a staircase)
* `e` - elevator

## `equal_edges`
A text file, where the # character represents a comment. `(a, b) (c, d) -(e, f)` means that the combined lengths of edges `(a, b)` and `(c, d)` are equal to the length of edge `(e, f)`. The order of the nodes does not matter (`(a, b)` is equivalent to `(b, a)`). These are used in the edge length calculation algorithm.

## `equal_heights`
A text file, where the # character represents a comment. Each line is a set of nodes, asserting that they are all at the same elevation.

## `equal_nodes`
A text file, where the # character represents a comment. Each line is a set of nodes, asserting that they are all at the same x, y coordinates (though not necessarily at the same elevation).

## `forks`
A text file. Each line represents a T-shaped "fork". This file assigns names to forks, which can be useful when providing navigation directions.

## `groups`
A text file. Each paragraph represents a "group" of nodes, which are nodes within which the user does not need explicit directions to go between (like staircases). The first line in each group is a set of nodes in that group. The second line is the name of the group. The proceeding lines are aa list of "exits" from the group, and the name of each "exit".

## `height_override`
TODO

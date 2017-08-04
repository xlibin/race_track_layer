**race_track_layer**
==================

## Attaching Plugin

* First, copy the race\_track\_layer to the *catkin_ws/src*. Then, do *catkin_make*.

*  In order to attach *race\_track\_layer* plugin, insert

```
- {name: race_track_layer,       type: "costmap_2d::RaceTrackLayer"}
```

to array of plugins of local costmap and global costmap just below the *obstacle_layer*.

## Explanation

* *race\_track\_layer* detects all footprints that are published by robots whatever their names are.

* *race\_track\_layer* customizes footprints for every robot. For example, own footprint of a robot cannot be an obstacle on own costmap.

* *race\_track\_layer* fills polygons that are published by robots, with *LETHAL_OBSTACLE*.

* *race\_track\_layer* clears old footprint locations and creates obstacles new footprint areas.

* *race\_track\_layer* detects other robot's speeds and directions, then sets inflation cost of 120 to their race tracks.

* Race tracks are triangles that start from midpoints of footprints of robots.

* Length of a race track depends on velocity(speed and direction) of robot. If speed is under 1 m/s, x and y coordinates of pioneer edge of the triangle will be: 
```
c.x = mid.x + cos(angle) * 5 ;
c.y = mid.y + sin(angle) * 5 ;
```

* After position of race track triangle is changed, *race\_track\_layer* clears cells whose costs are 120. Therefore, *race\_track\_layer* does not damage effects of other layers.

* In order to prevent from planner failure, race track triangle is filled with cost of 120. 

| Range        | Collision Status   |
| ------------- | ------------- |
| 253 - 254 | definitely in collision |
| 253 - 128 | possibly in collision |
| 127 - 1 | definitely not in collision |
| 0 | free space |


* The robot can make plans passing through other robot's race tracks. However, costs will be bigger near the robot because of the triangular structure of the race tracks.

* http://imgur.com/a/HJC0Y

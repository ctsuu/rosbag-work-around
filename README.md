# Extract rosbag information without ROS installed 

I was fighting for rosbag play for a while, still don't have good hand about it. There are some way to work around it. 

# Extract image and save into video format

The benifit of comment video format such as mp4 is small in size, easy to handle. 

First, you need anconv tools, it is part of the libav tools. Just install those:
```
sudo apt install libav-tools
```
Next, there is a nice little program [rosbag2video](https://github.com/mlaiacker/rosbag2video) available for clone. 
```
git clone  https://github.com/mlaiacker/rosbag2video
```
Now, go to you local directory, 
```
$ cd rosbag2video
$ python rosbag2video.py -r 30 -o udacity-dataset.mp4 udacity-dataset_sensor_camera_center_2016-10-20-15-13-30_0.bag
```
Half hour later, you will get udacity-dataset.mp4 file (744MB), instead of bag file(4.2GB). 

# Extract other sensors log from bag file

While you still in bag file directory, run the following code:
```
find . -name "*.bag" -exec rostopic echo -b {} -p "/vehicle/steering_report" > steering_report_all_mixed.csv \;
grep -v "field.header" steering_report_all_mixed.csv | awk 'BEGIN{FS=","} {print $1" "$5}' | sort -n | uniq > steering_report_all_sorted_uniq.csv
wc steering_report_all_sorted_uniq.csv
```

# Examples:
If you have chance open a bag file
```
$ rosbag info *.bag
path:        HMB_3_release.bag
version:     2.0
duration:    4:40s (280s)
start:       Nov 17 2016 16:30:41.18 (1479425441.18)
end:         Nov 17 2016 16:35:21.89 (1479425721.89)
size:        231.0 MB
messages:    11228
compression: none [300/300 chunks]
types:       sensor_msgs/CameraInfo      [c9a58c1b0b154e0e6da7578cb991d214]
             sensor_msgs/CompressedImage [8f7a12909da2c9d3332d540a0977563f]
topics:      /center_camera/camera_info              5614 msgs    : sensor_msgs/CameraInfo     
             /center_camera/image_color/compressed   5614 msgs    : sensor_msgs/CompressedImage
```
Modify the extract commend to mach topics: 
```
find . -name "*.bag" -exec rostopic echo -b {} -p "/center_camera/camera_info" > camera_report_all_mixed.csv \;
```
then, clean up some unnecessary columums:
```
grep -v "field.header" camera_report_all_mixed.csv | awk 'BEGIN{FS=","} {print $1" "$5}' | sort -n | uniq > camera_report_all_sorted_uniq.csv
```
```
wc camera_report_all_sorted_uniq.csv
```
open the camera_report_all_sorted_uniq.csv, I get the following info:
```
1479425441183205527 0
1479425441233038380 0
1479425441283030878 0
1479425441333087118 0
1479425441387141307 0
1479425441433005242 0
```
according to the bag time stamp:
```
start:       Nov 17 2016 16:30:41.18 (1479425441.18)
end:         Nov 17 2016 16:35:21.89 (1479425721.89)
```
you can split the string to time stamp and extension. 

However, the final_example.csv frame_id is different from the bag extraction. 
```
frame_id,steering_angle
1479425441182877835,-0.373665106110275
1479425441232704425,-0.0653962884098291
1479425441282730750,-0.160735441371799
1479425441332806714,0.317896057851613
1479425441382790272,0.19649308398366
1479425441432724303,-0.341386346518993
1479425441482746958,-0.0239574497565627
```
Only these frame_ids match picture's file name and grading system. 
*1479425441182877835.jpg


Enjoy the new challenge. 






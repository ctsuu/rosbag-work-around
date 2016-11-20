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
find . -name "*.bag" -exec rostopic echo -b {} -p "/can_bus_dbw/can_rx" > canbus_report_all_mixed.csv \;
grep -v "field.header" canbus_report_all_mixed.csv | awk 'BEGIN{FS=","} {print $1" "$5}' | sort -n | uniq > canbus_report_all_sorted_uniq.csv
wc canbus_report_all_sorted_uniq.csv
```
Now, you will get two csv canbus sensor logs. 

For earlier dataset, the bag files are arranged diffenently:
```
find . -name "*.bag" -exec rostopic echo -b {} -p "/vehicle/steering_report" > steering_report_all_mixed.csv \;
grep -v "field.header" steering_report_all_mixed.csv | awk 'BEGIN{FS=","} {print $1" "$5}' | sort -n | uniq > steering_report_all_sorted_uniq.csv
wc steering_report_all_sorted_uniq.csv
```





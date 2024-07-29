**Errors Encountered While Running the ROS2 on Raspberry pi**
1) Launching of rclcpp_timer
````
vxWorks *]# /usr/lib/examples_rclcpp_minimal_timer/timer_lambda
Launching process '/usr/lib/examples_rclcpp_minimal_timer/timer_lambda' ...
Process '/usr/lib/examples_rclcpp_minimal_timer/timer_lambda' (process Id = 0xffff800000451a00) launched.
/usr/lib/examples_rclcpp_minimal_timer/timer_lambda: Undefined PLT symbol "__aarch64_ldadd4_relax" (symnum = 1)
0xffff80000 (iTimer_lambda): RTP 0xffff800000451a00 has been deleted due to signal 11.
````
2) Launching demo_node_py
````
vxWorks *]# /usr/lib/demo_nodes_py/talker
Launching process '/usr/lib/demo_nodes_py/talker' ...
rtp exec: unable to launch process "/usr/lib/demo_nodes_py/talker": Not a valid ELF (S_loadElfLib_HDR_READ).
````
3) Using ROS2 cliinterface
````
[vxWorks *]# python3 ros2 run demo_nodes_py talker
Thu Jan  1 00:05:02 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
Thu Jan  1 00:05:03 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
Thu Jan  1 00:05:04 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
Thu Jan  1 00:05:05 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
Thu Jan  1 00:05:06 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
Thu Jan  1 00:05:07 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
Thu Jan  1 00:05:08 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
Thu Jan  1 00:05:09 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
Thu Jan  1 00:05:10 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
Thu Jan  1 00:05:11 1970: ipnet[33020]: Warning: connect failed on socket 25, 192.168.0.2 is unreachable
````
4) Once the command line is lost, again we have to resrat the raspberry pi and minicom for serial communication. 



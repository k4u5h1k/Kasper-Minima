---
layout:     post
title:      ROS for dummies
date:       2021-06-01
categories: programming
---

Off the bat, ROS is not an operating system, or even remotely complicated.

### WHAT YOU NEED TO UNDERSTAND THIS POST

- Basic understanding of Python.

- Basic linux terminal usage.

  

### WHAT IS ROS AND WHY SHOULD I USE IT?

- At its core it is simply a middleman between software written using ROS tools, and your operating system, similar to Docker.

- It provides tons of features for controlling very low level hardware with
  few lines of a popular language, data visualization, simple data transfer between all your components and a helluva lot more.

- People all over the world have already written tons of packages to do all sorts of stuff with every obscure component imaginable, and the majority of that software is open source!

  

## WHAT WILL THIS POST TEACH ME?

You will be able to use python to write basic ROS pubsub programs, or at the very least you'll understand how ROS works. Hopefully you don't understand it well enough to figure out that I only need one-fifth of my proposed project deadline to finish said project.

However, sadly I will not dig into the GUI elements like Gazebo and rqt because that is more elaborate and requires a lot more time to teach properly. Perhaps another time.

## INTRODUCTION

A robotic system running on ROS software is made up of several **nodes**. Each node is responsible for exactly **one** purpose, like controlling the speed, or the steering wheel or a camera. Nodes can talk to each other and function as one entity through **pubsub**, **services**, **actions** or **parameters**.

### You just bombarded me with big words, what do they mean?

When you subscribe to a magazine on some topic, the publisher periodically sends you the exact magazine you subscribed for. However you as a subscriber cannot send anything to the publisher. This is exactly what happens between nodes. node 1 can subscribe to node 2 on a particular topic, which means that node 1 can listen to node 2 talk about that particular topic. Nodes are free to publish or subscribe to as many other nodes as they want but the communication along a single connection is always one way. A service is not much different except now the communication is two way kind of like a client-server interface and the server provides whatever data the client requests.

![Nodes-TopicandService](/savvy/assets/images/Nodes-TopicandService.gif)

An action is same as a service but it is not blocking, which means that the client can do whatever it wants while it waits for a response from the action server. Parameters are global variables that all nodes can access (and very rarely modify).

## ENOUGH THEORY, LETS JUMP IN

The rclpy library, contains everything you need to write and run elegant systems in ROS. To start with, lets make a node that does nothing, just to become familiar with the syntax. Then we will move on to two nodes, one that publishes magazine issues periodically and another that subscribes to the publisher and recieves the issues as they are sent out.

**First load up your ROS environment variables by sourcing the setup**

```bash
Kaushik ros2-osx % source setup.zsh
```



### SIMPLE NODE

#### Execution

```bash
Kaushik tutorial % python3.8 first_node.py
just sits pretty until i hit Control-C
^C
```



#### Code

```python
import rclpy
from rclpy.node import Node

def main():
    rclpy.init()
    first_node = Node('my_first_node')
    rclpy.spin(first_node)

main()
```

Let us dissect this,       

```python
import rclpy
from rclpy.node import Node
```

Self explanatory, first we import the `rclpy` library and the `Node` class,

```python
def main():
    rclpy.init()
    first_node = Node('my_first_node')
```

We initiate the library and then instantiate our `node`,

```python
	rclpy.spin(first_node)
```

If we just instantiate our node and dont put this line the program execution will stop there. If we want to keep our node running until we kill it with `Control-C` or otherwise, this must be used.  

Congratulations you just learnt to write a node! Lets move on to something more advanced.

Now we will write a two nodes that send magazine issues using pubsub!
The publisher will send magazine issues as a string and the subscriber will recieve them in realtime.

### PUBLISHER

#### Execution

```bash
Kaushik first_pubsub % python3.8 publisher.py
Sending: issue number 1
Sending: issue number 2
Sending: issue number 3
Sending: issue number 4
Sending: issue number 5
Sending: issue number 6
^C
```



#### Code

```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class MagazinePublisher(Node):
    def __init__(self):
        super().__init__('magazine_publisher')
        self.playboy = self.create_publisher(String, 
                'fashion',
                10)
        self.timer = self.create_timer(1, 
                self.timer_callback)
        self.i = 1

    def timer_callback(self):
        msg = String()
        msg.data = 'issue number ' + str(self.i)
        self.playboy.publish(msg)
        print(f'Sending: {msg.data}')
        self.i += 1

rclpy.init()
magazine_publisher = MagazinePublisher()
rclpy.spin(magazine_publisher)
```

Commencing surgery,

```python
import rclpyfrom rclpy.node import Node
from std_msgs.msg import String
```

Apart from our usual imports there is a new one. In this project we are not only creating a node but also sending stuff which needs to be in a `String` class (not to be confused with the string data type). The actual message goes in `String.data`,

```python
class MagazinePublisher(Node):    
  def __init__(self):        
    super().__init__('magazine_publisher')        
    self.playboy = self.create_publisher(String,                 
                                         'fashion',                
                                         10)
```

Here we declare our publisher class that declares itself as a Node by the name `magazine_publisher`, and publishing on the topic `fashion`  ~~Didn't know what else to call playboy sorry~~ with a buffer size of `10`.  


```python
  self.timer = self.create_timer(1, 
                self.timer_callback)
  self.i = 1
```

Now we create a `timer` that calls our sender function every 1 second.

```python
  def timer_callback(self):
      msg = String()
      msg.data = 'issue number ' + str(self.i)
      self.playboy.publish(msg)
      print(f'Sending: {msg.data}')
      self.i += 1
```

This function gets called by the `timer` every second. It uses the `playboy` object to send magazine issues as messages.

```python
def main():
    rclpy.init()
    magazine_publisher = MagazinePublisher()
    rclpy.spin(magazine_publisher)

main()
```

Like the first node we initiate `rclpy` then instantiate `MagazinePublisher` and spin it.



### SUBSCRIBER

#### Execution

```bash
Kaushik first_pubsub % python3.8 subscriber.py
Recieved issue number 1
Recieved issue number 2
Recieved issue number 3
Recieved issue number 4
Recieved issue number 5
Recieved issue number 6
```



#### Code

```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class MagazineSubscriber(Node):
    def __init__(self):
        super().__init__('magazine_subscriber')
        self.subscription = self.create_subscription(String,
            'fashion',
            self.listener_callback,
            10)

    def listener_callback(self, msg):
        print('Recieved '+msg.data)

def main():
    rclpy.init()
    magazine_subscriber = MagazineSubscriber()
    rclpy.spin(magazine_subscriber)

main()
```

The `subscriber` only recieves the message and prints it out so its relatively simple.

```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String
```

Familiar imports. 

```python
class MagazineSubscriber(Node):
    def __init__(self):
        super().__init__('magazine_subscriber')
        self.subscription = self.create_subscription(String,
            'fashion',
            self.listener_callback,
            10)
```

Making a `subscriber` class and instantiating its parent with the name `magazine_subscriber` . We then create a subription to the `fashion` topic to recieve `String` data, with a buffer time of 10 (*doesn't matter as long as the callback function does not take too long to execute*). And whenever a message is recieved `listener_callback` function is called.

```python
    def listener_callback(self, msg):
        print('Recieved '+msg.data)
```

In the `listener_callback` function we simply print the recieved message. meh

```python
def main():
    rclpy.init()
    magazine_subscriber = MagazineSubscriber()
    rclpy.spin(magazine_subscriber)

main()
```

Familiar routine to run once and then `spin` the `subscriber` forever.

**NOW CAN THIS BE USED TO CONTROL AN RC CAR?**  <sup> yep </sup>

## PACKAGES

The actual beauty of ROS lies in its package system. Upto this point we were using `python3.8` to run our scripts but basically any ROS software can be converted into sort of an executable package which can then be directly run without even mentioning the language.

Ideally you would start writing your code inside an empty project and once done just run `colcon build` in the project root and `source project_root/install/setup.zsh`  

Lets try converting our magazine pubsub to a package,

### CREATING AND RUNNING A PACKAGE

#### STEP-1:

`cd` into `catkin_ws/src` 

```bash
Kaushik ros2-osx % cd catkin_ws/src
Kaushik src % pwd
/Users/Kaushik/ros2_foxy/ros2-osx/catkin_ws/src
```

Create an empty package using the command `ros2 pkg create --build-type ament_python magazine` . You should now see a new magazine directory. This is called the **project root**.

```bash
Kaushik src % ros2 pkg create --build-type ament_python magazine
going to create a new package
package name: magazine
destination directory: /Users/Kaushik/ros2_foxy/ros2-osx/catkin_ws/src
package format: 3
version: 0.0.0
description: TODO: Package description
maintainer: ['Kaushik <kaushik.sivashankar@gmail.com>']
licenses: ['TODO: License declaration']
build type: ament_python
dependencies: []
creating folder ./magazine
creating ./magazine/package.xml
creating source folder
creating folder ./magazine/magazine
creating ./magazine/setup.py
creating ./magazine/setup.cfg
creating folder ./magazine/resource
creating ./magazine/resource/magazine
creating ./magazine/magazine/__init__.py
creating folder ./magazine/test
creating ./magazine/test/test_copyright.py
creating ./magazine/test/test_flake8.py
creating ./magazine/test/test_pep257.py
Kaushik src % ls
examples        magazine
```

`magazine` has a structure like,

```bash
Kaushik src % tree magazine
magazine
├── magazine
│   └── __init__.py
├── package.xml
├── resource
│   └── magazine
├── setup.cfg
├── setup.py
└── test
    ├── test_copyright.py
    ├── test_flake8.py
    └── test_pep257.py

3 directories, 8 files

```



#### STEP-2:

Awesome now lets move our publisher and subscriber scripts into `magazine/magazine`. All of your nodes need to be in this <package_name>/<package_name> script. ezpz

```bash
Kaushik magazine % cd magazine
Kaushik magazine % cp ../../../../../tutorial/first_pubsub/* .
Kaushik magazine % ls
__init__.py     publisher.py    subscriber.py
```



#### STEP-3:

Now we must register the two scripts to be built into executables in the `setup.py` of the project's root.

```python
entry_points={
          'console_scripts': [
              'publisher = magazine.publisher:main',
              'subscriber = magazine.subscriber:main'
          ],
		}
```

And lastly,

#### STEP-4:

We must tell the ament build system that we have `rclpy` and `std_msgs` as dependencies that also need to be built into executables.

  ```python
<export>
    <build_type>ament_python</build_type>
    <exec_depend>rclpy</exec_depend>
    <exec_depend>std_msg</exec_depend>
</export>
  ```

#### STEP-5:

Awesome! Lets build it up! Simply navigate back to project root and run `colcon build`

```bash
Kaushik magazine % pwd
/Users/Kaushik/ros2_foxy/ros2-osx/catkin_ws/src/magazine
Kaushik magazine % colcon build
Starting >>> magazine
Finished <<< magazine [2.17s]

Summary: 1 package finished [3.21s]
```

Yay! We built the package! You should now see a new install folder in project root. Now simply `source install/setup.zsh` to add our new executable to `PATH`. Now we can run it using `ros2 run <package_name> <node_name>`

```bash
Kaushik magazine % source install/setup.zsh
[connext_cmake_module] Warning: The location at which Connext was found when the workspace was built [[/Applications/rti_connext_dds-5.3.1]] does not point to a valid directory, and the NDDSHOME environment variable has not been set. Support for Connext will not be available.

Kaushik magazine % ros2 run magazine publisher
Sending: issue number 1
Sending: issue number 2
Sending: issue number 3
Sending: issue number 4
^C
```

Lets quickly recap what we just did.

I really wanted to include services and rqt and gazibo but that has tons of stuff, requires more time and a slight learning curve only makes it more foreign and boring to audience that isnt 100% interested. Maybe I can cover that in a separate session.






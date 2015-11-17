---
title: Multi Node
layout: default
---

# Multi Node

## Quest objectives

## Getting started

## Installation

Install dependency module garethr-docker

{% task 1 %}
---
- execute: puppet module install garethr-docker
{% endtask %}

## Classification with the site.pp manifest

Note: Do not add this to the "default" classification, it must only be on 'learning.puppetlabs.vm'.  If you use default it will try to apply it to the sub-nodes.

{% highlight puppet %}
node 'learning.puppetlabs.vm' {
  include multi_node
}
{% endhighlight %}

1. run `puppet agent -t` to setup the docker environment

1. run `docker ps` to see the new nodes called "webserver" and "database"

1. run `docker exec -it webserver bash`
This tells docker to "execute" the command "bash"  using -i for interactive and -t for attaching a terminal.  It gives you a shell inside your terminal.

1. Once you're logged in the the bash shell on the container, use the curl based installer from the PE console to install the agent on this node.

Note: You will see an error about the agent package not being available.  Add the package by adding the class "pe_repo::platform::ubuntu_1404_amd64" to the PE Masters group and running `puppet agent -t` on your master.

1. Sign the node's certificate using the PE console.

1. From the bash prompt on the container, run `puppet agent -t`

1. Once the run is complete, hit CTRL-D or type "exit" to return to the learning VM.

1. Return to the console to view reports from the new node.

1. Repeat for the "database" node.

1. Once the nodes are set up, you can trigger commands to run inside the container by using `docker exec <nodename> <command>` so to run `puppet agent -t` one the "database" node, you can use the command `docker exec database puppet agent -t`.  
Note: This is only works for simple commands, those that use | character or input/output redirection won't work.


## Classifying nodes
To begin we'll set up some basic classification on our new nodes.

1. Install these modules on the master "jfryman-nginx" and "puppetlabs-mysql"

1. In the PE console create new node groups, one for "webserver.learning.puppetlabs.vm" and one for "database.learning.puppetlabs.vm"

1. Classify the webserver node with the 'nginx' class and database node with the 'mysql::server' class.

1. Trigger a puppet run on each node.

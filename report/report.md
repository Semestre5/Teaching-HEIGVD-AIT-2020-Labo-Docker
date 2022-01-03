# RAPPORT - Labo 4 - STI

Auteurs : Axel Vallon, Lev Pozniakoff et Robin Gaudin

Date : 13.12.2021

## Table of content

[TOC]

## Introduction

//TODO An introduction describing briefly the lab

## Identify issues and install the tools

1. <a name="M1"></a>**[M1]** Do you think we can use the current
   solution for a production environment? What are the main problems
   when deploying it in a production environment?

   **Réponse : **TODO

------

2. <a name="M2"></a>**[M2]** Describe what you need to do to add new
   `webapp` container to the infrastructure. Give the exact steps of
   what you have to do without modifiying the way the things are
   done. Hint: You probably have to modify some configuration and
   script files in a Docker image.

   **Réponse : **TODO

------

3. <a name="M3"></a>**[M3]** Based on your previous answers, you have
   detected some issues in the current solution. Now propose a better
   approach at a high level.

   **Réponse : **TODO

------

4. <a name="M4"></a>**[M4]** You probably noticed that the list of web
   application nodes is hardcoded in the load balancer
   configuration. How can we manage the web app nodes in a more dynamic
   fashion?

   **Réponse : **TODO

------

5. <a name="M5"></a>**[M5]** In the physical or virtual machines of a
   typical infrastructure we tend to have not only one main process
   (like the web server or the load balancer) running, but a few
   additional processes on the side to perform management tasks.

   For example to monitor the distributed system as a whole it is
   common to collect in one centralized place all the logs produced by
   the different machines. Therefore we need a process running on each
   machine that will forward the logs to the central place. (We could
   also imagine a central tool that reaches out to each machine to
   gather the logs. That's a push vs. pull problem.) It is quite
   common to see a push mechanism used for this kind of task.

   Do you think our current solution is able to run additional
   management processes beside the main web server / load balancer
   process in a container? If no, what is missing / required to reach
   the goal? If yes, how to proceed to run for example a log
   forwarding process?

   **Réponse : **TODO

------

6. <a name="M6"></a>**[M6]** In our current solution, although the
   load balancer configuration is changing dynamically, it doesn't
   follow dynamically the configuration of our distributed system when
   web servers are added or removed. If we take a closer look at the
   `run.sh` script, we see two calls to `sed` which will replace two
   lines in the `haproxy.cfg` configuration file just before we start
   `haproxy`. You clearly see that the configuration file has two
   lines and the script will replace these two lines.

   What happens if we add more web server nodes? Do you think it is
   really dynamic? It's far away from being a dynamic
   configuration. Can you propose a solution to solve this?

   **Réponse : **TODO



**Deliverables**

1. Take a screenshot of the stats page of HAProxy at
   <http://192.168.42.42:1936>. You should see your backend nodes.

   ![image-20220103110918747](figures/image-20220103110918747.png)

2. Give the URL of your repository URL in the lab report.

   https://github.com/Semestre5/Teaching-HEIGVD-AIT-2020-Labo-Docker

## Add a process supervisor to run several processes

1. describe which problem exists with the current solution based on the previous explanations and
   remarks. Propose a solution to solve the issue.

Le problème actuel est que le cluster peut continuer à exister malgrè le fait que le node de load-balancing ne soit plus en marche. Ceci n'est donc pas une solution tout à fait adaptée à notre environnement. L'idéal serait que les node puisse rejoindre le node principal seulement si le load-balancer est allumé. Une solutions qui pourrait être utilisée est l'utilisation de `join`

```
 serf join ip_load_balancer
 # https://www.serf.io/intro/getting-started/join.html
```



## Add a tool to manage membership in the web server cluster



## React to membership changes



## Use a template engine to easily generate configuration files



## G-enerate a new load balancer configuration when membership changes



## Make the load balancer automatically reload the new configuration



## Difficulties



## Conclusion
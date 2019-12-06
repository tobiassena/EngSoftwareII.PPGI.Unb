# EngSoftwareII.PPGI.Unb
Project executed for course Software Engineer II PPGI/UnB.

The aim of the project is evalute Expruna and verify feasibility of unattended execution of experiment taken from paper: Assessing a Swarm-GAP Based Solution for the Task Allocation Problem in Dynamic Scenarios. Details of the experiment are given on: https://github.com/junieramorim/replicationMaterial

Expruna can be accessed from: https://expruna.github.io/ 


## How to run your own experiment

ExpRunA uses Docker containers for its execution. For a better understanding of the tool, use the official website https://www.docker.com.
Among the containers used by the framework, one of them uses the image *eneiascs/dohko-job*, responsible for ExpRunA's infrastructure. If your experiment requires programs, libraries, or any dependencies that are not installed on the dohko-job image, you will need to make an extension of that image. Here's an example of an extended dohko-job image, with programs and dependencies added to run the hylaa experiment:


![alttext](https://github.com/tobiassena/EngSoftwareII.PPGI.Unb/blob/master/images/img1.png)


To create an extended dohko-job image, you must use it as a source in the Dockerfile file, as noted in line 1 of the image. From line 2 to 40 all the necessary commands were executed to leave the environment with all the necessary dependencies for the execution.


## Docker-compose

Docker-compose.yml is the file responsible for orchestrating all containers required for the experiment. By default, the dohko container is created from the *eneiascs/dohko-job* image, as in the image below:


![alttext](https://github.com/tobiassena/EngSoftwareII.PPGI.Unb/blob/master/images/img2.png)


If your experiment needs to extend the original image, you will need to change the image that will create dohko, as shown below:


![alttext](https://github.com/tobiassena/EngSoftwareII.PPGI.Unb/blob/master/images/img3.png)


Now, to run ExpRunA just use the docker-compose up command to upload all containers.
Some errors may occur as you will need to clear all ports used by the containers. The most common is that used by mysql. In this case, use the command *ps aux | grep mysql* to find out the number of the process your computer is running mysql on. Then kill the process with the *sudo kill XX* command (XX is the process number). In addition, it is also necessary to edit the .env file, changing the directory where the files will be stored locally and setting the configuration of the CPUs used by the containers. You can now rerun the *docker-compose up* command.
After using the docker-compose up command, you can verify that all containers required for execution are active by using the docker stats command.


![alttext](https://github.com/tobiassena/EngSoftwareII.PPGI.Unb/blob/master/images/img4.png)


If you have 6 active containers, as the image above, every ExpRunA environment is available to perform.
Now access your browser at http://localhost/autoexp/texteditor to use the tool.


## Using ExpRunA

As one of ExpRunA's purposes is to abstract the experiment's configuration, execution and analysis, little information is needed for its use. Basically, it will be necessary to inform the search hypotheses, the dependent variables, the treatments, the objects and the execution command. See an example of using DSL at https://github.com/eneiascs/dsm-experiments-evaluation/blob/master/hylaa/hylaa.exp.
Line 5 to 8 describes all the hypotheses that will be analyzed. Note that in all cases the dependent variable to be tested is time. You can choose the dependent variable you want.
In line 21 is described which instrument will capture the result of the execution of the experiment. In this case, valueExpression is the term that will refer to the dependent variable in this experiment defined as runtime :. Thus, ExpRunA will identify during execution which value it will have after the term defined in the valueExpression, and this value will be identified as the dependent variable.
In lines 28 to 32 you can describe all the possible treatments that your experiment will use. ExpRunA will identify an error if there is a hypothesis referencing a treatment or dependent variable that is not specified.
Line 76 specifies the command that will be executed by ExpRunA. In this command you can pass parameters to the defined treatments or objects as needed.
After specifying all the experiment information, you can perform the script generation and execution by right-clicking on the specification file and selecting Generate and Run.


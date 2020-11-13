# Fian-Ilham-Pratama
Genetic Algorithms for Unmanned Surface Vehicle
Final Project: Dynamic Path Planning of USV Based on Genetic Algorithm with Sliding Curve Guidance System
Name: Fian Ilham Pratama
WA number: 089695765720
email: fianilham1@gmail.com
Matlab version: R2018b

Procedure for Running USV Dynamic Path Planning Program Based on genetic algorithm (GA) with sliding curve guidance
############################################### ######
1. Open and run the file: Step1_Parampam.m
-> to declare USV parameter in Matlab workspace

2. Open and run the file: Step2_versi2_FIX_K Configuration2_Simulink.slx
-> to display dynamic path planning USV simulation in realtime
  (6DOF USV Model + Discrete PID MRAC + sliding curve guindace + GA-based path planning algorithm)

############################################### ######
Description of each file:
1. mapGlobal_Genetics_Algorithm.m => GA-based global path planning file where this program file will be called for
planning the shortest path between the starting point and the destination point in a static environment (static obstacle only)
* global GA parameters are in this file

2. mapLocal_Genetics_Algorithm.m => GA-based local path planning file where this program file will be called for
planning the shortest path between USV position points when there is a unpredicted obstacle and the destination point
in dynamic environment (can be static / dynamic obstacle) and will update the path generator
* ocal GA parameters are in this file

3. PID_MRAC_Diskrit_K DriverTEORI.m and PID_MRAC_Diskrit_TranslasiTEORI.m are Discrete PID MRAC program files for settings
USV translation speed and USV heading angle
translation speed ref model used => 1 / (16s + 1)
translation speed ref model used => 1 / (4.9s + 1)

4. Step1_Parampam.m is a program file containing all USV parameters and external noise gain used

5. Step2_versi2_FIX_K Configuration2_Simulink.slx is the simulation program file
   -> Blok Matlab function "Global Map"
      = Declaration of map size, size and position of static obstacle, starting waypoint and destination
   -> Blok Interpreted Matlab function "GLOBAL Path Planning"
      = Calling mapGlobal_Genetics_Algorithm.m
   -> Blok Interpreted Matlab function "LOCAL Path Planning"
      = Calling mapLocal_Genetics_Algorithm.m
   -> Block Matlab function "Path Generator"
      = Generate path planning result path (global / local)
   -> Block Matlab function "Dynamic Path Planning"
      = Declaration of size, position and speed of dynamic obstacle, contains all program commands and simulation plot
      = There are parameters such as the safety distance used by USV -> the accuracy of the collision prediction algorithm
   -> Blok Matlab function "Waypoint Transformation"
      = Aligns the waypoint set position between the USV model block (output X, Y, yaw) and the simulation display in the figure
   -> Block subsystem "USV Dynamic, Controller and Guidance"
      = Contains 6 dof USV model, MRAC PID, sliding curve guidance

%%%%%%%%%%%%%%%%%%%%%%%%%
note: a. Because all simulations are combined into one / realtime (USV and GA), the simulation will run slowly,
      b. There is a simulation pause when an obstacle is detected. This is due to the process of finding a new path
         collision-free (GA) which is run based on the order of execution of the Matlab program (1 processor)
      c. The number of TOTAL obstacles is limited to 5 obstacles including static and dynamic

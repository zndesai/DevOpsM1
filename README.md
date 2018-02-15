# CSC519 – Project milestone 1:  Configuration Management and Build Milestone

![image](https://github.ncsu.edu/rbhatt/CSC519--DevOps-M1/blob/master/screenshots/definition.png)

## Screencast: 
#### Part 1 - CheckboxIO build and post-build configuration
https://www.youtube.com/watch?v=ElBVSVM7FZE&feature=youtu.be
#### Part 2 - iTrust build and post-build configuration
https://youtu.be/Ik3pa8Qpq30

## Report:
#### Challenges faced: 
* <b>Git credentials:</b>  To clone the ncsu git repository, git username and password were required. And since it was the first time we were configuring the git token usage, it took us a lot of time to find a suitable solution. Since exposing the credentials in the environment variables was not the viable option, we finally used Jenkins-cli with create-credentials-by-xml command and remove the xml once the credentials got copied and added to the Jenkins. 
 
* <b>Restarting the Jenkins:</b> While downloading the Jenkins-cli and installing Jenkins plugins, the task was failing every time with the error cannot access request URL.  After investing a lot of time in this particular issue, we found that after restarting the Jenkins in the step before, we were not providing wait for it to get up and running. We had to introduce the wait task after every restart of Jenkins. 

* <b>Creating the job:</b> Creating a build job was another conflicting process. We created XML configuration file for the job, and used jenkins_job plugin create the job, but it was not working for some reason. After investing a lot of efforts in this issue, we finally decided to use Jenkins-cli jar with Jenkins admin credentials to create the job. 


* <b>Building the job:</b> After adopting the same approach to build the file using Jenkins-cli, we stumbled across the error where the Jenkins could not find the job created in the build task. But same job was listed in the task above (We listed the jobs to verify the creation). 
One alternative way was to use shell with curl and create the build using that. However, for curl to work, special crumbs have to be created using apis. Even though we were able to make the curl command work and trigger the build, the output for curl was an HTML response and we had to use/expose that crumb in the environment variable. 
We decided to find a workaround for Jenkins-cli and finally found a solution where using username:token@URL instead of URL did the trick.

* <b>Post build script:</b> Issue with using postbuild-script plugin was that the distribution was no longer supported. So we decided to use post build task plugin, but there was no online documentation on how to use this plugin with ansible or with shell. The breakthrough was to use the plugin in your Jenkins, apply the configuration and copy that configuration from Jenkins config.xml file. Once we get the xml syntax, all we need to do is use your post build shell script within <script> tags. 
<br>Also, Vagrant up command worked perfectly while configuring the checkbox io using command line, but when we were using the same command in ansible playbook, we were facing this error: 
```
Vagrant is attempting to interface with the UI in a way that requires a TTY. Most actions in Vagrant that require a TTY have configuration switches to disable this requirement. Please do that or run Vagrant with TTY.
```

*	<b>Sync folder management:</b> Since we have 3 different hosts running on the single machine, the sync was a bit difficult at first. And every time we copy some command, we had to manually fix the indentation. Afterwards, we solved this issue by using vagrant rsync-auto, which automatically sync the changes in your sync folder to your VM in real time. Also, we performed copy of mySync to transfer the files to Jenkins server.

 
*	<b>Memory issues:</b> Since our initial VM configuration for Jenkins server was built on 1024MB of memory, the entire machine crashed once we created another VM inside with checkbox configuration.  Later we used 2048MB to configure the Jenkins VM.

*	<b>Vagrant version issues:</b> Installing package using apt-get downloaded the older vagrant of version which was not compliant with the Ubuntu/trusty32 system. So we had to manually find the package and install it. 

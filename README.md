### A docker container with the release version of GNU Radio (3.8 right now) in the latest Ubuntu 

![Screenshot of two GNU Radios](https://iie.fing.edu.uy/personal/flarroca/wp-content/uploads/sites/12/2020/05/Screenshot_2020-05-13_11-54-20.png)

I have prepared this Dockerfile to be able to migrate my OOT modules to 3.8 and not mess the 3.7 installation of my host PC. The resulting container has the GUI available (so you may use the gnuradio companion and QT widgets) as well as audio. Several packages needed to compile OOTs are also installed and configured. 

How to use it: 

1. Install docker by following [Docker's installation guide](https://docs.docker.com/get-docker/) (though I've tested this on Ubuntu only, so for convenience a direct link to the corresponding [guide for Ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)). 
1. Clone this repo: `git clone https://github.com/git-artes/docker-gnuradio.git`
1. Enter the docker-gnuradio folder and execute `docker build -t ubuntu:gnuradio-releases .` (this step is necessary only once, or every time you modify *Dockerfile*) 
1. Run the container: `docker run --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" --device /dev/snd -v persistent:/home/gnuradio/persistent --group-add=audio -it ubuntu:gnuradio-releases bash`

You will then be in a comand line logged as user *gnuradio* who is a sudoer (password: *gnuradio*). The folder *persistent* in its home directory will persist even after you exit the container. **Remember**: everything else is erased after you exit the container. 

I'm a total newbie using Docker, so feel free to contact me with suggetions. 

**Known problems:**
- When you `sudo` it outputs `sudo: setrlimit(RLIMIT_CORE): Operation not permitted`. You may ignore this. 

IIE Instituto de Ingeniería Eléctrica  
Facultad de Ingeniería  
Universidad de la República  
Montevideo, Uruguay  
http://iie.fing.edu.uy/investigacion/grupos/artes/  
 



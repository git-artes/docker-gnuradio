### A docker container with the release version of GNU Radio (3.10 right now) in the latest stable Ubuntu (plus 3.9 and 3.8 versions in Ubuntu 20.04, and a 3.7 version in Ubuntu 18.04)

![Screenshot of three GNU Radios](https://iie.fing.edu.uy/~flarroca/todos_gnuradio.png)

I have originally prepared these Dockerfiles to be able to migrate my OOT modules to 3.8 and not mess the 3.7 installation of my host PC. The resulting container has the GUI available (so you may use the gnuradio companion and QT widgets) as well as audio. Several packages needed to compile OOTs are also installed and configured. 

If you want the release version of GNU Radio (currently 3.10): 

1. Install docker by following [Docker's installation guide](https://docs.docker.com/get-docker/) (though I've tested this on Ubuntu only, so for convenience a direct link to the corresponding [guide for Ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)). 
1. Clone this repo: `git clone https://github.com/git-artes/docker-gnuradio.git`
1. Enter the `docker-gnuradio/gnuradio-releases` folder and execute `docker build -t ubuntu:gnuradio-releases .` (this step is necessary only once, or every time you modify *Dockerfile*) 
1. Run the container: `docker run --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" --device /dev/snd -v persistent:/home/gnuradio/persistent --group-add=audio -it ubuntu:gnuradio-releases bash`

You will then be in a comand line logged as user *gnuradio* who is a sudoer (password: *gnuradio*). The folder *persistent* in its home directory will persist even after you exit the container. **Remember**: everything else is erased after you exit the container. This means for instance that if you compile and install an OOT you will have to do that again every time you run the container (unless for instance you compile in the *persistent* folder). You may modify Dockerfile to avoid this. 

I've also made a couple of Dockers container for GNU Radio 3.9 and GNU Radio 3.8 (in Ubuntu 20.04), and GNU Radio 3.7 (in Ubuntu 18.04) to check on compatibility issues regarding my OOTs. In the first case, instead of the last two steps above you have to: 

If you want GNU Radio 3.9:

1. Enter the `docker-gnuradio/gnuradio-releases-39` folder and execute `docker build -t ubuntu:gnuradio-releases-3.9 .` (again, this step is only necessary once)
2. Run the container: `docker run --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" --privileged --device /dev/snd -v persistent-39:/home/gnuradio/persistent --group-add=audio -it ubuntu:gnuradio-releases-3.9 bash`. 

If you want GNU Radio 3.8:

1. Enter the `docker-gnuradio/gnuradio-releases-38` folder and execute `docker build -t ubuntu:gnuradio-releases-3.8 .` (again, this step is only necessary once)
2. Run the container: `docker run --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" --privileged --device /dev/snd -v persistent-38:/home/gnuradio/persistent --group-add=audio -it ubuntu:gnuradio-releases-3.8 bash`. 

If you want GNU Radio 3.7:

1. Enter the `docker-gnuradio/gnuradio-releases-37` folder and execute `docker build -t ubuntu:gnuradio-releases-3.7 .` (again, this step is only necessary once)
2. Run the container: `docker run --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" --privileged --device /dev/snd -v persistent-37:/home/gnuradio/persistent --group-add=audio -it ubuntu:gnuradio-releases-3.7 bash`. 

In both cases you will end up with the same situation as before. However, they won't share the `persistent` folder, as I think it doesn't make sense.

I'm a total newbie using Docker, so feel free to contact me with suggestions. Also, I've mostly tested the 3.8 and 3.7 versions.

**Known problems:**
- When you `sudo` it may output `sudo: setrlimit(RLIMIT_CORE): Operation not permitted`. You may ignore this. 
- I've ran into problems with unfound packages during the `apt-get install` part after updating some containers. A quick and dirty solution is to purge all containers through `docker system prune -a` (**warning**: you'll eliminate all your previous containers).

IIE Instituto de Ingeniería Eléctrica  
Facultad de Ingeniería  
Universidad de la República  
Montevideo, Uruguay  
http://iie.fing.edu.uy/investigacion/grupos/artes/  
 

